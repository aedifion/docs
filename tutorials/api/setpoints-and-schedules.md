---
description: Tutorial on writing setpoints and schedules for active building control.
---

# Controls

## Overview

The aedifion Setpoint Writer is a lightweight piece of software capable of running on the [aedifion.device](../../aedifion.io/gateway.md). It allows writing setpoints or whole schedules of many setpoints to any writable datapoint on any building automation component in the attached building network. The semantics of writing setpoints and schedules abstracts from the underlying building networks and automation protocols and thereby provides a unified, fine-grained control of the building's automation components.

## Setpoints vs. schedules

Before we start writing setpoints and schedules, we need to clearly understand what they are and how they differ.

### Setpoints

Setpoints are single one-shot best-effort low-overhead irrevocable write operations. In particular, this implies the following properties of setpoint writing:

* No direct feedback is given about the success or failure of a setpoint write operation.
* No state is kept for a setpoint write operation.
* Existing setpoints \(at the same priority\) are overwritten without any warning.
* A setpoint never expires and is never reset automatically.
* Once sent, a setpoint write operation is immediately executed and cannot be cancelled.
* But: Setpoints are fast and very easy to use.

### Schedules

Schedules are comprised of multiple subsequent setpoint write operations. They are stateful, acknowledged, robust, and modifiable. 

* The state of a schedule and corresponding state transitions are logged and can be queried through the API.
* The current state of the datapoint that is written to is saved before overwriting it.
* Schedules can cancel themselves if a periodic heartbeat is not received.
* Setpoints can be added to a schedule; the existing setpoints can be modified and deleted. All while the schedule is running.
* Schedules can be cancelled before and during execution.
* But: Schedules are more complex to use than bare setpoint write operations.

#### **States and transitions**

The lifecycle of a schedule is defined by five possible states:

1. A new schedules is always created in state `initialized`.
2. When the schedule is syntactically correct and the specified project and data points exists the schedule will transition into state `active` after a few seconds.
3. When a \(critical\) error occurs at any time, the schedule transitions into state `failed` where it has reached end-of-life \(EoL\).
4. When a user cancels a schedule, it first transitions into `stopping` where it awaits acknowledgement from the setpoint writer side and cannot be modified anymore.
5. When the schedule has been completely executed or successfully stopped by the user, it transitions into `terminated` where it has reached EoL.

#### **Structure and execution of a schedule**

A schedule $$S$$ is defined as a series of setpoints, ****i.e.,

$$
S = (s_1, ..., s_n) = ((id_1, t_1, v_1), ..., (id_n, t_n, v_n))
$$

where  $$s_1, ..., s_n$$are setpoints and each single setpoint $$s_i$$ within is defined by a unique id $$id_i$$, a start time $$t_i$$ and its value $$v_i$$.

Setpoints are executed strictly in order of time. If two setpoints have the same start time, their ordering in $$S$$ determines which is executed first. With $$t_1 \leq ... \leq t_n$$ wlog., a schedule thus executes as follows:

* The schedule starts with execution of $$s_1$$ at time $$t_1$$ where $$v_1$$ is written.
* Setpoint $$s_i$$ is written at time $$t_i$$ and overwrites the previous value __$$v_{i-1}$$with $$v_i$$.
  * If $$s_i$$ lies in the past, i.e., $$S$$ is created after $$t_i$$, $$s_i$$ is written immediately.
* The schedule ends with execution of $$s_n$$ \(we refer to this as the _stop event_\) at time $$t_n$$ where $$v_n$$ is written.

### Usage scenarios

Due to the different properties of setpoints and schedules, the user should carefully choose which one to use in his/her scenario. For example:

* Use setpoints to switch on and off components like a human would on a real switch, but use schedules to switch on and off components on predefined points in time like a timer would do.
* Use setpoints to make permanent changes, but use schedules to make temporary changes as schedules leave the system in the same state as before running the schedule.

## API Tutorial

Setpoints and schedules are written and managed through the [HTTP API](../../developers/api-documentation.md). In this section, we go through the steps of unlocking setpoints and schedules for an existing project, configuring selected datapoints for writing, and, finally, writing sample setpoints and schedules.

