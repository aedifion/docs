---
description: Detailed information on aedifion.analytics' algorithms
---

# Analytics

## Components

Detailed specification of generic [component data models](../../glossary.md#component-data-model) and available [analysis functions](../../glossary.md#analysis-function) for a [component](../../glossary.md#component).

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
| CO2 | CO2 sensor | ppm |
| humidity | Relative humidity sensor | % |
| presence | Presence detection sensor or presence counter | binary or number |
| temperature | Room temperature sensor | °C |
| temperature setpoint | Temperature setpoint | °C |
| VOC | Volatile organic compound sensor | ppm |
| window opening | Window opening sensor: True for open | binary |

#### Available analytics functions

| Analysis function | Specification for component |
| :--- | :--- |
| CO2 comfort | Comfort up to 1000 ppm CO2 |
| humidity comfort | Comfort range between 30 % and 70 % |

### thermally activated systems

#### Pins

| Pin | Description | Unit |
| :--- | :--- | :--- |
| condensation alarm | Alarm message: True for alarm | binary |
| heat flow | Heat flux sensor | kW |
| inlet temperature | Temperature sensor | °C |
| outlet temperature | Temperature sensor | °C |
| volume flow | Volume flow sensor | l/s |

#### Available analytics functions

| Analysis function | Specification for component |
| :--- | :--- |
| condensate limit violation |  |
| heat flux | Heat input |

## Analytics functions

Required pins... TODO

### 

### CO2 comfort

### condensate limit violation

### cycle analysis

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <ul>
          <li>cycles per hour</li>
          <li>cycle time
            <ul>
              <li>average</li>
              <li>maximum</li>
              <li>minimum</li>
            </ul>
          </li>
          <li>operation time
            <ul>
              <li>average</li>
              <li>maximum</li>
              <li>minimum</li>
            </ul>
          </li>
          <li>overall number of cycles</li>
        </ul>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### efficiency ratio

The efficiency ratio is a KPI defined in by the beneficial energy divided by the energy expenses in an energy conversion:

$$
\eta = Q_{benefit}/P_{expenses}
$$

This KPI is outputted as an overall integral over the analyzed time interval as well a time series with continuous values \(change of value based\).

### heat flux

### humidity comfort

### ordered load duration

### setpoint attainment

## Information

The libraries of component data models and analytics functions are constantly expanded. If you are missing a component and/or analytics function or wish to implement your own functions or want us to implement it, feel free to [contact us](https://docs.aedifion.io/docs/~/drafts/-L_HZNFlbsllp3SXA5Xn/primary/contact).

