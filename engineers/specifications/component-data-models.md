---
description: >-
  Detailed specification of available component data models and their
  application.
---

# Component data models

## Available component data models

* [Boiler](component-data-models.md#boiler)
  * [Condensing Boiler](component-data-models.md#condensing-boiler)
  * [Low-Temperature Boiler](component-data-models.md#low-temperature-boiler)
* [Combined Heat and Power](component-data-models.md#combined-heat-and-power)
* [Fan](component-data-models.md#heat-pump)
* [Heat Pump](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#heat-pump)

## Application notes

Hints for a smooth application of [component data models](../../glossary.md#component-data-model) and their mapping.

* **1-to-n mapping:** One [datapoint](../../glossary.md#datapoint) can be mapped to several [instantiated components](../../glossary.md#instanced-component) to allow data models of different granularity.
* **Unit sensitivity:** To this state, our algorithms are unit sensitive. Every [pin ](../../glossary.md#pin)and [attribute ](../../glossary.md#attribute)is specified with an unit. Mind the specifications.

{% hint style="danger" %}
If unit conventions are disregarded, this can lead to errors and even misleading results of algorithms.
{% endhint %}

* **Incomplete mapping:** [Pins ](../../glossary.md#pin)and [attributes](../../glossary.md#attribute), are placeholders which might or might not be [mapped ](../../glossary.md#mapping)to data. Algorithms will work on incomplete mapped components, they require mapping for specific placeholders though. Check the [algorithm documentation](analytics.md) for required mappings.

## Example Component

This example leads through our description of [component data models](../../glossary.md#component-data-model) by providing exemplary values and descriptions. The first paragraph of a component data model provides information regarding the component and the data model itself, special features, exceptions and so forth.

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

{% hint style="danger" %}
Mind the units.
{% endhint %}

| Name | Info | Unit |
| :--- | :--- | :--- |
| example attribute | Placeholder for any information on the requested meta data. | kW |
| ... | ... | ... |
{% endtab %}

{% tab title="Available Functions" %}
_The **Available Functions** tab provide a list of functions available for the component data model. For more information click the regarding function._

### [Example Function](analytics.md#example-analysis)

Exemplary specification of the function, if applied to this component.



### [Another Example Function](analytics.md#example-analysis)

Exemplary specification of the function, if applied to this component.
{% endtab %}
{% endtabs %}

## Boiler

The **Boiler** is the basic model of the heat conversion plant boiler. Further component data models like the [Condensing Boiler](component-data-models.md#condensing-boiler) and [Low-Temperature Boiler ](component-data-models.md#low-temperature-boiler)inherit this component_._ Boilers __burn fuels like oil, natural gas, pallets etc. to heat up a heat carrier fluid \(water in general\).

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
      <td style="text-align:left">exhaust co</td>
      <td style="text-align:left">Carbon monoxide concentration in exhaust gas.</td>
      <td style="text-align:left">ppm</td>
    </tr>
    <tr>
      <td style="text-align:left">exhaust nox</td>
      <td style="text-align:left">Nitrogen oxide concentration in exhaust gas.</td>
      <td style="text-align:left">ppm</td>
    </tr>
    <tr>
      <td style="text-align:left">exhaust o2</td>
      <td style="text-align:left">Oxygen concentration in exhaust gas.</td>
      <td style="text-align:left">%</td>
    </tr>
    <tr>
      <td style="text-align:left">exhaust temperature</td>
      <td style="text-align:left">Temperature of exhaust gas.</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">heat flow</td>
      <td style="text-align:left">Thermal power transferred to heat carrier fluid (water).</td>
      <td style="text-align:left">kW</td>
    </tr>
    <tr>
      <td style="text-align:left">heat meter</td>
      <td style="text-align:left">Thermal energy transferred to heat carrier fluid (water). Cumulating counter.</td>
      <td
      style="text-align:left">kWh</td>
    </tr>
    <tr>
      <td style="text-align:left">inlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water) entering the component. Also
        referred to as <b>return temperature</b>.</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">malfunction message</td>
      <td style="text-align:left">
        <p>Informs about technical availability of component.</p>
        <p>1 = malfunction</p>
        <p>0 = no malfunction</p>
      </td>
      <td style="text-align:left">binary</td>
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
      <td style="text-align:left">power consumption</td>
      <td style="text-align:left">Fuel power consumed by component.</td>
      <td style="text-align:left">kW</td>
    </tr>
    <tr>
      <td style="text-align:left">power consumption meter</td>
      <td style="text-align:left">Fuel energy consumed. Cumulating counter.</td>
      <td style="text-align:left">kWh</td>
    </tr>
    <tr>
      <td style="text-align:left">volume flow</td>
      <td style="text-align:left">Volume flow of heat carrier fluid (water).</td>
      <td style="text-align:left">l/s</td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Attributes" %}
No attributes defined for this component data model.
{% endtab %}

{% tab title="Available Functions" %}
### [Operating Cycle Analysis](analytics.md#operating-cycle-analysis)

Checks for appropriate cycle behavior and provides recommendation on how to improve.



### [Schedule Analysis](analytics.md#schedule-analysis)

Checks if plant is operated according to a provided schedule.
{% endtab %}
{% endtabs %}

### Condensing Boiler

The **Condensing Boiler** is a specific type of [boiler](component-data-models.md#boiler). One of the flue gas components of fuel combustion is gaseous water. A condensing boiler liquefies the water from the flue gases. The condensate heat released is used to heat the heat carrier fluid.

{% tabs %}
{% tab title="Component Identifier" %}
### **condensing\_boiler**
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
      <td style="text-align:left">exhaust co</td>
      <td style="text-align:left">Carbon monoxide concentration in exhaust gas.</td>
      <td style="text-align:left">ppm</td>
    </tr>
    <tr>
      <td style="text-align:left">exhaust nox</td>
      <td style="text-align:left">Nitrogen oxide concentration in exhaust gas.</td>
      <td style="text-align:left">ppm</td>
    </tr>
    <tr>
      <td style="text-align:left">exhaust o2</td>
      <td style="text-align:left">Oxygen concentration in exhaust gas.</td>
      <td style="text-align:left">%</td>
    </tr>
    <tr>
      <td style="text-align:left">exhaust temperature</td>
      <td style="text-align:left">Temperature of exhaust gas.</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">heat flow</td>
      <td style="text-align:left">Thermal power transferred to heat carrier fluid (water).</td>
      <td style="text-align:left">kW</td>
    </tr>
    <tr>
      <td style="text-align:left">heat meter</td>
      <td style="text-align:left">Thermal energy transferred to heat carrier fluid (water). Cumulating counter.</td>
      <td
      style="text-align:left">kWh</td>
    </tr>
    <tr>
      <td style="text-align:left">inlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water) entering the component. Also
        referred to as <b>return temperature</b>.</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">malfunction message</td>
      <td style="text-align:left">
        <p>Informs about technical availability of component.</p>
        <p>1 = malfunction</p>
        <p>0 = no malfunction</p>
      </td>
      <td style="text-align:left">binary</td>
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
      <td style="text-align:left">power consumption</td>
      <td style="text-align:left">Fuel power consumed by component.</td>
      <td style="text-align:left">kW</td>
    </tr>
    <tr>
      <td style="text-align:left">power consumption meter</td>
      <td style="text-align:left">Fuel energy consumed. Cumulating counter.</td>
      <td style="text-align:left">kWh</td>
    </tr>
    <tr>
      <td style="text-align:left">volume flow</td>
      <td style="text-align:left">Volume flow of heat carrier fluid (water).</td>
      <td style="text-align:left">l/s</td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Attributes" %}
No attributes defined for this component data model.
{% endtab %}

{% tab title="Available Functions" %}
### [Operating Cycle Analysis](analytics.md#operating-cycle-analysis)

Checks for appropriate cycle behavior and provides recommendation on how to improve.



### [Schedule Analysis](analytics.md#schedule-analysis)

Checks if plant is operated according to a provided schedule.
{% endtab %}
{% endtabs %}

### Low-Temperature Boiler

The **Low-Temperature Boiler** is a specific type of [boiler](component-data-models.md#boiler). It is defined by the low temperature level of the supply temperature output provided by the boiler.

{% tabs %}
{% tab title="Component Identifier" %}
### low-temperature\_boiler
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
      <td style="text-align:left">exhaust co</td>
      <td style="text-align:left">Carbon monoxide concentration in exhaust gas.</td>
      <td style="text-align:left">ppm</td>
    </tr>
    <tr>
      <td style="text-align:left">exhaust nox</td>
      <td style="text-align:left">Nitrogen oxide concentration in exhaust gas.</td>
      <td style="text-align:left">ppm</td>
    </tr>
    <tr>
      <td style="text-align:left">exhaust o2</td>
      <td style="text-align:left">Oxygen concentration in exhaust gas.</td>
      <td style="text-align:left">%</td>
    </tr>
    <tr>
      <td style="text-align:left">exhaust temperature</td>
      <td style="text-align:left">Temperature of exhaust gas.</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">heat flow</td>
      <td style="text-align:left">Thermal power transferred to heat carrier fluid (water).</td>
      <td style="text-align:left">kW</td>
    </tr>
    <tr>
      <td style="text-align:left">heat meter</td>
      <td style="text-align:left">Thermal energy transferred to heat carrier fluid (water). Cumulating counter.</td>
      <td
      style="text-align:left">kWh</td>
    </tr>
    <tr>
      <td style="text-align:left">inlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water) entering the component. Also
        referred to as <b>return temperature</b>.</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">malfunction message</td>
      <td style="text-align:left">
        <p>Informs about technical availability of component.</p>
        <p>1 = malfunction</p>
        <p>0 = no malfunction</p>
      </td>
      <td style="text-align:left">binary</td>
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
      <td style="text-align:left">power consumption</td>
      <td style="text-align:left">Fuel power consumed by component.</td>
      <td style="text-align:left">kW</td>
    </tr>
    <tr>
      <td style="text-align:left">power consumption meter</td>
      <td style="text-align:left">Fuel energy consumed. Cumulating counter.</td>
      <td style="text-align:left">kWh</td>
    </tr>
    <tr>
      <td style="text-align:left">volume flow</td>
      <td style="text-align:left">Volume flow of heat carrier fluid (water).</td>
      <td style="text-align:left">l/s</td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Attributes" %}
No attributes defined for this component data model.
{% endtab %}

{% tab title="Available Functions" %}
### [Operating Cycle Analysis](analytics.md#operating-cycle-analysis)

Checks for appropriate cycle behavior and provides recommendation on how to improve.



### [Schedule Analysis](analytics.md#schedule-analysis)

Checks if plant is operated according to a provided schedule.
{% endtab %}
{% endtabs %}

## Combined Heat and Power

The **Combined Heat and Power** component data model represents various kinds of combined heat and power generation. It does not differ in the method of energy conversion.

{% tabs %}
{% tab title="Component Identifier" %}
### **combined\_heat\_and\_power**
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
      <td style="text-align:left">exhaust co</td>
      <td style="text-align:left">Carbon monoxide concentration in exhaust gas.</td>
      <td style="text-align:left">ppm</td>
    </tr>
    <tr>
      <td style="text-align:left">exhaust nox</td>
      <td style="text-align:left">Nitrogen oxide concentration in exhaust gas.</td>
      <td style="text-align:left">ppm</td>
    </tr>
    <tr>
      <td style="text-align:left">exhaust o2</td>
      <td style="text-align:left">Oxygen concentration in exhaust gas.</td>
      <td style="text-align:left">%</td>
    </tr>
    <tr>
      <td style="text-align:left">exhaust temperature</td>
      <td style="text-align:left">Temperature of exhaust gas.</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">heat flow</td>
      <td style="text-align:left">Thermal power transferred to heat carrier fluid (water).</td>
      <td style="text-align:left">kW</td>
    </tr>
    <tr>
      <td style="text-align:left">heat meter</td>
      <td style="text-align:left">Thermal energy transferred to heat carrier fluid (water). Cumulating counter.</td>
      <td
      style="text-align:left">kWh</td>
    </tr>
    <tr>
      <td style="text-align:left">inlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water) entering the component. Also
        referred to as <b>return temperature</b>.</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">malfunction message</td>
      <td style="text-align:left">
        <p>Informs about technical availability of component.</p>
        <p>1 = malfunction</p>
        <p>0 = no malfunction</p>
      </td>
      <td style="text-align:left">binary</td>
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
      <td style="text-align:left">power consumption</td>
      <td style="text-align:left">Fuel power consumed by component.</td>
      <td style="text-align:left">kW</td>
    </tr>
    <tr>
      <td style="text-align:left">power consumption meter</td>
      <td style="text-align:left">Fuel energy consumed. Cumulating counter.</td>
      <td style="text-align:left">kWh</td>
    </tr>
    <tr>
      <td style="text-align:left">power generation</td>
      <td style="text-align:left">Net electrical power generation by component.</td>
      <td style="text-align:left">kW</td>
    </tr>
    <tr>
      <td style="text-align:left">power generation meter</td>
      <td style="text-align:left">Net electricity production. Cumulating counter.</td>
      <td style="text-align:left">kWh</td>
    </tr>
    <tr>
      <td style="text-align:left">volume flow</td>
      <td style="text-align:left">Volume flow of heat carrier fluid (water).</td>
      <td style="text-align:left">l/s</td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Attributes" %}
No attributes defined for this component data model.
{% endtab %}

{% tab title="Available Functions" %}
### [Operating Cycle Analysis](analytics.md#operating-cycle-analysis)

Checks for appropriate cycle behavior and provides recommendation on how to improve.



### [Schedule Analysis](analytics.md#schedule-analysis)

Checks if plant is operated according to a provided schedule.
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
      <td style="text-align:left">differential pressure</td>
      <td style="text-align:left">Pressure increase by fan. static pressure difference.</td>
      <td style="text-align:left">Pa</td>
    </tr>
    <tr>
      <td style="text-align:left">inverter speed</td>
      <td style="text-align:left">Represents fan speed and ventilation partial load.</td>
      <td style="text-align:left">%</td>
    </tr>
    <tr>
      <td style="text-align:left">malfunction message</td>
      <td style="text-align:left">
        <p>Informs about technical availability of component.</p>
        <p>1 = malfunction</p>
        <p>0 = no malfunction</p>
      </td>
      <td style="text-align:left">binary</td>
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
      <td style="text-align:left">power consumption</td>
      <td style="text-align:left">Electrical power consumed by component.</td>
      <td style="text-align:left">kW</td>
    </tr>
    <tr>
      <td style="text-align:left">power consumption meter</td>
      <td style="text-align:left">Electrical energy consumed. Cumulating counter.</td>
      <td style="text-align:left">kWh</td>
    </tr>
    <tr>
      <td style="text-align:left">volume flow</td>
      <td style="text-align:left">Air flow rate.</td>
      <td style="text-align:left">m&#xB3;/h</td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Attributes" %}
No attributes defined for this component data model.
{% endtab %}

{% tab title="Available Functions" %}
### [Operating Cycle Analysis](analytics.md#operating-cycle-analysis)

Checks for appropriate cycle behavior and provides recommendation on how to improve.



### [Schedule Analysis](analytics.md#schedule-analysis)

Checks if plant is operated according to a provided schedule.
{% endtab %}
{% endtabs %}

## Heat Pump

The **Heat Pump** component data model is representative for components which are able to raise the temperature level between two heat carrier loops \(water/water\) via thermal compression.

{% tabs %}
{% tab title="Component Identifier" %}
### **heat\_pump**
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
      <td style="text-align:left">condenser heat flow</td>
      <td style="text-align:left">Thermal power transferred to heat carrier fluid (water). Condenser side.</td>
      <td
      style="text-align:left">kW</td>
    </tr>
    <tr>
      <td style="text-align:left">condenser heat meter</td>
      <td style="text-align:left">Thermal energy transferred to heat carrier fluid (water). Cumulating counter.
        Condenser side.</td>
      <td style="text-align:left">kWh</td>
    </tr>
    <tr>
      <td style="text-align:left">condenser inlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water) entering the component. Also
        referred to as <b>return temperature</b>. Condenser side.</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">condenser outlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water) exiting the component. Also
        referred to as <b>supply temperature</b>. Condenser side.</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">condenser volume flow</td>
      <td style="text-align:left">Volume flow of heat carrier fluid (water). Condenser side.</td>
      <td style="text-align:left">l/s</td>
    </tr>
    <tr>
      <td style="text-align:left">evaporator heat flow</td>
      <td style="text-align:left">Thermal power transferred to heat carrier fluid (water). Condenser side.</td>
      <td
      style="text-align:left">kW</td>
    </tr>
    <tr>
      <td style="text-align:left">evaporator heat meter</td>
      <td style="text-align:left">Thermal energy transferred to heat carrier fluid (water). Cumulating counter.
        Condenser side.</td>
      <td style="text-align:left">kWh</td>
    </tr>
    <tr>
      <td style="text-align:left">evaporator inlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water) entering the component. Also
        referred to as <b>return temperature</b>. Condenser side.</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">evaporator outlet temperature</td>
      <td style="text-align:left">Temperature of heat carrier fluid (water) exiting the component. Also
        referred to as <b>supply temperature</b>. Condenser side.</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">evaporator volume flow</td>
      <td style="text-align:left">Volume flow of heat carrier fluid (water). Condenser side.</td>
      <td style="text-align:left">l/s</td>
    </tr>
    <tr>
      <td style="text-align:left">malfunction message</td>
      <td style="text-align:left">
        <p>Informs about technical availability of component.</p>
        <p>1 = malfunction</p>
        <p>0 = no malfunction</p>
      </td>
      <td style="text-align:left">binary</td>
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
      <td style="text-align:left">power consumption</td>
      <td style="text-align:left">Electrical power consumed by component.</td>
      <td style="text-align:left">kW</td>
    </tr>
    <tr>
      <td style="text-align:left">power consumption meter</td>
      <td style="text-align:left">Electrical energy consumed. Cumulating counter.</td>
      <td style="text-align:left">kWh</td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Attributes" %}
No attributes defined for this component data model.
{% endtab %}

{% tab title="Available Functions" %}
### [Operating Cycle Analysis](analytics.md#operating-cycle-analysis)

Checks for appropriate cycle behavior and provides recommendation on how to improve.



### [Schedule Analysis](analytics.md#schedule-analysis)

Checks if plant is operated according to a provided schedule.
{% endtab %}
{% endtabs %}

## Information

The library of component data models is constantly expanded. If you are missing a component data model, or want us to implement it for you, feel free to [contact us](../../contact.md#support).

