---
description: Tutorial on writing setpoints and schedules for active building control.
---

# Setpoints and schedules

## Overview

The aedifion Setpoint Writer is a lightweight piece of software capable of running on the [aedifion.device](../what-is-aedifion.io/aedifion.device.md). It allows writing setpoints or whole schedules of many setpoints to any writable datapoint on any building automation component in the attached building network. The semantics of writing setpoints and schedules abstracts from the underlying building networks and automation protocols and thereby provides a unified, fine-grained control of the building's automation components.

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

Setpoints and schedules are written and managed through the [HTTP API](../developers/api-documentation.md). In this section, we go through the steps of unlocking setpoints and schedules for an existing project, configuring selected datapoints for writing, and, finally, writing sample setpoints and schedules.

### Preliminaries

The examples provided in this section partly build on each other. For the sake of brevity, boiler plate code such as imports or variable definitions is only shown once and left out in subsequent examples.

To execute the examples provided in this tutorial, the following is needed:

* A valid login \(username and password\) to the aedifion.io platform. If you do not have a login yet, please [contact us](../contact.md) regarding a demo login. The login used in the example will not work!
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

In order to activate setpoints and schedules for your project, please [contact us](../contact.md) personally. We will need to know:

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

### Setpoints API

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

### Schedules API

Schedules are the generalization of setpoints. They allow you to specify multiple setpoints that are written at predefined points in times. And, schedules will automatically clean-up after themselves, i.e., reset the written datapoint to the same state it was in before the schedule, if you let them.

#### **Creating schedules**

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
        'monthly', or 'yearly'. <b>Coming soon.</b>
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
      <td style="text-align:left">3600</td>
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
      <td style="text-align:left">[{'id':1, 'start':'2018-11-09T18:00:00Z, 'value':18.5}, {'id':1, 'start':'2018-11-12T7:00:00Z,
        'value':21.0}
        <br />]</td>
    </tr>
  </tbody>
</table>The different options for defining a schedule deserve some more explanation. The example schedule in the above table realizes a very simple weekend override for the office temperature in winter. In detail, it would do the following:

* Operate on datapoint `bacnet100-4120-Real-room-temperature-setpoint-RTs_real` of project `1`at priority `15`.
* Start on 9.11.2018 18:00h \(a Friday afternoon\) by setting the desired room temperature to `18.5` °C \(to save energy when no one is in on the weekend\).
* Continue on 12.11.2018 7:00h \(a Monday morning\) by setting the desired room temperature to `21.0` °C \(to provide thermal comfort during work hours\).
* Repeat this simple schedule `weekly`.
* If no heartbeat is received for `3600` seconds \(one hour\), stop the schedule and reset the desired temperature to `null` thereby letting the existing automation regain control.

**Answer:** On success, the API call returns the created schedule. Each schedule is assigned a random **reference** that is used to query, modify, and delete the created schedule.

{% tabs %}
{% tab title="Python" %}
```python
import requests

# configuration
api_url = "https://api.aedifion.io"
auth = ("user", "password") # fill in email and password
dataPointID = ... # fill in data point identifier
project_id = ... # fill in project id
schedule = { ...} # fill in schedule

r = requests.post(url + "/v2/datapoint/schedule",
         params={'dataPointID':dataPointID, 'project_id':project_id},
         json=schedule,
         auth=auth)
print(r.status_code, r.json()
```
{% endtab %}

{% tab title="Curl" %}

{% endtab %}

{% tab title="Matlab" %}

{% endtab %}
{% endtabs %}

#### **Updating schedules**

**Request:** Schedules are updated through

```text
PUT https://api-dev.aedifion.io/v2/datapoint/setpoint
```

The caller can/must provide:

