---
description: Writing protocol specifications.
---

# Writing Protocol

{% hint style="danger" %}
This document is work-in-progress and due to change.
{% endhint %}

## Introduction

### What is SWOP?

SWOP is a recursive acronym for _SWOP Writing Operation Protocol_. Regarding its purpose and goals, we may also call it

* _Simple Writing Operation Protocol_,
* _Standard Writing Operation Protocol_,
* _Safe Writing Operation Protocol_.

SWOP is a lightweight JSON-based protocol for _writing_ to building automation systems. Writing enables actively changing the control of the building, permanently or temporarily, by, e.g.,

* temporarily overwriting a setpoint,
* adding a control strategy for weekend mode operations,
* removing a suboptimal control schedule,
* ...

Actively intervening in building control is serious business. Thus, SWOP's primary maxim is _correctness_. All design goals and choices of SWOP, e.g., intuitivity, simplicity, etc., are made to promote this maxim.

### Why do I need SWOP?

SWOP defines an abstract interface between the issuer of a write operation \(e.g., the user through an API\) and the receiver of this operation, e.g., a special device connected to a building automation network. Thereby, SWOP decouples the issuer and receiver of a write operation.

This is important for different reasons:

{% tabs %}
{% tab title="Deployment models" %}
Through SWOP's decoupling of issuer and receiver, we can pull control logic from local constrained SCADA systems and deploy it in remote systems, e.g., on a central cloud platform. This has many advantages in terms of controllability, maintainability, expressivity, integration.
{% endtab %}

{% tab title="Integration and collaboration" %}
SWOP provides a simple common language between issuer and receiver of a write operation allowing implementations, devices, and platforms of different vendors to talk to each other.
{% endtab %}

{% tab title="Abstraction" %}
SWOP is deliberately agnostic of the underlying automation bus and technology. Thus, control algorithms \(that issue write operations\) can be developed and deployed independently of the underlying automation technology. This fosters generalization, portability and reduces duplication of work.
{% endtab %}
{% endtabs %}

### Scenario

SWOP assumes and applies to the following simple scenario:

* We wish to write to a building, plant, or district, referred to as _target facility_.
* Write operations are initiated by humans, machines, or algorithms through a standard interface referred to as the _API_. The API is the _issuer_ of a write operation. In the most general case, the API should be globally available, e.g., hosted on a central server outside of the target facility's closed local networks.
* An _edge device_ is located at the target facility and connected to the Internet. The edge device is the _receiver_ of the write operation. It is responsible for actually performing the write operation and, for this purpose, is connected to the target facility's _automation network_.
* Typically, the edge device is separated from the Internet through a firewall that filters traffic and prevents connection attempts from the outside. Thus, we assume that communication with the edge device cannot be initiated from the outside, e.g., from the API server, but the edge device can initiate communication with the API server or other servers on the Internet.

Note that the outlined scenario is general enough to cover and apply to a multitude of control scenarios, e.g.,

* cloud-based control, where control logic and APIs are located in the cloud completely separated from the edge device which acts as a pure proxy for writing to the automation bus,
* remote control, where control logic is located on a user's PC and operations are written through a local or cloud-based API,
* on-premise control, where control logic is hosted on the edge device which acts as both the issuer and receiver of write operations for multiple devices and the automation network,
* agent-based control, where control logic is distributed over multiple edge device that control each other via SWOP.

## Design principles and goals

Write operations actively influence actual building operations. Errors can have severe real-life consequences such as increased energy consumption, decreased thermal comfort, worsened indoor climate, damage to automation technology and realty, interference with life-safety mechanisms, and even harm to a person.

For the aforementioned reasons, all design aspects of SWOP consider _correctness_ first and other requirements second. Informally, correctness means that a write operation is executed _exactly as expected_ or an appropriate error is given - no error must go unnoticed and the system must never be in an inconsistent and/or unknown state.

