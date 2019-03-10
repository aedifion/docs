---
description: >-
  Detailed information on aedifion.analytics' component data models and
  analytics functions.
---

# Analytics

## Components

Detailed specification of generic [component data models](../../glossary.md#component-data-model) and available [analysis functions](../../glossary.md#analysis-function) for a [component](../../glossary.md#component). The pin designation is exactly the same on API query. The components list will extend continuously.

### boiler

#### Pins

| Pin | Description | Unit |
| :--- | :--- | :--- |
| heat flow | Heat flux sensor | kW |
| inlet temperature | Temperature sensor | °C |
| malfunction message | Malfunction shutdown of component: True for "malfunction" | binary |
| operating message | Operation of component: True for "on" | binary |
| outlet temperature | Temperature sensor | °C |
| power consumption | Power sensor. Usually gas meter. Power or cumulating counter possible. | kWh or kW |
| volume flow | Volume flow sensor | l/s |

#### Available analytics functions

<table>
  <thead>
    <tr>
      <th style="text-align:left">Analysis function</th>
      <th style="text-align:left">Specification for component</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">cycle analysis</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">efficiency ratio</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">heat flux</td>
      <td style="text-align:left">Provided heating warmth</td>
    </tr>
    <tr>
      <td style="text-align:left">ordered load duration</td>
      <td style="text-align:left">
        <ul>
          <li>Consumed energy</li>
          <li>Output heat flux</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>### combined heat and power

#### Pins

| Pin | Description | Unit |
| :--- | :--- | :--- |
| power consumption | Power sensor. Usually gas meter. Power or cumulating counter possible. | kWh or kW |
| inlet temperature | Temperature Sensor | °C |
| heat flow | Heat flux sensor | kW |
| malfunction message | Malfunction shutdown of component: True for "malfunction" | binary |
| operating message | Operation of component: True for "on" | binary |
| outlet temperature | Temperature sensor | °C |
| power generation | Power sensor of electricity output | kW |
| volume flow | Volume flow sensor | l/s |

#### Available analytics functions

<table>
  <thead>
    <tr>
      <th style="text-align:left">Analysis function</th>
      <th style="text-align:left">Specification for component</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">cycle analysis</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">heat flux</td>
      <td style="text-align:left">Provided heating warmth</td>
    </tr>
    <tr>
      <td style="text-align:left">ordered load duration</td>
      <td style="text-align:left">
        <ul>
          <li>Consumed energy</li>
          <li>Generated power</li>
          <li>Output heat flux</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>### control loop

Available for liquid based heating/cooling circuits.

#### Pins

| Pin | Description | Unit |
| :--- | :--- | :--- |
| controller output | Valve setpoint | % |
| operating message | Operation of component: True for "on" | binary |
| process value | Temperature sensor | °C |
| setpoint | Temperature setpoint | °C |

#### Available analytics functions

| Analysis function | Specification for component |
| :--- | :--- |
| cycle analysis |  |
| setpoint attainment |  |

### heat flux sensor

#### Pins

| Pin | Description | Unit |
| :--- | :--- | :--- |
| inlet temperature | Temperature sensor | °C |
| outlet temperature | Temperature sensor | °C |
| volume flow | Volume flow sensor | l/s |

#### Available analytics functions

| Analysis function | Specification for component |
| :--- | :--- |
| heat flux |  |

### heat pump

Available as water/water heat pump.

#### Pins

| Pin | Description | Unit |
| :--- | :--- | :--- |
| condenser inlet temperature | Temperature sensor | °C |
| condenser outlet heat flow | Heat flux sensor | kW |
| condenser outlet temperature | Temperature sensor | °C |
| condenser volume flow | Volume flow sensor | l/s |
| evaporator inlet heat flow | Heat flux sensor | kW |
| evaporator inlet temperature | Temperature sensor | °C |
| evaporator outlet temperature | Temperature sensor | °C |
| evaporator volume flow | Volume flow sensor | l/s |
| malfunction message | Malfunction shutdown of component: True for "malfunction" | binary |
| operating message | Operation of component: True for "on" | binary |
| power consumption | Power sensor | kW |

#### Available analytics functions

<table>
  <thead>
    <tr>
      <th style="text-align:left">Analysis function</th>
      <th style="text-align:left">Specification for component</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">cycle analysis</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">cycle analysis interpretation</td>
      <td style="text-align:left">Evaluation, interpretation &amp; recommendation of component operating
        cycles</td>
    </tr>
    <tr>
      <td style="text-align:left">efficiency ratio</td>
      <td style="text-align:left">Condenser heat flux as beneficial heat flux</td>
    </tr>
    <tr>
      <td style="text-align:left">heat flux</td>
      <td style="text-align:left">
        <ul>
          <li>Condenser</li>
          <li>Evaporator</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">ordered load duration</td>
      <td style="text-align:left">
        <ul>
          <li>Condenser outlet heat flux</li>
          <li>Evaporator inlet heat flux</li>
          <li>Power consumption</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>### room

#### Pins

| Pin | Description | Unit |
| :--- | :--- | :--- |
| co2 | CO2 sensor | ppm |
| humidity | Relative humidity sensor | % |
| presence | Presence detection sensor or presence counter | binary or number |
| temperature | Room temperature sensor | °C |
| temperature setpoint | Temperature setpoint | °C |
| voc | Volatile organic compound sensor | ppm |
| window opening | Window opening sensor: True for open | binary |

#### Available analytics functions

| Analysis function | Specification for component |
| :--- | :--- |
| co2 comfort ratio | Comfort up to 1000 ppm CO2 |
| humidity comfort ratio | Comfort range between 30 % and 70 % |

### thermally activated systems

#### Pins

| Pin | Description | Unit |
| :--- | :--- | :--- |
| condensation alert | Alarm message: True for alarm | binary |
| heat flow | Heat flux sensor | kW |
| inlet temperature | Temperature sensor | °C |
| outlet temperature | Temperature sensor | °C |
| volume flow | Volume flow sensor | l/s |

#### Available analytics functions

| Analysis function | Specification for component |
| :--- | :--- |
| condensate limit violation ratio |  |
| heat flux | Heat input |

## Analytics functions

Detailed specification of [analysis functions](../../glossary.md#analysis-function). The analysis function and pin designations are exactly the same on API query. The analysis function list will extend continuously.

### co2 comfort ratio

The co2 comfort ratio is a KPI defined by the time fraction of the total time span analysed of staying below a certain comfort threshold divided by the total time span analysed:

$$
CR_{CO_2} = t_{comfort} / t_{total}
$$

Standard threshold is a $$CO_2$$ __concentration of 1000 ppm.

Required pin: co2

### condensate limit violation ratio

The condensate limit violation ratio is a KPI defined by the time fraction of the total time span analysed where the condensate limit was violated divided by the total time span analysed:

$$
CLV_{alert} = t_{alert} / t_{total}
$$

Required pin: condensation alert

### cycle analysis

The cycle analysis function includes the determination of several related KPIs. An operating cycle is defined by the time a component gets switched on until the next time it gets switched on. Only full cycles within the time span analysed are considered. By implication this means, if a component has not two starting processes within the analysed time span, no full cycle can be analysed. 

KPIs available:

* Cycles per hour: Average number of cycles per hour as decimal number.
* Cycle time: Average, minimum and maximum time of a complete component operation cycle.
* Cycle operation time: Average, minimum and maximum time of operation of the component within the cycles analysed.
* Overall number of cycles: Counted number of completed cycles within analysis time span.
* Overall operating time: Overall sum of operating time of component within analysis time span. Not bound to full cycles.

Required pin: operating message

### efficiency ratio

The efficiency ratio is a KPI defined in by the beneficial energy divided by the energy expenses in an energy conversion:

$$
\eta = Q_{benefit}/P_{expenses}
$$

This KPI is outputted as an overall integral over the analyzed time interval as well a time series with continuous values \(change of value based\).

Required pins:

* of all beneficial heat flows: heat flow **or** inlet temperature, outlet temperature and volume flow
* power consumption

### heat flux

The heat flux analysis function determines a virtual heat flux sensor and its virtual time series from a temperature difference and a volume flow for water \(density $$\rho$$; heat capacity $$c_p$$\):

$$
\dot Q = \rho \cdot c_p \cdot \dot V \cdot (T_{outlet}-T_{inlet})
$$

Pins required:

* inlet temperature
* outlet temperature
* volume flow

### humidity comfort ratio

The humidity comfort ratio is a KPI defined by the time fraction of the total time span analysed of staying within humidity comfort ranges divided by the total time span analysed:

$$
CR_{humidity} = t_{comfort} / t_{total}
$$

Standard humidity comfort range is between 30 % and 70 % relative humidity.

Required pin: humidity

### ordered load duration

The ordered load duration analysis function rearranges a power/heat flux time series from highest to lowest values over time spans in hours \(decimal representation\).

### setpoint attainment

The setpoint attainment KPI determines the time-weighted deviation as absolute integral between the setpoint and process values of a controlled system. Deviations within a tolerance spectrum around the setpoint value are evaluated as zero deviation. Deviations in times of inactive control are evaluated as zero deviation.

Requiered pins:

* process value
* setpoint
* optional: operating message

## Information

The libraries of component data models and analytics functions are constantly expanded. If you are missing a component and/or analytics function or wish to implement your own functions or want us to implement it, feel free to [contact us](https://docs.aedifion.io/docs/~/drafts/-L_HZNFlbsllp3SXA5Xn/primary/contact).