* `reference` \[mandatory\]: The reference of the schedule assigned during creation.
* `schedule` \[mandatory\]: The details of the updated schedule as follows.
  * `name` \[optional\]: The new human readable name for this schedule.
  * `resetValue` \[optional, default=None\]: The new reset value of this schedule.

    **Note: changing the reset value has only effect on setpoints that are added or modified afterward, but not on the existing and already scheduled setpoints.**

  * `heartbeat` \[optional, default=None\]: The new heartbeat duration in seconds, takes effect immediately.
  * `repeat` \[optional, default=None\]: The new repeat interval, one of 'hourly', 'daily', 'weekly', 'monthly', 'yearly', the schedule is repeated in these intervals.

    **This feature is currently not supported, yet.**

  * `setpoints_add` \[optional\]: A list of _new_ setpoints to add to the schedule.

    If a new setpoint has the same `id` as an existing setpoint, the update is rejected.

  * `setpoints_modify` \[optional\]: A list of _existing_ setpoints to modify identified by their `id`.

    If a setpoint from this list does not exist, the update is rejected.

  * `setpoints_delete` \[optional\]: A list of _existing_ setpoints to delete identified by their `id`.

    If a setpoint from this list does not exist, the update is rejected.

**Answer:** On success, the API returns the updated schedule in the `resource` field of the answer.

{% hint style="warning" %}
**Adding, modifying, deleting setpoints in the past:** Updating a running schedule, it is not allowed to add, modify, or delete setpoints that lie in the past in order to prevent inconsistent states. To account for potential communication delays between the API and the setpoint writer, an additional security margin of 60 seconds into the future is enforced. In other words, all setpoints that are added, modified, or deleted within an update, must have a start time that is at least 60 seconds into the future.
{% endhint %}

{% hint style="info" %}
**Order of updates:** Updates of setpoints are processed in the following order:

1. New setpoints are added from `setpoints_add`.
2. Setpoints in `setpoints_modify` are modif**i**ed accordingly.
3. Setpoints in `setpoints_delete` are removed.

This order of applying updates means that one could add a new setpoint, modify it, and delete it _within in the same update operation_ \(although this evidently does not make much sense\).
{% endhint %}

{% hint style="warning" %}
**Deleting stop events:** When the stop event $$s_n$$ is deleted, the latest \(time-wise\) remaining setpoint $$s_i$$ becomes the new stop event, including its original value $$v_i$$. When a setpoint $$s_m$$ with $$t_m \gt t_n$$ is added, $$s_m$$ becomes the new stop event, including its original value $$v_m$$. The bottom line is that the user must take care while adding, modifying, and deleting setpoints from a schedule that the last \(time-wise\) setpoint "correctly" resets the system. In the 99%, this will mean defining _'reset'_ as the value of the time-wise last setpoint _after_ addition, modification, and deletion of setpoints.
{% endhint %}

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
Coming soon.
{% endtab %}

{% tab title="Matlab" %}
Coming soon.
{% endtab %}
{% endtabs %}

#### **Querying schedules**

**Request:** The current state and details of a schedules are retrieved through

```text
GET https://api-dev.aedifion.io/v2/datapoint/schedule/{reference}
```

The caller can/must provide:

* `reference` \[mandatory\]: The reference of the schedule assigned during creation.

**Answer:** On success, the API will return the JSON encoded schedule.

{% tabs %}
{% tab title="Python" %}
```python
import requests

# configuration
... # see above
reference = ... # from create or update

r = requests.get(url + "/v2/datapoint/schedule/{}".format(ref))
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="Curl" %}
Coming soon.
{% endtab %}

{% tab title="Matlab" %}
Coming soon.
{% endtab %}
{% endtabs %}

**Deleting schedules**

**Request:** A schedule is stopped and deleted through

```text
DELETE https://api-dev.aedifion.io/v2/datapoint/schedule/{reference}
```

The caller can/must provide:

* `reference` \[mandatory\]: The reference of the schedule assigned during creation.

**Answer:** On success, the API will return the deleted schedule in the _'resource'_ field of the answer. After deletion, the schedule is either in state `finished` or `terminated` and can still be queried through the `GET /v2/datapoint/schedule/{reference}` endpoint, but cannot be modified or executed anymore.

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

## Planned features and extensions

The following features will be released soon.

* _Acknowledged setpoints:_ Introduction of a Boolean parameter `acknowledged` that triggers an asynchronous acknowledgement from the setpoint writer towards the API which can later be queried by the user through an additional API endpoint `GET /v2/datapoint/setpoint/{reference}`
* Pausing and un-pausing a running schedule through additional endpoint `PUT /v2/datapoint/schedule/{reference}/toggle`

Missing a feature? [Request it!](../contact.md#support)

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