Given that humans are usually a \(most\) frequent source of errors, it is of utmost importance to synchronize notion of _correctness_ with what humans _expect_ as correct behavior.

We aim to achieve this by valuing simplicity, intuitivity, and explicitness over the possibly adverse goals of functionality, efficiency, e.g., communication footprint, brevity, and flexibility.

Other design goals are \(always subordinate to correctness\):

* Robustness: The protocol should be robust to outages and failures and sustain its _correct_ operation as long as possible.
* Agnostic: The protocol should be agnostic of the underlying automation bus and such apply to different automation buses, e.g., BACnet, Modbus, KNX.
* Extensibility: The protocol should allow for future extensions. 
* Flexibility: The protocol should cater to different communication scenarios.

## Protocol specifications

SWOP is a stateful message based protocol, i.e., it exchanges messages of certain types and format in a certain order. The expected messages and order of messages are defined by different high level _flows_.

Currently, SWOP defines the following flows:

* Setpoint Flow

Following flows are in design but not specified yet:

* Schedule Creation Flow
* Schedule Update Flow
* Schedule Deletion Flow

### Flows

#### Setpoint Flow

The setpoint flow defines the type and order of messages for writing a single setpoint.

The usual flow without any errors proceeds as follows \(square brackets mark optional steps\).

| Step | Receiver | Comm. | Issuer |
| :--- | :--- | :---: | :--- |
| 1 |  |  | User provides parameters for write operation. |
| 2 | Receive `CMD` message. | &lt;--`CMD`-- | Initiate the Setpoint flow with `CMD` message. |
| 3 | Write the setpoint. |  |  |
| \[4\] | Acknowledge success or error. | --`ACK`--&gt; | Receive `ACK`. |
| \[5\] |  |  | Update internal operation state. |

If the receiver does not require and acknowledgement \(see `CMD` message format below\), Steps 4 and 5 are omitted.

_Handling a lost `CMD` message_

The receiver may be temporarily unreachable, or, more generally, the `CMD` message may be lost in transit, e.g., when a non-direct transport such as MQTT is used between issuer and receiver. To handle this case, the issuer may choose to repeat his `CMD` message after an appropriate timeout. It is, however, left to the application logic whether the write operation should be retried in this way or aborted altogether. Note that this is thus not a message or object field but must be a parameter of the API call \(which is outside these protocol specifications\).

_Handling a lost `ACK` message_

The issuer may be temporarily unreachable, or, more generally, the `ACK` message may be lost in transit, e.g., when a non-direct transport such as MQTT is used between issuer and receiver. The receiver knows its acknowledgement has been lost if it receives a copy of the `CMD` from the issuer. It then reacts by sending a copy of the lost `ACK` to the issuer.

### Objects and Messages

_Objects_ bundle and group relevant information for write operations, acknowledgements, errors, etc. Objects are never sent directly but must be carried within a message. _Messages_ carry objects and provide further context, e.g., position within a flow.

