---
description: >-
  Detailed specification of available component data models and their
  application.
---

# Component data models

## Available component data models

* [Boiler](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#boiler)
  * [Condensing Boiler](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#condensing-boiler)
  * [Low-Temperature Boiler](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#low-temperature-boiler)
* [Combined Heat and Power](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#combined-heat-and-power)
* [Fan](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#fan)
* [Heat Pump](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#heat-pump)

## Application notes

Hints for a smooth application of [component data models](https://docs.aedifion.io/docs/glossary#component-data-model) and their mapping.

* **1-to-n mapping:** One [datapoint](https://docs.aedifion.io/docs/glossary#datapoint) can be mapped to several [instantiated components](https://docs.aedifion.io/docs/glossary#instanced-component) to allow data models of different granularity.
* **Unit sensitivity:** To this state, our algorithms are unit sensitive. Every [pin ](https://docs.aedifion.io/docs/glossary#pin)and attribute is specified with an unit. Note the specifications.

{% hint style="danger" %}
If unit conventions are disregarded, this can lead to errors and even misleading results of algorithms.
{% endhint %}

* **Incomplete mapping:** [Pins ](https://docs.aedifion.io/docs/glossary#pin)and attributes, are placeholders which might or might not be [mapped ](https://docs.aedifion.io/docs/glossary#mapping)to data. Algorithms will work on incomplete mapped components, they require mapping for specific placeholders though. Check the [algorithm documentation](https://docs.aedifion.io/docs/engineers/specifications/analytics) for required mappings.

## Example

This example leads through our description of [component data models](https://docs.aedifion.io/docs/glossary#component-data-model) by providing exemplary values and descriptions. The first paragraph of a component data model provides information regarding the component and the data model itself, special features, exceptions and so forth.

{% tabs %}
{% tab title="Component Identifier" %}
### example\_component

The **Component Identifier** is the string identifier of the component. It is used as input parameter for some endpoints \([example endpoint](https://api.aedifion.io/ui/#!/Analytics/get_component_analysisfunctions)\).
{% endtab %}

{% tab title="Pins" %}
The pin table lists basic information on each pin of the component.

{% hint style="danger" %}
 **Mind the units.**
{% endhint %}

| Name | Info | Unit |
| :--- | :--- | :--- |
| example pin | Placeholder for any information on the requested datapoint. | kW |
| ... | ... | ... |
{% endtab %}

{% tab title="Attributes" %}
The attributes table lists basic information on each attribute of the component. **Mind the unit.**

| Name | Info | Unit |
| :--- | :--- | :--- |
| example attribute | Placeholder for any information on the requested meta data. | kW |
| ... | ... | ... |
{% endtab %}

{% tab title="Available Functions" %}
The **Available Functions** tab provide a list of functions available for the component data model. For more information click the regarding function. 

### Example Function

Exemplary specification of the function, if applied to this component.

### Another Example Function

Exemplary specification of the function, if applied to this component.
{% endtab %}
{% endtabs %}

## Boiler

The **Boiler** is the basic model of the heat conversion plant boiler. Further component data models like the [Condensing Boiler](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#condensing-boiler) and [Low-Temperature Boiler ](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#low-temperature-boiler)inherit this component_._ Boilers __burn fuels like oil, natural gas, pallets etc. to heat up a heat carrier fluid \(water in general\).

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
### Operating Cycle Analysis

### Schedule Analysis
{% endtab %}
{% endtabs %}

### Condensing Boiler

The **Condensing Boiler** is a specific type of [boiler](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#boiler). One of the flue gas components of fuel combustion is gaseous water. A condensing boiler liquefies the water from the flue gases. The condensate heat released is used to heat the heat carrier fluid.

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
### Operating Cycle Analysis

### Schedule Analysis
{% endtab %}
{% endtabs %}

### Low-Temperature Boiler

The **Low-Temperature Boiler** is a specific type of [boiler](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#boiler). It is defined by the low temperature level of the supply temperature output provided by the boiler.

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
### Operating Cycle Analysis

### Schedule Analysis
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
### Operating Cycle Analysis

### Schedule Analysis
{% endtab %}
{% endtabs %}

## Fan

The **Fan** component data model represents various forms of fans.

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
### Operating Cycle Analysis

### Schedule Analysis
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
### Operating Cycle Analysis

### Schedule Analysis
{% endtab %}
{% endtabs %}

