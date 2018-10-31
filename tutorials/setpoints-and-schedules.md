---
description: Tutorial on writing setpoints and schedules for active building control.
---

# Setpoints and schedules

## Overview

The aedifion Setpoint Writer is a lightweight piece of software capable of running on a small industrial pc that allows writing setpoints \(or whole schedules consisting of many setpoints\) to any writable datapoint on any building automation component in the attached building network. The semantics of writing setpoints and schedules completely abstracts from the underlying building networks and automation protocols and thereby provides a unified, fine-grained control of the building's automation components.

{% hint style="info" %}
The code examples in this tutorial are configured to work on the main building of the [E.ON ERC](http://www.eonerc.rwth-aachen.de/cms/E-ON-ERC/Das-Center/~jswg/Kontaktliste/lidx/1/). To try them out yourself adapt them to your building or [request demo access](../contact.md#sales).
{% endhint %}

## Setpoints vs. Schedules

### Setpoints

Setpoints are single one-shot best-effort low-overhead irrevocable write operations.

* No direct feedback is given about the success or failure of a setpoint write operation.
* No state is kept for a setpoint write operation.
* Existing setpoints \(at the same priority\) are overwritten without warning.
* A setpoint never expires and is never reset automatically.
* Once sent, a setpoint write operation is immediately executed and cannot be cancelled.
* But: Setpoints are fast and very easy to use.

### Schedules

Schedules are comprised of multiple subsequent setpoint write operations. They are stateful, acknowledged, robust, and modifiable:

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

A schedule $$S$$ is \(almost\) completely defined by a series of single **setpoints** $$s_i$$. A single setpoint within a schedule is defined by

* `id`\[mandatory\]: An integer uniquely identifying this setpoint within the schedule.
* `start`\[mandatory\]: The date-time when the setpoint should be written \(in RFC3339 format, assumed UTC if not timezone is provided\).
* `value`\[mandatory\]: The value to write, either a string-encoded float or the special strings 'null' to clear the value or 'reset' to use this schedules's \(automatically\) defined `resetValue` \(see FAQ below\).

A schedule is thus defined by

$$
S = (s_1, ..., s_n) = ((i_1, t_1, v_1), ..., (i_n, t_n, v_n))
$$

Setpoints are executed strictly in order of time. If two setpoints have the same start time, their ordering in $$S$$ determines which is executed first.

With $$t_1 \leq ... \leq t_n$$ wlog., a schedule thus executes as follows:

* The schedule starts with execution of $$s_1$$ at time $$t_1$$ where $$v_1$$ is written.
* Setpoint $$s_i$$ is written at time $$t_i$$ and overwrites the previous value __$$v_{i-1}$$with $$v_i$$.
  * If $$s_i$$ lies in the past, i.e., $$S$$ is created after $$t_i$$, $$s_i$$ is written immediately.
* The schedule ends with execution of $$s_n$$ \(we refer to this as the _stop event_\) at time $$t_n$$ where $$v_n$$ is written.

{% hint style="info" %}
To reset the system in its state before the schedule, the user should usually set $$v_n =$$ 'reset'.
{% endhint %}

### Usage scenarios

Due to the different properties of setpoints and schedules, the user should carefully choose which one to use in his/her scenario.

For example:

* Use setpoints to switch on and off components like a human would on a real switch, but use schedules to switch on and off components on predefined points in time like a timer switch would do.
* Use setpoints to make permanent changes, but use schedules to make temporary changes as schedules will leave the building automation system in the same state it was in before running the schedule.

## API Tutorial

Setpoints and schedules are written through the [aedifion HTTP API](../developers/api-documentation/).

### Preliminaries

#### Configuring a project for writing

...

#### Configuring a datapoint for writing

...

### Setpoints API

Setpoints are written through the following API endpoint:

```text
POST https://api-dev.aedifion.io/v2/datapoint/setpoint
```

The caller must provide:

* `dataPointID` \[mandatory\]: the alphanumeric identifier of the data point that should be written to
* `project_id` \[mandatory\]: the numeric identifier of the project to which the data point specified by `dataPointID` belongs
* `value` \[mandatory\]: a string containing an integer or float value that should be written to the data point or the special string `null` to clear the value
* `priority` \[optional, default=13\]: the priority at which to write for building networks such as BACnet that support priorities

{% tabs %}
{% tab title="Python" %}
```python
import requests
r = post("https://api.aedifion.io/v2/datapoint/setpoint", 
         auth=("email", "password"),
         params={'dataPointID':'bacnet512-4120L022VEGSHSB_Anlage-L22', 'project_id':4, 'priority':13, 'value':'null'})
print(r.status_code, r.json())         
```
{% endtab %}

{% tab title="Curl" %}
```bash
curl -X POST -u email:password 'https://api-dev.aedifion.io/v2/datapoint/setpoint?dataPointID=bacnet512-4120L022VEGSHSB_Anlage-L22&project_id=4&value=null&priority=13'
```
{% endtab %}

{% tab title="Matlab" %}
Coming soon.
{% endtab %}
{% endtabs %}

A successful setpoint write operation \(from point of view of the API\) will return HTTP 200 and a JSON containing the details of the setpoint, e.g.,

```javascript
{
  "operation":"create",
  "resource":{
               "action":"writeSingle",
               "dataPointID":"bacnet512-4120L022VEGSHSB_Anlage-L22",
               "priority":14,
               "value":"null"
             },
  "success":true
}
```

{% hint style="warning" %}
**Interpreting the response:** For bare setpoint write operations, a _successful response_ only means that the API accepted the request, i.e., the setpoint writer is correctly configured and the user is authorized for writing to the building. There may still happen errors on the side of the setpoint writer that are not reported back to the API. If a real acknowledgement is required, the user should use schedules instead.
{% endhint %}

### Schedules API

#### **Creating, modifying, querying and deleting schedules**

Schedules are created, modified, queried, and deleted through the following API endpoints

```text
POST   https://api-dev.aedifion.io/v2/datapoint/schedule
PUT    https://api-dev.aedifion.io/v2/datapoint/schedule/{reference}
GET    https://api-dev.aedifion.io/v2/datapoint/schedule/{reference}
DELETE https://api-dev.aedifion.io/v2/datapoint/schedule/{reference}
```

#### **Creating schedules**

**Request:** Schedules are created through

```text
POST https://api-dev.aedifion.io/v2/datapoint/schedule
```

The caller can/must provide:

* `dataPointID` \[mandatory\]: the alphanumeric identifier of the data point that should be written to through the schedule
* `project_id` \[mandatory\]: the numeric identifier of the project to which the data point specified by `dataPointID` belongs
* `schedule` \[mandatory\]: The details of the schedule, as follows.
  * `name` \[optional\]: A human readable name for this schedule.
  * `resetValue` \[optional\]: The value to write when the schedule finishes or fails.

    If `None` the `resetValue` will be automatically inferred from the current state of the data point.

  * `priority` \[optional\]: The priority at which to write setpoints contained in the schedule.

    A default value can be configured per building and per datapoint.

  * `heartbeat` \[optional, default=None\]: A duration in seconds after which the schedule deletes itself if no heartbeat from the API is received.
  * `setpoints` \[mandatory\]: A list of setpoints that define this schedule.

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

