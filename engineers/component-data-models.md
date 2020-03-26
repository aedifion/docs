---
description: >-
  Detailed specification of available component data models and their
  application.
---

# Component data models

## Available component data models

* [Boiler](component-data-models.md#boiler)
* [Combined Heat and Power](component-data-models.md#combined-heat-and-power)
* [Fan](component-data-models.md#heat-pump)
* [Heat Meter](component-data-models.md#heat-meter)
* [Heat Pump](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#heat-pump)
* [Room](component-data-models.md#room)
* [Thermal Control Loop](component-data-models.md#thermal-control-loop)
* [Weather Station](component-data-models.md#weather-station)

## Application notes

Hints for a smooth application of [component data models](../glossary.md#component-data-model) and their mapping.

* **1-to-n mapping:** One [datapoint](../glossary.md#datapoint) can be mapped to several [instantiated components](../glossary.md#instanced-component) to allow data models of different granularity.
* **Unit sensitivity:** To this state, our algorithms are unit sensitive. Every [pin ](../glossary.md#pin)and [attribute ](../glossary.md#attribute)is specified with an unit. Mind the specifications.

{% hint style="danger" %}
If unit conventions are disregarded, this can lead to errors and even misleading results of algorithms.
{% endhint %}

* **Incomplete mapping:** [Pins ](../glossary.md#pin)and [attributes](../glossary.md#attribute), are placeholders which might or might not be [mapped ](../glossary.md#mapping)to data. Algorithms will work on incomplete mapped components, they require mapping for specific placeholders though. Check the [algorithm documentation](analytics.md) for required mappings.

## Example Component

This example leads through our description of [component data models](../glossary.md#component-data-model) by providing exemplary values and descriptions. The first paragraph of a component data model provides information regarding the component and the data model itself, special features, exceptions and so forth.

{% tabs %}
{% tab title="Component Identifier" %}
### example\_component

_The **Component Identifier** is the string identifier of the component. It is used as input parameter for some endpoints \(_[_example endpoint_](https://api.aedifion.io/ui/#!/Analytics/get_component_analysisfunctions)_\)._
{% endtab %}

{% tab title="Pins" %}
_The **Pins** table lists basic information on each pin of the component._

{% hint style="danger" %}
 **Mind the units.**
{% endhint %}

| Name | Info | Unit |
| :--- | :--- | :--- |
| example pin | Placeholder for any information on the requested datapoint. | kW |
| ... | ... | ... |
{% endtab %}

{% tab title="Attributes" %}
_The **Attributes** table lists basic information on each attribute of the component._

_The **Key** parameter is the string identifier of the attribute used for our API._

_The **Type** parameter is the value type expected, e.g. float, string, json, etc._

_The **Unit** parameter is the unit of the value expected._

{% hint style="danger" %}
Mind the units.
{% endhint %}

## Example Attribute

_Default Values are implemented within the analytics function themselves, they do not appear on API calls._

Attributes are starting with a short description.

| Key | Type | Default Value | Unit |
| :--- | :--- | :--- | :--- |
| example\_attribute | float or integer | 42.0 | kW |
| ... | ... |  | ... |

## Another Example Attribute

...
{% endtab %}

{% tab title="Available Functions" %}
_The **Available Functions** tab provide a list of functions available for the component data model. For more information click the regarding function._

## [Example Function](analytics.md#example-analysis)

Exemplary specification of the function, if applied to this component.

## [Another Example Function](analytics.md#example-analysis)

Exemplary specification of the function, if applied to this component.
{% endtab %}
{% endtabs %}

## Boiler

The **Boiler** is the component model of the heat conversion plant boiler including subcategories of boilers like condensing boilers.

{% tabs %}
{% tab title="Component Identifier" %}
### **boiler**
{% endtab %}

{% tab title="Pins" %}
{% hint style="danger" %}
Mind the units.
{% endhint %}

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Info</th>
      <th style="text-align:left">Unit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">inlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water) entering the component. Also
        referred to as <b>return temperature</b>
      </td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">operating message</td>
      <td style="text-align:left">
        <p>Informs about operational state of component</p>
        <p>1 = operating</p>
        <p>0 = switched-off</p>
      </td>
      <td style="text-align:left">binary</td>
    </tr>
    <tr>
      <td style="text-align:left">outlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water) exiting the component. Also
        referred to as <b>supply temperature</b>
      </td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">volume flow</td>
      <td style="text-align:left">Volume flow of heat carrier fluid (water)</td>
      <td style="text-align:left">
        <p>default: l/s</p>
        <p>use component attribute to adjust</p>
      </td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Attributes" %}
## Custom Day Schedules

Overwrites basic schedule for specific days with an individual schedule. The json is flexibly expandable for any number of days

| Key | Type | Example Value |
| :--- | :--- | :--- |
| custom\_day\_schedules | json | { "2020-02-28":{"start":"09:00", "end":"18:00"}, "2020-02-29":{"start":"09:00", "end":"18:00"} } |

## Custom Holiday

Overwrites basic schedule for specific days. On a holiday plant operation is considered as unintentional. Add holidays to this parameter

| Key | Type | Example Value |
| :--- | :--- | :--- |
| custom\_holiday | json | \["2020-01-02", "2020-01-28", "2020-04-07"\] |

## Preconditioning

If the basic schedule is inherited from building usage times or opening hours, add a preconditioning attribute and thus a preconditioning period to the start time of the basic schedule. The operation of a particular plant before the start time is therefore evaluated as intended in the scope of the preconditioning time

| Key | Type | Example Value | Unit |
| :--- | :--- | :--- | :--- |
| preconditioning | float \(int is tolerated\) | 30.0 | min |

## Regional Key

  
The regional key is used to automatically load regional holidays and overwrite the basic schedule with holidays accordingly. On a holiday plant operation is considered as unintentional. Utilize regional keys according ISO 3166-2

| Key | Type | Example Value |
| :--- | :--- | :--- |
| regional\_key | string | DE-NW |

## Schedule

Weekly repeated basic schedule the plant is intended to execute. Workday individual schedule. If a workday has none intentional operating times, do not add it to the schedule json

| `Key` | Type | Example Value |
| :--- | :--- | :--- |
| schedule | json | {"Mon":{"start":"10:00", "end":"20:00"},"Tue":{"start":"10:00", "end":"20:00"},"Wed":{"start":"10:00", "end":"20:00"}, "Thu":{"start":"10:00", "end":"20:00"},"Fri":{"start":"10:00", "end":"20:00"},"Sat":{"start":"10:00", "end":"20:00"},"Sun":{"start":"10:00","end":"20:00"}} |

## Schedule Timezone

Timezone of the schedule provided in IANA timezone codes. Default: UTC

| Key | Type | Example Value |
| :--- | :--- | :--- |
| schedule\_timezone | string | Europe/Berlin |

## Shutdown Flexibility

If the basic schedule is inherited from building usage times or opening hours, add a shutdown flexibility attribute and thus a shutdown flexibility period to the end time of the basic schedule. A shutdown of a plant prior to the end of the usage times of the building allows to consume the buffers of conditioned rooms within the limits of comfort or thermal energy of heat storages. The operation of a particular plant before the schedule end time is therefore evaluated as unintended in the scope of the shutdown flexibility

| Key | Type | Example Value | Unit |
| :--- | :--- | :--- | :--- |
| shutdown\_flexibility | float \(int is tolerated\) | 30.0 | min |

## Volume flow unit

This attribute allows to adapt the unit of the volume flow pin.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Key</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Available Values</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">volume_flow_unit</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>litersPerSecond
          <br />litersPerMinute</p>
        <p>litersPerHour
          <br />cubicMetersPerSecond</p>
        <p>cubicMetersPerMinute</p>
        <p>cubicMetersPerHour</p>
      </td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Available Functions" %}
### [Operating Cycle Analysis](analytics.md#operating-cycle-analysis)

### [Reduced Load Analysis](analytics.md#reduced-load-analysis)

### [Schedule Analysis](analytics.md#schedule-analysis)

### [Setpoint Deviation Analysis](analytics.md#setpoint-deviation-analysis)

### [Temperature Spread Analysis](analytics.md#temperature-spread-analysis)

### [ Virtual Heat Meter Analysis](analytics.md#virtual-heat-meter)
{% endtab %}
{% endtabs %}

## Combined Heat and Power

The **Combined Heat and Power** component data model represents various kinds of combined heat and power generation.

{% tabs %}
{% tab title="Component Identifier" %}
### **combined heat and power**
{% endtab %}

{% tab title="Pins" %}
{% hint style="danger" %}
Mind the units.
{% endhint %}

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Info</th>
      <th style="text-align:left">Unit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">inlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water) entering the component. Also
        referred to as <b>return temperature</b>.</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">operating message</td>
      <td style="text-align:left">
        <p>Informs about operational state of component.</p>
        <p>1 = operating</p>
        <p>0 = switched-off</p>
      </td>
      <td style="text-align:left">binary</td>
    </tr>
    <tr>
      <td style="text-align:left">outlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water) exiting the component. Also
        referred to as <b>supply temperature</b>.</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">volume flow</td>
      <td style="text-align:left">Volume flow of heat carrier fluid (water).</td>
      <td style="text-align:left">
        <p>default: l/s</p>
        <p>use component attribute to adjust</p>
      </td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Attributes" %}
## Custom Day Schedules

Overwrites basic schedule for specific days with an individual schedule. The json is flexibly expandable for any number of days

| Key | Type | Example Value |
| :--- | :--- | :--- |
| custom\_day\_schedules | json | { "2020-02-28":{"start":"09:00", "end":"18:00"}, "2020-02-29":{"start":"09:00", "end":"18:00"} } |

## Custom Holiday

Overwrites basic schedule for specific days. On a holiday plant operation is considered as unintentional. Add holidays to this parameter

| Key | Type | Example Value |
| :--- | :--- | :--- |
| custom\_holiday | json | \["2020-01-02", "2020-01-28", "2020-04-07"\] |

## Preconditioning

If the basic schedule is inherited from building usage times or opening hours, add a preconditioning attribute and thus a preconditioning period to the start time of the basic schedule. The operation of a particular plant before the start time is therefore evaluated as intended in the scope of the preconditioning time

| Key | Type | Example Value | Unit |
| :--- | :--- | :--- | :--- |
| preconditioning | float \(int is tolerated\) | 30.0 | min |

## Regional Key

  
The regional key is used to automatically load regional holidays and overwrite the basic schedule with holidays accordingly. On a holiday plant operation is considered as unintentional. Utilize regional keys according ISO 3166-2

| Key | Type | Example Value |
| :--- | :--- | :--- |
| regional\_key | string | DE-NW |

## Schedule

Weekly repeated basic schedule the plant is intended to execute. Workday individual schedule. If a workday has none intentional operating times, do not add it to the schedule json

| `Key` | Type | Example Value |
| :--- | :--- | :--- |
| schedule | json | {"Mon":{"start":"10:00", "end":"20:00"},"Tue":{"start":"10:00", "end":"20:00"},"Wed":{"start":"10:00", "end":"20:00"}, "Thu":{"start":"10:00", "end":"20:00"},"Fri":{"start":"10:00", "end":"20:00"},"Sat":{"start":"10:00", "end":"20:00"},"Sun":{"start":"10:00","end":"20:00"}} |

## Schedule Timezone

Timezone of the schedule provided in IANA timezone codes. Default: UTC

| Key | Type | Example Value |
| :--- | :--- | :--- |
| schedule\_timezone | string | Europe/Berlin |

## Shutdown Flexibility

If the basic schedule is inherited from building usage times or opening hours, add a shutdown flexibility attribute and thus a shutdown flexibility period to the end time of the basic schedule. A shutdown of a plant prior to the end of the usage times of the building allows to consume the buffers of conditioned rooms within the limits of comfort or thermal energy of heat storages. The operation of a particular plant before the schedule end time is therefore evaluated as unintended in the scope of the shutdown flexibility

| Key | Type | Example Value | Unit |
| :--- | :--- | :--- | :--- |
| shutdown\_flexibility | float \(int is tolerated\) | 30.0 | min |

## Volume flow unit

This attribute allows to adapt the unit of the volume flow pin.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Key</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Available Values</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">volume_flow_unit</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>litersPerSecond
          <br />litersPerMinute</p>
        <p>litersPerHour
          <br />cubicMetersPerSecond</p>
        <p>cubicMetersPerMinute</p>
        <p>cubicMetersPerHour</p>
      </td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Available Functions" %}
### [Operating Cycle Analysis](analytics.md#operating-cycle-analysis)

### [Setpoint Deviation Analysis](analytics.md#setpoint-deviation-analysis)

### [Schedule Analysis](analytics.md#schedule-analysis)

### [Temperature Spread Analysis](analytics.md#temperature-spread-analysis)

### [ Virtual Heat Meter Analysis](analytics.md#virtual-heat-meter)
{% endtab %}
{% endtabs %}

## Fan

The **Fan** component data model represents various kinds of fans.

{% tabs %}
{% tab title="Component Identifier" %}
### fan
{% endtab %}

{% tab title="Pins" %}
{% hint style="danger" %}
Mind the units.
{% endhint %}

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Info</th>
      <th style="text-align:left">Unit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">operating message</td>
      <td style="text-align:left">
        <p>Informs about operational state of component</p>
        <p>1 = operating</p>
        <p>0 = switched-off</p>
      </td>
      <td style="text-align:left">binary</td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Attributes" %}
## Custom Day Schedules

Overwrites basic schedule for specific days with an individual schedule. The json is flexibly expandable for any number of days

| Key | Type | Example Value |
| :--- | :--- | :--- |
| custom\_day\_schedules | json | { "2020-02-28":{"start":"09:00", "end":"18:00"}, "2020-02-29":{"start":"09:00", "end":"18:00"} } |

## Custom Holiday

Overwrites basic schedule for specific days. On a holiday plant operation is considered as unintentional. Add holidays to this parameter

| Key | Type | Example Value |
| :--- | :--- | :--- |
| custom\_holiday | json | \["2020-01-02", "2020-01-28", "2020-04-07"\] |

## Preconditioning

If the basic schedule is inherited from building usage times or opening hours, add a preconditioning attribute and thus a preconditioning period to the start time of the basic schedule. The operation of a particular plant before the start time is therefore evaluated as intended in the scope of the preconditioning time

| Key | Type | Example Value | Unit |
| :--- | :--- | :--- | :--- |
| preconditioning | float \(int is tolerated\) | 30.0 | min |

## Regional Key

  
The regional key is used to automatically load regional holidays and overwrite the basic schedule with holidays accordingly. On a holiday plant operation is considered as unintentional. Utilize regional keys according ISO 3166-2

| Key | Type | Example Value |
| :--- | :--- | :--- |
| regional\_key | string | DE-NW |

## Schedule

Weekly repeated basic schedule the plant is intended to execute. Workday individual schedule. If a workday has none intentional operating times, do not add it to the schedule json

| `Key` | Type | Example Value |
| :--- | :--- | :--- |
| schedule | json | {"Mon":{"start":"10:00", "end":"20:00"},"Tue":{"start":"10:00", "end":"20:00"},"Wed":{"start":"10:00", "end":"20:00"}, "Thu":{"start":"10:00", "end":"20:00"},"Fri":{"start":"10:00", "end":"20:00"},"Sat":{"start":"10:00", "end":"20:00"},"Sun":{"start":"10:00","end":"20:00"}} |

## Schedule Timezone

Timezone of the schedule provided in IANA timezone codes. Default: UTC

| Key | Type | Example Value |
| :--- | :--- | :--- |
| schedule\_timezone | string | Europe/Berlin |

## Shutdown Flexibility

If the basic schedule is inherited from building usage times or opening hours, add a shutdown flexibility attribute and thus a shutdown flexibility period to the end time of the basic schedule. A shutdown of a plant prior to the end of the usage times of the building allows to consume the buffers of conditioned rooms within the limits of comfort or thermal energy of heat storages. The operation of a particular plant before the schedule end time is therefore evaluated as unintended in the scope of the shutdown flexibility

| Key | Type | Example Value | Unit |
| :--- | :--- | :--- | :--- |
| shutdown\_flexibility | float \(int is tolerated\) | 30.0 | min |
{% endtab %}

{% tab title="Available Functions" %}
### [Operating Cycle Analysis](analytics.md#operating-cycle-analysis)

### [Schedule Analysis](analytics.md#schedule-analysis)
{% endtab %}
{% endtabs %}

## Heat Meter

The **Heat Meter** component data model represents a heat meter. It can be physically present in the energy system or virtually on the aedifion platform.

{% tabs %}
{% tab title="Component Identifier" %}
### heat meter
{% endtab %}

{% tab title="Pins" %}
{% hint style="danger" %}
Mind the units.
{% endhint %}

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Info</th>
      <th style="text-align:left">Unit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">inlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water) entering the heat meter</td>
      <td
      style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">outlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water) exiting the heat meter</td>
      <td
      style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">volume flow</td>
      <td style="text-align:left">Volume flow of heat carrier fluid (water)</td>
      <td style="text-align:left">
        <p>default: l/s</p>
        <p>use component attribute to adjust</p>
      </td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Attributes" %}
## Volume flow unit

This attribute allows to adapt the unit of the volume flow pin.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Key</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Available Values</th>
      <th style="text-align:left">Unit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">volume_flow_unit</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>litersPerSecond
          <br />litersPerMinute</p>
        <p>litersPerHour
          <br />cubicMetersPerSecond</p>
        <p>cubicMetersPerMinute</p>
        <p>cubicMetersPerHour</p>
      </td>
      <td style="text-align:left">None</td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Available Functions" %}
### [ Virtual Heat Meter Analysis](analytics.md#virtual-heat-meter)
{% endtab %}
{% endtabs %}

## Heat Pump

The **Heat Pump** component data model is representative for components which are able to raise the temperature level between two heat carrier loops \(water/water\) via thermal compression.

{% tabs %}
{% tab title="Component Identifier" %}
### **heat pump**
{% endtab %}

{% tab title="Pins" %}
{% hint style="danger" %}
Mind the units.
{% endhint %}

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Info</th>
      <th style="text-align:left">Unit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">condenser inlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water) entering the component. Also
        referred to as <b>return temperature</b>. Condenser side</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">condenser outlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water) exiting the component. Also
        referred to as <b>supply temperature</b>. Condenser side</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">condenser volume flow</td>
      <td style="text-align:left">Volume flow of heat carrier fluid (water). Condenser side.</td>
      <td style="text-align:left">
        <p>default: l/s</p>
        <p>use component attribute to adjust</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">evaporator inlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water) entering the component. Also
        referred to as <b>return temperature</b>. Evaporator side</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">evaporator outlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water) exiting the component. Also
        referred to as <b>supply temperature</b>. Evaporator side</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">evaporator volume flow</td>
      <td style="text-align:left">Volume flow of heat carrier fluid (water). Evaporator side</td>
      <td style="text-align:left">
        <p>default: l/s</p>
        <p>use component attribute to adjust</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">operating message</td>
      <td style="text-align:left">
        <p>Informs about operational state of component</p>
        <p>1 = operating</p>
        <p>0 = switched-off</p>
      </td>
      <td style="text-align:left">binary</td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Attributes" %}
## Custom Day Schedules

Overwrites basic schedule for specific days with an individual schedule. The json is flexibly expandable for any number of days

| Key | Type | Example Value |
| :--- | :--- | :--- |
| custom\_day\_schedules | json | { "2020-02-28":{"start":"09:00", "end":"18:00"}, "2020-02-29":{"start":"09:00", "end":"18:00"} } |

## Custom Holiday

Overwrites basic schedule for specific days. On a holiday plant operation is considered as unintentional. Add holidays to this parameter

| Key | Type | Example Value |
| :--- | :--- | :--- |
| custom\_holiday | json | \["2020-01-02", "2020-01-28", "2020-04-07"\] |

## Preconditioning

If the basic schedule is inherited from building usage times or opening hours, add a preconditioning attribute and thus a preconditioning period to the start time of the basic schedule. The operation of a particular plant before the start time is therefore evaluated as intended in the scope of the preconditioning time

| Key | Type | Example Value | Unit |
| :--- | :--- | :--- | :--- |
| preconditioning | float \(int is tolerated\) | 30.0 | min |

## Regional Key

  
The regional key is used to automatically load regional holidays and overwrite the basic schedule with holidays accordingly. On a holiday plant operation is considered as unintentional. Utilize regional keys according ISO 3166-2

| Key | Type | Example Value |
| :--- | :--- | :--- |
| regional\_key | string | DE-NW |

## Schedule

Weekly repeated basic schedule the plant is intended to execute. Workday individual schedule. If a workday has none intentional operating times, do not add it to the schedule json

| `Key` | Type | Example Value |
| :--- | :--- | :--- |
| schedule | json | {"Mon":{"start":"10:00", "end":"20:00"},"Tue":{"start":"10:00", "end":"20:00"},"Wed":{"start":"10:00", "end":"20:00"}, "Thu":{"start":"10:00", "end":"20:00"},"Fri":{"start":"10:00", "end":"20:00"},"Sat":{"start":"10:00", "end":"20:00"},"Sun":{"start":"10:00","end":"20:00"}} |

## Schedule Timezone

Timezone of the schedule provided in IANA timezone codes. Default: UTC

| Key | Type | Example Value |
| :--- | :--- | :--- |
| schedule\_timezone | string | Europe/Berlin |

## Shutdown Flexibility

If the basic schedule is inherited from building usage times or opening hours, add a shutdown flexibility attribute and thus a shutdown flexibility period to the end time of the basic schedule. A shutdown of a plant prior to the end of the usage times of the building allows to consume the buffers of conditioned rooms within the limits of comfort or thermal energy of heat storages. The operation of a particular plant before the schedule end time is therefore evaluated as unintended in the scope of the shutdown flexibility

| Key | Type | Example Value | Unit |
| :--- | :--- | :--- | :--- |
| shutdown\_flexibility | float \(int is tolerated\) | 30.0 | min |

## Volume flow unit

This attribute allows to adapt the unit of the volume flow pin.

{% hint style="info" %}
The attribute adjusts the volume flow pin unit on the condenser as well as the evaporator side of the heat pump.
{% endhint %}

<table>
  <thead>
    <tr>
      <th style="text-align:left">Key</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Available Values</th>
      <th style="text-align:left">Unit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">volume_flow_unit</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>litersPerSecond
          <br />litersPerMinute</p>
        <p>litersPerHour
          <br />cubicMetersPerSecond</p>
        <p>cubicMetersPerMinute</p>
        <p>cubicMetersPerHour</p>
      </td>
      <td style="text-align:left">None</td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Available Functions" %}
### [Operating Cycle Analysis](analytics.md#operating-cycle-analysis)

### [Setpoint Deviation Analysis](analytics.md#setpoint-deviation-analysis)

### [Schedule Analysis](analytics.md#schedule-analysis)

### [Temperature Spread Analysis](analytics.md#temperature-spread-analysis)

### [ Virtual Heat Meter Analysis](analytics.md#virtual-heat-meter)
{% endtab %}
{% endtabs %}

## Room

The **Room** component data model is the basis component model for rooms.

{% tabs %}
{% tab title="Component Identifier" %}
### room
{% endtab %}

{% tab title="Pins" %}
{% hint style="danger" %}
Mind the units.
{% endhint %}

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Info</th>
      <th style="text-align:left">Unit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">co2</td>
      <td style="text-align:left">CO2 concentration in the room air</td>
      <td style="text-align:left">ppm</td>
    </tr>
    <tr>
      <td style="text-align:left">operating message</td>
      <td style="text-align:left">
        <p>Operating message of room control</p>
        <p>1 = operating</p>
        <p>0 = switched-off</p>
      </td>
      <td style="text-align:left">binary</td>
    </tr>
    <tr>
      <td style="text-align:left">presence</td>
      <td style="text-align:left">
        <p>Presence of one or more persons inside the room</p>
        <p>1 = presence</p>
        <p>0 = no presence</p>
      </td>
      <td style="text-align:left">binary</td>
    </tr>
    <tr>
      <td style="text-align:left">temperature</td>
      <td style="text-align:left">Inside air temperature in the room</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">temperature setpoint</td>
      <td style="text-align:left">Setpoint of the inside air temperature in the room</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Attributes" %}
## Custom Day Schedules

Overwrites basic schedule for specific days with an individual schedule. The json is flexibly expandable for any number of days

| Key | Type | Example Value |
| :--- | :--- | :--- |
| custom\_day\_schedules | json | { "2020-02-28":{"start":"09:00", "end":"18:00"}, "2020-02-29":{"start":"09:00", "end":"18:00"} } |

## Custom Holiday

Overwrites basic schedule for specific days. On a holiday plant operation is considered as unintentional. Add holidays to this parameter

| Key | Type | Example Value |
| :--- | :--- | :--- |
| custom\_holiday | json | \["2020-01-02", "2020-01-28", "2020-04-07"\] |

## Preconditioning

If the basic schedule is inherited from building usage times or opening hours, add a preconditioning attribute and thus a preconditioning period to the start time of the basic schedule. The operation of a particular plant before the start time is therefore evaluated as intended in the scope of the preconditioning time

| Key | Type | Example Value | Unit |
| :--- | :--- | :--- | :--- |
| preconditioning | float \(int is tolerated\) | 30.0 | min |

## Regional Key

  
The regional key is used to automatically load regional holidays and overwrite the basic schedule with holidays accordingly. On a holiday plant operation is considered as unintentional. Utilize regional keys according ISO 3166-2

| Key | Type | Example Value |
| :--- | :--- | :--- |
| regional\_key | string | DE-NW |

## Schedule

Weekly repeated basic schedule the plant is intended to execute. Workday individual schedule. If a workday has none intentional operating times, do not add it to the schedule json

| `Key` | Type | Example Value |
| :--- | :--- | :--- |
| schedule | json | {"Mon":{"start":"10:00", "end":"20:00"},"Tue":{"start":"10:00", "end":"20:00"},"Wed":{"start":"10:00", "end":"20:00"}, "Thu":{"start":"10:00", "end":"20:00"},"Fri":{"start":"10:00", "end":"20:00"},"Sat":{"start":"10:00", "end":"20:00"},"Sun":{"start":"10:00","end":"20:00"}} |

## Schedule Timezone

Timezone of the schedule provided in IANA timezone codes. Default: UTC

| Key | Type | Example Value |
| :--- | :--- | :--- |
| schedule\_timezone | string | Europe/Berlin |

## Shutdown Flexibility

If the basic schedule is inherited from building usage times or opening hours, add a shutdown flexibility attribute and thus a shutdown flexibility period to the end time of the basic schedule. A shutdown of a plant prior to the end of the usage times of the building allows to consume the buffers of conditioned rooms within the limits of comfort or thermal energy of heat storages. The operation of a particular plant before the schedule end time is therefore evaluated as unintended in the scope of the shutdown flexibility

| Key | Type | Example Value | Unit |
| :--- | :--- | :--- | :--- |
| shutdown\_flexibility | float \(int is tolerated\) | 30.0 | min |
{% endtab %}

{% tab title="Available Functions" %}
### [Reduced Load Analysis](analytics.md#reduced-load-analysis)

### [Room Air Quality Analysis](analytics.md#room-air-quality-analysis)

### [Schedule Analysis](analytics.md#schedule-analysis)
{% endtab %}
{% endtabs %}

## Thermal Control Loop

The **Thermal Control Loop** component data model is representative for thermal control loops. It can be utilized to model thermal control loops of the conversion, distribution, and acceptance layer.

{% tabs %}
{% tab title="Component Identifier" %}
### thermal control loop
{% endtab %}

{% tab title="Description" %}

{% endtab %}

{% tab title="Pins" %}
{% hint style="danger" %}
Mind the units.
{% endhint %}

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Info</th>
      <th style="text-align:left">Unit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">inlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water, brine) entering the control
        loop. Also referred to as <b>uncontrolled supply temperature</b> of the control
        loop</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">operating message</td>
      <td style="text-align:left">
        <p>Informs about operational state of component</p>
        <p>1 = operating</p>
        <p>0 = switched-off</p>
      </td>
      <td style="text-align:left">binary</td>
    </tr>
    <tr>
      <td style="text-align:left">outlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water, brine) exiting the control loop.
        Also referred to as <b>controlled supply temperature</b>, or <b>consumer supply temperature</b>
      </td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">outlet temperature setpoint</td>
      <td style="text-align:left">Setpoint temperature of heat carrier fluid (water, brine) exiting the
        control loop. Also referred to as <b>setpoint of supply temperature</b>
      </td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">pump operating message</td>
      <td style="text-align:left">
        <p>Operating message of pump within the control loop</p>
        <p>1 = operating</p>
        <p>0 = switched-off</p>
      </td>
      <td style="text-align:left">binary</td>
    </tr>
    <tr>
      <td style="text-align:left">return temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water, brine) returning from consumer
        circuit. Also referred to as <b>consumer return temperature</b>
      </td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">valve position</td>
      <td style="text-align:left">
        <p>Degree of valve opening</p>
        <p>0 = fully closed</p>
        <p>100 = fully opened</p>
      </td>
      <td style="text-align:left">%</td>
    </tr>
    <tr>
      <td style="text-align:left">valve position setpoint</td>
      <td style="text-align:left">
        <p>Setpoint for degree of valve opening</p>
        <p>0 = fully closed</p>
        <p>100 = fully opened</p>
      </td>
      <td style="text-align:left">%</td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Attributes" %}
## Custom Day Schedules

Overwrites basic schedule for specific days with an individual schedule. The json is flexibly expandable for any number of days

| Key | Type | Example Value |
| :--- | :--- | :--- |
| custom\_day\_schedules | json | { "2020-02-28":{"start":"09:00", "end":"18:00"}, "2020-02-29":{"start":"09:00", "end":"18:00"} } |

## Custom Holiday

Overwrites basic schedule for specific days. On a holiday plant operation is considered as unintentional. Add holidays to this parameter

| Key | Type | Example Value |
| :--- | :--- | :--- |
| custom\_holiday | json | \["2020-01-02", "2020-01-28", "2020-04-07"\] |

## Preconditioning

If the basic schedule is inherited from building usage times or opening hours, add a preconditioning attribute and thus a preconditioning period to the start time of the basic schedule. The operation of a particular plant before the start time is therefore evaluated as intended in the scope of the preconditioning time

| Key | Type | Example Value | Unit |
| :--- | :--- | :--- | :--- |
| preconditioning | float \(int is tolerated\) | 30.0 | min |

## Regional Key

  
The regional key is used to automatically load regional holidays and overwrite the basic schedule with holidays accordingly. On a holiday plant operation is considered as unintentional. Utilize regional keys according ISO 3166-2

| Key | Type | Example Value |
| :--- | :--- | :--- |
| regional\_key | string | DE-NW |

## Schedule

Weekly repeated basic schedule the plant is intended to execute. Workday individual schedule. If a workday has none intentional operating times, do not add it to the schedule json

| `Key` | Type | Example Value |
| :--- | :--- | :--- |
| schedule | json | {"Mon":{"start":"10:00", "end":"20:00"},"Tue":{"start":"10:00", "end":"20:00"},"Wed":{"start":"10:00", "end":"20:00"}, "Thu":{"start":"10:00", "end":"20:00"},"Fri":{"start":"10:00", "end":"20:00"},"Sat":{"start":"10:00", "end":"20:00"},"Sun":{"start":"10:00","end":"20:00"}} |

## Schedule Timezone

Timezone of the schedule provided in IANA timezone codes. Default: UTC

| Key | Type | Example Value |
| :--- | :--- | :--- |
| schedule\_timezone | string | Europe/Berlin |

## Shutdown Flexibility

If the basic schedule is inherited from building usage times or opening hours, add a shutdown flexibility attribute and thus a shutdown flexibility period to the end time of the basic schedule. A shutdown of a plant prior to the end of the usage times of the building allows to consume the buffers of conditioned rooms within the limits of comfort or thermal energy of heat storages. The operation of a particular plant before the schedule end time is therefore evaluated as unintended in the scope of the shutdown flexibility

| Key | Type | Example Value | Unit |
| :--- | :--- | :--- | :--- |
| shutdown\_flexibility | float \(int is tolerated\) | 30.0 | min |
{% endtab %}

{% tab title="Available Functions" %}
### [Control Loop Oscillation Analysis](analytics.md#control-loop-oscillation-analysis)

### [Operating Cycle Analysis](analytics.md#operating-cycle-analysis)

### [Reduced Load Analysis](analytics.md#reduced-load-analysis)

### [Setpoint Deviation Analysis](analytics.md#setpoint-deviation-analysis)

### [Schedule Analysis](analytics.md#schedule-analysis)

### [Synchronized Operation Analysis](analytics.md#synchronized-operation-analysis)

### [Temperature Spread Analysis](analytics.md#temperature-spread-analysis)
{% endtab %}
{% endtabs %}

## Weather Station

The **Weather Station** component data model links weather sensors and correlating data points.

{% tabs %}
{% tab title="Component Identifier" %}
### **weather station**
{% endtab %}

{% tab title="Pins" %}
{% hint style="danger" %}
Mind the units.
{% endhint %}

| Name | Info | Unit |
| :--- | :--- | :--- |
| temperature | Temperature of outside air | °C |
| reference temperature | Reference temperature from online databases which is used for outdoor air temperature sensor checkup | °C |
{% endtab %}

{% tab title="Attributes" %}
## Latitude

Geographical latitude of weather station.

| Key | Type | Example Value | Unit |
| :--- | :--- | :--- | :--- |
| latitude | float | 6.9403 | ° |

## Longitude

Geographical longitude of weather station.

| Key | Type | Example Value | Unit |
| :--- | :--- | :--- | :--- |
| longitude | float | 50.9407 | ° |
{% endtab %}

{% tab title="Available Functions" %}
### [Weather Station Analysis](analytics.md#weather-station-analysis)
{% endtab %}
{% endtabs %}

## Information

The library of component data models is constantly expanded. If you are missing a component data model, or want us to implement it for you, feel free to [contact us](../contact.md#support).