### Preliminaries

The examples provided in this section partly build on each other. For the sake of brevity, boiler plate code such as imports or variable definitions is only shown once and left out in subsequent examples.

To execute the examples provided in this tutorial, the following is needed:

* A valid login \(username and password\) to the aedifion.io platform. If you do not have a login yet, please [contact us](../../contact.md) regarding a demo login. The login used in the example will not work!
* A project with writable datapoints.
* Optionally, a working installation of [Python](https://www.python.org/) or [Curl](https://curl.haxx.se/).

### Enabling write access

Setpoints and schedules write directly to building automation components and may thus \(deliberately\) complement, interfere with, or completely override existing control and automation. This is a critical operation and thus disabled per default on all projects. Further, there is a series of safeguards in place that limit and protect access to this feature. We go through these safeguards one by one in the following.

#### 1. Activating setpoint and schedules on the project

The setpoints and schedules feature is per default completely locked for all projects. Trying to post a setpoint or schedule results - even with the right roles and permissions \(next step\) - in the following error:

```javascript
{
  "success": false,
  "error": "Project 1 not found or setpoint writing not enabled for project.",
  "operation": "post"
}
```

In order to activate setpoints and schedules for your project, please [contact us](../../contact.md) personally. We will need to know:

* The project \(id\) for which you wish to activate writing.
* Whether you wish to enable setpoints or schedules or both.
* The maximum priority that is allowed for writing setpoints and schedules, e.g., for protocols such as BACnet which support different priorities. Writing at higher priorities will not be possible.

#### 2. Configuring roles and permissions

Writing requires _write permissions_ on datapoints. If you do not have sufficient access, you will receive an error message similar to this one:

```javascript
{
  "success": false,
  "error": "Unauthorized access to endpoint: 39 - POST /v2/datapoint/setpoint"
}
```

Per default, the automatically created _admin_ role of a project has full read/write access on all datapoints. It is, however, strongly advised to set up roles with more restricted write access, e.g., limited to only the necessary datapoints, and not freely assign the _admin_ role to all users just for the sake of simplicity. Please refer to our [administration tutorial](administration.md) on how to set up roles and permissions for projects and datapoints exactly as you need them.

#### 3. Configuring datapoints for writing

In addition to the general feature lock \(step 1\) and the role-based access control \(step 2\), a third safeguard is in place: lower and upper bounds for writing defined individually for each datapoint. Trying to write to a datapoint that has no bounds configured results in this error:

```javascript
{
  "success": false,
  "error": "Datapoint 'bacnet100-4120-Real-room-temperature-setpoint-RTs_real' has no bounds for setpoint writing. Cowardly refusing setpoint schedule - please contact support@aedifion.com",
  "operation": "create"
}
```

Writing bounds can be configured through the `PUT /v2/datapoint` endpoint which requires the following parameters:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Paramater</b>
      </th>
      <th style="text-align:center">Datatype</th>
      <th style="text-align:center">Type</th>
      <th style="text-align:center">Required</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>project_id</b>
      </td>
      <td style="text-align:center">integer</td>
      <td style="text-align:center">query</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The numeric id of the project.</td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>dataPointID</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">query</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The alphanumeric id of the datapoint for which to configure writing bounds.</td>
      <td
      style="text-align:left">bacnet100-4120-Real-room-temperature-setpoint-RTs_real</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>setpoint_</b>
        </p>
        <p><b>min_value</b>
        </p>
      </td>
      <td style="text-align:center">float</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">Lower bound for writing to this datapoint.</td>
      <td style="text-align:left">15</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>setpoint_<br />max_value</b>
      </td>
      <td style="text-align:center">float</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">Upper bound for writing to this datapoint.</td>
      <td style="text-align:left">25</td>
    </tr>
  </tbody>
</table>Here is an example of configuring writing bounds for a room temperature setpoint. A range of 15° to 25° Celsius seems reasonable:

{% tabs %}
{% tab title="Python" %}
```python
import requests
api_url = "https://api.aedifion.io"
john = ("john.doe@aedifion.com", "mys3cr3tp4ssw0rd")
project_id = 1
dataPointID = "bacnet100-4120-Real-room-temperature-setpoint-RTs_real"
datapoint_update = {
    "setpoint_min_value":15,
    "setpoint_max_avlue":25
}
r = requests.put(api_url + "/v2/datapoint",
    params = {"project_id": project_id, "dataPointID": dataPointID}
```
{% endtab %}

{% tab title="Curl" %}
```bash
curl https://api3.aedifion.io/v2/datapoint?project_id=1&dataPointID=bacnet100-4120-Real-room-temperature-setpoint-RTs_real
   -X PUT 
   -u john.doe@aedifion.com:mys3cr3tp4ssw0rd 
   -H 'Content-Type: application/json' 
   -d '{
         "setpoint_max_value": 25, 
         "setpoint_min_value": 15
       }'
```
{% endtab %}
{% endtabs %}

The answer confirms that our request was successful.

```javascript
  "success": 
  "operation": "update",
  "resource": {
    "dataPointID": "bacnet100-4120-Real-room-temperature-setpoint-RTs_real",
    "hash_id": "7fSjkcIH",
    "id": 533,
    "project_id": 1,
    "setpoint_max_value": 25,
    "setpoint_min_value": 15
  }
}
```

Within the configured bounds, writing is now possible while writing any attempt to write outside these bounds results in an error:

```javascript
{
  "success": false,
  "error": "Setpoints for datapoint 'bacnet100-4120-Real-room-temperature-setpoint-RTs_real' are restricted to [15.0,25.0]. Cowardly refusing schedule due to value 30.",
  "operation": "create"
}
```

### Writing single setpoints

Setpoints are written through the `POST /v2/datapoint/setpoint` endpoint. The caller must provide the following parameters:

| **Paramater** | Datatype | Type | Required | Description | Example |
| :--- | :---: | :---: | :---: | :--- | :--- |
| **project\_id** | integer | query | yes | The numeric id of the project. | 1 |
| **dataPointID** | string | query | yes | The alphanumeric id of the datapoint that should be written to.  | bacnet100-4120-Real-room-temperature-setpoint-RTs\_real |
| **value** | string | query | yes | A string containing an integer or float value that should be written to the data point or the special string `null` to clear the value. | 16.7 |
| **priority** | integer | query | no | The priority at which to write the setpoint \(default = 13\). | 15 |

Having [enabled write access](setpoints-and-schedules.md#enabling-write-access), we can now write a setpoint. Please carefully choose a datapoint when executing these examples as you are operating on a real building.

{% tabs %}
{% tab title="Python" %}
```python
import requests
api_url = "https://api.aedifion.io"
john = ("john.doe@aedifion.com", "mys3cr3tp4ssw0rd")
project_id = 1
dataPointID = "bacnet100-4120-Real-room-temperature-setpoint-RTs_real"
r = post(api_url + "/v2/datapoint/setpoint", 
         auth=john
         params={'project_id':1, 'dataPointID':dataPointID, 'value':16.7, 'priority':15})
print(r.text)
```
{% endtab %}

{% tab title="Curl" %}
```bash
curl https://api-dev.aedifion.io/v2/datapoint/setpoint?dataPointID=bacnet100-4120-Real-room-temperature-setpoint-RTs_real&project_id=1&value=16.7&priority=13
    -X POST 
    -u john.doe@aedifion.com
```
{% endtab %}
{% endtabs %}

A successful setpoint write operation \(from point of view of the API\) returns HTTP 200 \(OK\) and a JSON containing the details of the setpoint:

```javascript
{
  "success":true,
  "operation":"create",
  "resource": {
    "action": "writeSingle",
    "dataPointID": "bacnet100-4120-Real-room-temperature-setpoint-RTs_real",
    "priority": 15,
    "value": 16.7
  }
}
```

{% hint style="warning" %}
**Interpreting the response:** For bare setpoint write operations, a _successful response_ \(HTTP 200\) only means that the API accepted the request, i.e., the setpoint writer is correctly configured and the user is authorized for writing to the building. There may still happen errors on the side of the setpoint writer that are not reported back to the API. If a real acknowledgement is required, the user should use schedules instead.
{% endhint %}

We have now set the desired temperature of to 16.7 °C and the HVAC system will probably start working hard to follow your control input. Spin up any existing building management system of your choice and check the effect of your actions.

The written setpoint will remain active until it is cleared or overwritten. Since 16.7 °C is pretty cold for an office, we will soon wish to reset this ourselves and let the existing building automation system regain control. To this end, we need to write `'null'` to the previously written datapoint and priority.

{% tabs %}
{% tab title="Python" %}
```python
r = post(api_url + "/v2/datapoint/setpoint", 
         auth=john,
         params={'project_id':1, 'dataPointID':dataPointID, 'value':'null', 'priority':15})
print(r.text)
```
{% endtab %}
{% endtabs %}

### Creating schedules

Schedules are the generalization of setpoints. They allow you to specify multiple setpoints that are written at predefined points in times. And, schedules will automatically clean-up after themselves, i.e., reset the written datapoint to the same state it was in before the schedule, if you let them.

A new schedule is created through the `POST /v2/datapoint/schedule` endpoint. The caller must identify the datapoint in the query and provide the details of the schedule as a JSON object in the body of the request:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Paramater</b>
      </th>
      <th style="text-align:center">Datatype</th>
      <th style="text-align:center">Type</th>
      <th style="text-align:center">Required</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>project_id</b>
      </td>
      <td style="text-align:center">integer</td>
      <td style="text-align:center">query</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The numeric id of the project.</td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>dataPointID</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">query</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The alphanumeric id of the datapoint on which to create the schedule.</td>
      <td
      style="text-align:left">bacnet100-4120-Real-room-temperature-setpoint-RTs_real</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>name</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">no</td>
      <td style="text-align:left">A human readable name for this schedule.</td>
      <td style="text-align:left">Weekend_override</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>resetValue</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">no</td>
      <td style="text-align:left">The value to write when the schedule finishes or fails. Uses existing
        value before schedule, if not provided.</td>
      <td style="text-align:left">null</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>priority</b>
      </td>
      <td style="text-align:center">integer</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">no</td>
      <td style="text-align:left">The priority at which to write to the datapoint (default = 13).</td>
      <td
      style="text-align:left">15</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>repeat</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">no</td>
      <td style="text-align:left">Repeat interval of this schedule. One of 'hourly', 'daily', 'weekly',
        'monthly', or 'yearly'. <em><b>Coming soon.</b></em>
      </td>
      <td style="text-align:left">'weekly'</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>heartbeat</b>
      </td>
      <td style="text-align:center">integer</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">no</td>
      <td style="text-align:left">Duration in seconds after which the schedule deletes itself if no heartbeat
        is received.</td>
      <td style="text-align:left">60</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>setpoints</b>
      </td>
      <td style="text-align:center">list of dict</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">A list of setpoints. Each setpoint is defined by a unique <code>id</code>,
        a <code>start</code> time, and a <code>value</code>.</td>
      <td style="text-align:left">[{'id':0, 'start':'2018-11-09T18:00:00Z', 'value':18.5}, {'id':1, 'start':'2018-11-12T7:00:00Z',
        'value':21.0}]</td>
    </tr>
  </tbody>
</table>The different options for defining a schedule deserve some more explanation. The example schedule in the above table realizes a very simple weekend override for the office temperature in winter. In detail, it would do the following:

* Operate on datapoint `bacnet100-4120-Real-room-temperature-setpoint-RTs_real` of project `1`at priority `15`.
* Start on 9.11.2018 18:00h \(a Friday afternoon\) by setting the desired room temperature to `18.5` °C \(to save energy when no one is in on the weekend\).
* Continue on 12.11.2018 7:00h \(a Monday morning\) by setting the desired room temperature to `21.0` °C \(to provide thermal comfort during work hours\).
* Repeat this simple schedule `weekly`.
* If no heartbeat is received for `60` seconds, stop the schedule and reset the desired temperature to `null` thereby letting the existing automation regain control.

Let's post the request and inspect the answer. Note that we'll leave out the _repeat_ feature because it is  not supported, yet and also leave out an explicit _resetValue_ so that it is determined automatically.

{% tabs %}
{% tab title="Python" %}
```python
import requests
api_url = "https://api.aedifion.io"
john = ("john.doe@aedifion.com", "mys3cre3tp4ssw0rd")
project_id = 1
dataPointID = "bacnet100-4120-Real-room-temperature-setpoint-RTs_real"
schedule = {
    "name": "Weekend_override",
    "priority": 15,
    "heartbeat": 60,
    "setpoints": [
        {'id':0, 'start':'2018-11-09T18:00:00Z', 'value':'18.5'}, 
        {'id':1, 'start':'2018-11-12T07:00:00Z', 'value':'21.0'}   
    ]
}
r = requests.post(url + "/v2/datapoint/schedule",
         params={'dataPointID':dataPointID, 'project_id':project_id},
         auth=john,
         json=schedule)
print(r.text)
```
{% endtab %}

{% tab title="Curl" %}
Coming soon 🐒
{% endtab %}

{% tab title="Swagger UI" %}
Coming soon 🐒
{% endtab %}
{% endtabs %}

When this request is posted to the API, the API will check \(among permissions\) the syntactic correctness of the request and that no other schedule is active on the same datapoint and priority. If the request is accepted by the API, it is sent to the Setpoint Writer that runs on the aedifion.device in the building network. It is important to note that the API returns immediately after sending the schedule to the Setpoint Writer, i.e., without waiting for a confirmation from the Setpoint Writer. Instead, the Setpoint Writer will asynchronously update the schedule's status on the API at a later point. Creation of schedules is asynchronous because it requires one round of communication with the setpoint writer that may be subject to delay and intermittent connectivity that would heavily slow down or even break synchronous operation.

The reply from the API is similar to the following:

```javascript
{
  "success": true,
  "operation": "create",
  "resource": {
    "reference": "18f86b8a-1669-49da-adc3-e171c8e4e229",
    "name": "Weekend_override",
    "status": "initialized",    
    "datapoint_id": 127,
    "priority": 15,
    "heartbeat": 60,
    "log": "[{\"msg\": \"Schedule created by user.\", \"status\": \"initialized\", \"time\": \"2018-11-08T13:37:40.819719Z\"}]",
    "setpoints": [
      {
        "id": 0,
        "start": "2018-11-09T18:00:00Z",
        "value": "null"
      },
      {
        "id": 1,
        "start": "2018-11-12T07:00:00Z",
        "value": "null"
      }
    ]
  }
}
```

The answer confirms that the schedule has been created and provides further details that deserve a detailed explanation.

* The _status_ of the schedule is in state `initialized` meaning that the schedule has been accepted by the API and sent to the setpoint writer in the building network \(c.f. [Overview](setpoints-and-schedules.md#overview)\). The setpoint writer may still reject the schedule, e.g., if the requested datapoint is non-writable.
* The answer contains a _reference_ to this schedule that can be used with the `GET /v2/datapoint/schedule/{reference}` endpoint to retrieve information about the state of the schedule at a later time. This is necessary since schedules operate on the setpoint writer in the building asynchronously from the aedifion.io platform and API \(c.f. [Overview](setpoints-and-schedules.md#overview)\).
* The answer further contains a JSON-formatted _log_ which is filled by the API and Setpoint Writer with any notable events concerning this schedule. The log is a list of events where each event notes at least the _status_ of the schedule, a human readable message _msg_, and the _time_ of the event.

### **Querying schedules**

When a setpoint is created or executed, the Setpoint Writer updates the schedule's status on the API on each major event, e.g., on activation, on writing a setpoint from the schedule, and any kind of error. It is the user's responsibility to check the status of his/her schedule continuously. To this end, the `GET /v2/datapoint/schedule/{reference}` can be used to which the caller must only provide a single argument:

| **Paramater** | Datatype | Type | Required | Description | Example |
| :--- | :---: | :---: | :---: | :--- | :--- |
| **reference** | string | path | yes | The alphanumeric id of the schedule returned on creation. | 18f86b8a-1669-49da-adc3-e171c8e4e229 |

In the following, we get the schedule but print only its log for the sake of clarity.

{% tabs %}
{% tab title="Python" %}
```python
import json
def printLog(schedule):
    log = json.loads(schedule['log'])
    print("Log:")
    for event in log:
        print(" -> {event['time']} - {event['status']:11s} {event['msg']}")
reference = '18f86b8a-1669-49da-adc3-e171c8e4e229'
r = requests.get(url + f"/v2/datapoint/schedule/{reference}", auth=john)
schedule = r.json()
printLog(schedule)
```
{% endtab %}

{% tab title="Curl" %}
```bash
curl https://api.aedifion.io/v2/datapoint/schedule/18f86b8a-1669-49da-adc3-e171c8e4e229
    -u john.doe@aedifion.com    
```
{% endtab %}
{% endtabs %}

```text
Log:
 -> 2018-11-27T13:37:40.819719Z - initialized Schedule created by user.
 -> 2018-11-27T13:37:41.448276Z - active      Schedule successfully activated on remote logger.
```

Note how a second item has been appended to the log, i.e., the confirmation from the Setpoint Writer that the schedule has been accepted and activated. If we wait for a minute, we will see two more events being appended to the log:

```text
Log:
 -> 2018-11-27T13:37:40.819719Z - initialized Schedule created by user.
 -> 2018-11-27T13:37:41.448276Z - active     Schedule successfully activated on remote logger.
 -> 2018-11-27T13:38:41.588739Z - active     Successfully wrote setpoint (id=reset, value=null, priority = 15).
 -> 2018-11-27T13:38:41.809271Z - failed     Deletion of schedule on missing heartbeat (2 setpoints deleted before execution).
```

What happened? We set the _heartbeat_ for this schedule to 60 seconds but didn't send any heartbeats. Exactly 60 seconds after activating this schedule, the Setpoint Writer thus cancelled this schedule by 

* writing the reset value \(`null` in our case\) which needs to be done since we could have written setpoints before, then
* cancelling all remaining scheduled setpoints, and, finally,
* notifying the API about the failure of this schedule.

This schedule has now reached [End-of-Life](setpoints-and-schedules.md#states-and-transitions): We could query it again and again but nothing would change anymore and attempt to update or delete this schedule will be rejected.

### **Updating and modifying schedules**

Since our first test schedule timed-out, let's create a second with a much higher heartbeat of 600 seconds on which we can calmly try our updates. Please note down the returned reference, e.g., in this example:`390ca8e9-f699-49a4-921f-2746a7eecc5d` Go on and `GET` the schedule to make sure that it has actually been accepted and now is `active`. 

All schedules that are not in state `failed` or `terminated` \(such as our second test schedule\) can be updated and modified through the `PUT /v2/datapoint/schedule/{reference}` endpoint with the following parameters:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Paramater</b>
      </th>
      <th style="text-align:center">Datatype</th>
      <th style="text-align:center">Type</th>
      <th style="text-align:center">Required</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>reference</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">path</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The alphanumeric id of the schedule to modify.</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>name</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">no</td>
      <td style="text-align:left">The new name for the schedule.</td>
      <td style="text-align:left">Weekend_override</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>resetValue</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">no</td>
      <td style="text-align:left">The new reset value of this schedule.</td>
      <td style="text-align:left">20</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>heartbeat</b>
      </td>
      <td style="text-align:center">integer</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">no</td>
      <td style="text-align:left">Duration in seconds after which the schedule deletes itself if no heartbeat
        is received.</td>
      <td style="text-align:left">60</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>setpoints_</b>
        </p>
        <p><b>add</b>
        </p>
      </td>
      <td style="text-align:center">list of dict</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">no</td>
      <td style="text-align:left">A list of <em>new</em> setpoints to add to the schedule. If a new setpoint
        has the same <code>id</code> as an existing setpoint, the update is rejected.</td>
      <td
      style="text-align:left">[{'id':2, 'start':'2018-11-12T9:00:00Z', 'value':20.0}]</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>setpoints_</b>
        </p>
        <p><b>modify</b>
        </p>
      </td>
      <td style="text-align:center">list of dict</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">no</td>
      <td style="text-align:left">A list of <em>existing</em> setpoints to modify identified by their <code>id</code>.
        If a setpoint from this list does not exist, the update is rejected.</td>
      <td
      style="text-align:left">[{'id':0, 'start':'2018-11-09T19:00:00Z', 'value':17}, {'id':1, 'start':'2018-11-12T8:00:00Z',
        'value':22}]</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>setpoints_</b>
        </p>
        <p><b>delete</b>
        </p>
      </td>
      <td style="text-align:center">list of dict</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">no</td>
      <td style="text-align:left">
        <p>A list of <em>existing</em> setpoints to delete identified by their <code>id</code>.</p>
        <p>If a setpoint from this list does not exist, the update is rejected.</p>
      </td>
      <td style="text-align:left">[{'id':2, 'start':'2018-11-12T9:00:00Z', 'value':20}]</td>
    </tr>
  </tbody>
</table>Let's post this update 

TODO

{% tabs %}
{% tab title="Python" %}
```python
import requests

# configuration
... # see above
reference = ... # from create
update_schedule = {...} # fill in update

r = requests.put(url + "/v2/datapoint/schedule/{}".format(reference),
         json=update_schedule,
         auth=auth)
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="Curl" %}
Coming soon 🐒
{% endtab %}

{% tab title="Swagger UI" %}
Coming soon 🐒
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Order of updates:** Updates of setpoints are processed in the following order:

1. New setpoints are added from `setpoints_add`.
2. Setpoints in `setpoints_modify` are modif**i**ed accordingly.
3. Setpoints in `setpoints_delete` are removed.

This order of applying updates means that one could add a new setpoint, modify it, and delete it _within in the same update operation_ \(although this evidently does not make much sense\).
{% endhint %}

### **Deleting schedules**

Any schedule that is not `terminated` or `failed` can be stopped through the `DELETE /v2/datapoint/schedule/{reference}` endpoint which takes a single argument:

| **Paramater** | Datatype | Type | Required | Description | Example |
| :--- | :---: | :---: | :---: | :--- | :--- |
| **reference** | string | path | yes | The alphanumeric id of the schedule returned on creation. | 390ca8e9-f699-49a4-921f-2746a7eecc5d |

{% tabs %}
{% tab title="Python" %}
```python
import requests

# configuration
... # see above
reference = ... # from create or update

r = requests.delete(url + "/v2/datapoint/schedule/{}".format(ref))
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="Curl" %}

{% endtab %}

{% tab title="Matlab" %}

{% endtab %}
{% endtabs %}

On success, the API will return the deleted schedule in the _'resource'_ field of the answer. After deletion, the schedule is either in state `finished` or `terminated` and can still be queried through the `GET /v2/datapoint/schedule/{reference}` endpoint, but cannot be modified or executed anymore.

```javascript
{
  "operation": "delete",
  "resource": {
    "datapoint_id": 127,
    "heartbeat": 3600,
    "log": "[{\"msg\": \"Schedule created by user.\", \"status\": \"initialized\", \"time\": \"2018-11-27T15:10:44.662878Z\"}, {\"msg\": \"Schedule successfully activated on remote logger.\", \"status\": \"active\", \"time\": \"2018-11-27T15:10:45.315786Z\"}, {\"msg\": \"User triggered DELETE /v2/datapoint/schedule/{{reference}}\", \"status\": \"stopping\", \"time\": \"2018-11-27T15:55:38.145546Z\"}]",
    "name": "Weekend_override",
    "priority": 15,
    "reference": "390ca8e9-f699-49a4-921f-2746a7eecc5d",
    "setpoints": [
      {
        "id": 0,
        "start": "2019-11-09T18:00:00Z",
        "value": "null"
      },
      {
        "id": 1,
        "start": "2019-11-12T07:00:00Z",
        "value": "null"
      }
    ],
    "status": "stopping"
  },
  "success": true
}
```

Note that the status of the schedule is now reported as `stopping`. The reason is that, similar as during [creation of a schedule](setpoints-and-schedules.md#creating-schedules), the API will forward the delete request to the Setpoint Writer then return immediately. Just as everything about schedules, deletion is asynchronous and the caller should thus `GET` the schedule a short time after issuing the delete in order to verify that everything worked out as expected.

## Limitations and caveats

#### **Adding, modifying, deleting setpoints in the past**

Updating a running schedule, it is not allowed to add, modify, or delete setpoints that lie in the past in order to prevent inconsistent states. To account for potential communication delays between the API and the setpoint writer, an additional security margin of 60 seconds into the future is enforced. In other words, all setpoints that are added, modified, or deleted within an update, must have a start time that is at least 60 seconds into the future.

#### Deleting stop events

The time-wise last setpoint in a schedule is referred to as stop event. The value that it writes is left in the system. In the 99% of cases, this means that defining `'reset'` as the value of the time-wise last setpoint is the right choice as this will write the value that was written for this datapoint and priority before the schedule went active.

However, when the stop event $$s_n$$is deleted, the latest \(time-wise\) remaining setpoint $$s_i$$automatically becomes the new stop event, including its original value $$v_i$$. Similarly, when a setpoint $$s_m$$ with $$t_m \gt t_n$$ is added $$s_m$$ automatically becomes the new stop event, including its original value $$v_m$$. If $$v_i$$\(or $$v_m$$\) are something other than `'reset'` the system might be unintentionally left in a different state after the schedule than expected by the user.

The bottom line is that the user must take care while adding, modifying, and deleting setpoints from a schedule that the last \(time-wise\) setpoint writes a value that "correctly" resets the system. Again, usually `'reset'` will be the right choice of value for the stop event.

## Planned features

The following features will be released soon.

* _Acknowledged setpoints:_ Introduction of a Boolean parameter `acknowledged` that triggers an asynchronous acknowledgement from the setpoint writer towards the API which can later be queried by the user through an additional API endpoint `GET /v2/datapoint/setpoint/{reference}`
* Pausing and un-pausing a running schedule through additional endpoint `PUT /v2/datapoint/schedule/{reference}/toggle`

Missing a feature? [Request it!](../../contact.md#support)

## FAQ

**What happens if the setpoint writer looses internet connection?**  
Since all current schedules are replicated on the setpoint writer, all schedules continue running without any interruption. However, the setpoint writer can no longer receive heartbeats from the API. Thus, all schedules with a heartbeat will eventually time-out and then be safely reset.

**What happens when the setpoint writer crashes?**  
All schedules \(and their state\) are persisted in a local database on the HD of the setpoint writer. The setpoint writer is set to automatically reboot when it crashes and will reload and reactivate all running schedules \(as long as the HD is intact\). This process takes no more than one minute. As everything, crash events and subsequent reloads are logged to the API.

**Why can't I add, modify, or delete setpoints of a running schedules that lie in the past?**  
The short answer: We do our very best, but what's past is past and nobody can change it.

The long answer: While there may be valid use cases for adding, modifying, or deleting setpoints with a start time in the past, this is deliberately prevented for different reasons: First, there is no intuitive well-defined semantics for changing setpoints in the present that have already been written in the past -- _altering the past_ would almost certainly result in a state of the system that is different to what the user expects. Second, all actions are logged on the API and this log should be an exact transcript of all written setpoints -- adding, modifying, and deleting setpoints in the past would render this log inconsistent with the actual course of events.

**My setpoint or schedule seems to have no effect!**  
A setpoint or schedule operation will have no effect if the data point has already an active value, i.e. a value that is not 'null', written at a higher priority.

**I cannot write at priority higher than 8!**  
In order to preserve life safety mechanism, the API will generally refuse to write at priorities higher than 8 \(= manual operator\). Higher priorities can be unlocked on request to support@aedifion.com and require written consent.

**What are stop events?**  
We refer to the time-wise last element $$s_n$$ in a schedule as the stop event. The stop event defines the state the data point is left in after the schedule has finished. In 99% of the use cases, a user would choose $$v_n =$$ 'reset' which instructs the system to clean up and reset to the initial state \(automatically determined before writing anything\) after finishing with the schedule However, a user may set $$v_n$$ to other values to set the system into a different state than before the schedule, if this is explicitly desired.