All objects and messages must be JSON-formatted \[[RFC4627](https://tools.ietf.org/html/rfc4627)\] using UTF-8 encoding \[[RFC3629](https://tools.ietf.org/html/rfc3629)\]. All datetime must be formatted according to \[[RFC3339](https://tools.ietf.org/html/rfc3339)\]. Decimals are separated by points \(not comma\).

The protocol defines the following objects:

* Setpoint \(`SPT`\)

The protocol defines the following messages:

* Base \(`BASE`\)
* Command \(`CMD`\)
* Acknowledgement \(`ACK`\)

{% tabs %}
{% tab title="SPT" %}
The setpoint object contains all information required to write a setpoint. It allows the fields summarized in Table 1.

| Field | Type | Required | Comment |
| :--- | :--- | :--- | :--- |
| `type` | string | yes | Must be set to `SPT`. |
| `datapoint` | string | yes | The identifier of the datapoint to write to. Must be unique on the Edge device. |
| `value` | bool, float, int, string | yes | The value to write. The required type depends on the datapoint to write to. |
| `priority` | int | yes/no \* | The priority to write at, if supported by the automation protocol. |

**Table 1:** Attributes of the Setpoint objects. \*Required if the underlying automation bus supports priorities, e.g., for BACnet. Not required if automation bus has no priority, e.g., for ModBus.

_Explanation of object attributes:_

* The field `type` identifies this object as a setpoint object.
* The field `datapoint` references the datapoint on the building automation network to write to.

  The reference must uniquely identify the target datapoint on the receiver side.

  The write operation must be aborted and an appropriate error returned if the reference is ambiguous.

* The field `value` contains the value that should be written to the given datapoint.

  The type of `value` must be consistent with the type of the target datapoint or allow a _loss-free_ conversion.

  E.g., converting an integer `10` to a float `10.0` is loss-free, while rounding a float `value = 10.3` to an integer `value = 10` is not loss-free.

  If `value` cannot be converted without information loss to the type expected by the target datapoint, the write operation must be aborted and an appropriate error returned.

* The field `priority` contains the priority at which the value should be written.

  For automation protocols, such as BACnet, that support priorities, this field is mandatory.

  For others, `priority` is optional and can be ignored if not supported.

```javascript
{
  "type": "SPT",
  "datapoint": "bacnet93-4120-External-Room-Set-Temperature-RTs",
  "priority": 13,
  "value": 20.3
}
```

**Example 1:** A setpoint object.
{% endtab %}

{% tab title="BASE" %}
The Base Message defines common fields for _all_ messages that are actually send during runs of the SWOP protocol. It should be thought of as an abstract base class for all other message types. Table 2 summarizes its defined fields:

| Field | Type | Required | Comment |
| :--- | :--- | :--- | :--- |
| `type` | string | yes | Type of this message, i.e. one of `CMD`, `ACK`, `ERR`. |
| `protocol_version` | string | no | The SWOP protocol version used. |

**Table 2:** Fields of the Base messages. 

_Explanation of message fields:_

* The field `type` identifies this message as a command, an acknowledgement, or an error message.
* The field `protocol_version` specifies the used version of SWOP. This caters to later versions of the protocol that may not be backwards compatible.
{% endtab %}

{% tab title="CMD" %}
#### Command Message \(extends Base Message\)

A `CMD` message conveys a command from the API to the edge device. A command encapsulates a setpoint \(i.e., the details of what to write\) in an envelope that contains further context \(i.e., the details of how to write\). A `CMD` message may / must provide the fields summarized in Table 2.

| Field | Type | Required | Comment |
| :--- | :--- | :--- | :--- |
| `command` | str | yes | The command to execute. Available commands are: `NEW_SETPOINT`. |
| `detail` | object | yes | A Setpoint object. |
| `acknowledge` | bool | no | Whether this operation must be acknowledged by the edge device to the API. |
| `dry_run` | bool | no | Whether to perform a dry run of the requested action or not. |
| `reference` | string | no | Reference for this operation on the API side. |

**Table 3:** Fields of Command messages \(additional to those of Base message\). 

_Explanation of message fields:_

* The field `command` specifies the desired action. Currently, the only defined command is the writing of a new 

  setpoint. Further commands can be added in future protocol versions.

* The field `detail` contains required additional information to execute this command. In the case of a new 

  setpoint, this field contains a setpoint object.

* The field `acknowledge` specifies whether the issuer expects this write operation to be acknowledged by the receiver.

  The exact scope and extent of the acknowledgement is left to the implementation of the receiver and also depends on the capabilities of the underlying automation network and is further discussed in the section on the Acknowledgement Message.

* Using the field `dry_run`, the issuer may specify that this operation should only be tested but not actually

  written to the target datapoint. The exact scope and extent of the test is left to the implementation of the receiver and also depends on the capabilities of the underlying automation network.

* The field `reference` may be used by the issuer to provide a \(unique\) reference for state of this operation kept 

  on the API side. If this field is given any other message related to this operation must contain this reference.

It is up to the edge- or API-side implementation of the SWOP protocol to set defaults for the non-required field.

```javascript
{
  "reference": "f2d70718-fe44-46bd-a3e0-8c4008749851",
  "success": false,
  "message": "Setpoint could not be written.",
  "detail": {
    "value": "15,3",
    "error": "ValueError: Could not cast to float.",
  }
}
```

**Example 2:** A Command message for a new setpoint.
{% endtab %}

{% tab title="ACK" %}
#### Acknowledgement Message

The `ACK` message conveys an acknowledgement of success or error from the receiver of a write operation to the issuer. An acknowledgement may / must provide the fields summarized in Table 4.

| Field | Type | Required | Comment |
| :--- | :--- | :--- | :--- |
| `reference` | string | yes | Reference for this operation on the API side copied from initial `CMD` message. |
| `success` | bool | yes | Whether the operation was successful or not. |
| `message` | str | no | A human readable error or success message. |
| `detail` | object | no | Information about the success or failure of the referenced write operation. |

**Table 4:** Fields of the Acknowledgement message \(additional to those of Base message\). 

_**Explanation of message fields:**_

* The field `reference` must repeat the reference provided by the API for this operation 

  in the corresponding earlier `CMD` message. The reference allows the API to retrieve and update

  any internal state kept about this operation.

* The field `success` must be set to `true` if and only if all steps of the operation

  could be carried out without error.

* The field `message` should contain a human readable message that can be appended to the operation's log.
* The field `detail` is an arbitrary JSON object with additional details about the success

  or failure of the referenced operation. Examples of things to include are

  * the state of the data point before and/or after the setpoint was written, e.g., its BACnet priority array,
  * the active present value of the data point which was written to,
  * the details of a conversion error that prevented a successful write
  * etc.

```javascript
{
  "reference": "f2d70718-fe44-46bd-a3e0-8c4008749851",
  "success": false,
  "message": "Setpoint could not be written.",
  "detail": {
    "status": "failed",
    "value": "15,3",
    "error": "ValueError: Could not cast to float.",
  }
}
```

**Example 3:** An Acknowledgement message for a conversion error.

```javascript
{
  "reference": "f2d70718-fe44-46bd-a3e0-8c4008749851",
  "success": true,
  "message": "Setpoint successfully written.",
  "detail": {
    "state_before": {
      "priority_array": [
        16, "null", "null", "null", "null", "null", "null", "null", "null", "null", "null",
        "null", "null", 16.4, "null", "null", "null"], 
      "present_value": 16.4
    } 
  }
}
```

**Example 4:** An Acknowledgement message for a successful setpoint write operation.
{% endtab %}
{% endtabs %}

## Message Transport

This section defines how the SWOP protocol is transported between issuer and receiver. To this end, SWOP can be carried over several standard protocols such as HTTP and MQTT as described in the following.

### Via HTTP

{% hint style="warning" %}
TODO
{% endhint %}

### Via MQTT

{% hint style="warning" %}
TODO
{% endhint %}

## Limitations

This document provides _protocol specifications_. It thus deliberately does not specify choices that pertain to the implementation of this protocol. This includes especially the following aspects.

### Default values

The protocol specification does not specify default values for optional message fields or object attributes. The following default values are, however, recommended for implementations:

* Base message
  * `protocol_version` should be set and specify the lowest required version.
* Command message
  * `acknowledge` defaults to `false`.
  * `dry_run` defaults to `false`.
  * `reference` defaults to `NULL`.
* Acknowledgement message
  * `message` defaults to `NULL`.
  * `detail` defaults to `NULL`.

#### Additional fields and attributes

Implementations may choose to add custom messages, objects, fields, and attributes \(aka vendor extensions\). Following established conventions on the web, their identifiers should be prefixed with the string `x-` \(fields and attributes\) or `X-` \(objects and messages\).

