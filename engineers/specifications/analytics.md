---
description: Detailed information on aedifion.analytics' algorithms
---

# Analytics

## Components

Detailed specification of generic [component data models](../../glossary.md#component-data-model) and available [analysis functions](../../glossary.md#analysis-function) for a [component](../../glossary.md#component).

### boiler

#### Pins

<table>
  <thead>
    <tr>
      <th style="text-align:left">Pin</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Unit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">heat flow</td>
      <td style="text-align:left">Heat flux sensor</td>
      <td style="text-align:left">kW</td>
    </tr>
    <tr>
      <td style="text-align:left">inlet temperature</td>
      <td style="text-align:left">Temperature sensor</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">malfunction message</td>
      <td style="text-align:left">
        <p>Digital datapoint for malfunction shutdown of component.</p>
        <p>True for &quot;malfunction&quot;.</p>
      </td>
      <td style="text-align:left">binary</td>
    </tr>
    <tr>
      <td style="text-align:left">operating message</td>
      <td style="text-align:left">
        <p>Digital datapoint for operation of component.</p>
        <p>True for &quot;on&quot;.</p>
      </td>
      <td style="text-align:left">binary</td>
    </tr>
    <tr>
      <td style="text-align:left">outlet temperature</td>
      <td style="text-align:left">Temperature sensor</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">power consumption</td>
      <td style="text-align:left">Power sensor. Probably gas meter. Power or cumulating counter possible.</td>
      <td
      style="text-align:left">kWh or kW</td>
    </tr>
    <tr>
      <td style="text-align:left">volume flow</td>
      <td style="text-align:left">Volume flow sensor</td>
      <td style="text-align:left">l/s</td>
    </tr>
  </tbody>
</table>#### Available analytics functions

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

<table>
  <thead>
    <tr>
      <th style="text-align:left">Pin</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Unit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">power consumption</td>
      <td style="text-align:left">Power sensor. Probably gas meter. Power or cumulating counter possible.</td>
      <td
      style="text-align:left">kWh or kW</td>
    </tr>
    <tr>
      <td style="text-align:left">inlet temperature</td>
      <td style="text-align:left">Temperature Sensor</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">heat flow</td>
      <td style="text-align:left">Heat flux sensor</td>
      <td style="text-align:left">kW</td>
    </tr>
    <tr>
      <td style="text-align:left">malfunction message</td>
      <td style="text-align:left">
        <p>Digital datapoint for malfunction shutdown of component.</p>
        <p>True for &quot;malfunction&quot;.</p>
      </td>
      <td style="text-align:left">binary</td>
    </tr>
    <tr>
      <td style="text-align:left">operating message</td>
      <td style="text-align:left">
        <p>Digital datapoint for operation of component.</p>
        <p>True for &quot;on&quot;.</p>
      </td>
      <td style="text-align:left">binary</td>
    </tr>
    <tr>
      <td style="text-align:left">outlet temperature</td>
      <td style="text-align:left">Temperature sensor</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">power generation</td>
      <td style="text-align:left">Power sensor of electricity output</td>
      <td style="text-align:left">kW</td>
    </tr>
    <tr>
      <td style="text-align:left">volume flow</td>
      <td style="text-align:left">Volume flow sensor</td>
      <td style="text-align:left">l/s</td>
    </tr>
  </tbody>
</table>#### Available analytics functions

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

#### Pins

#### Available analytics functions

### heat exchanger

#### Pins

#### Available analytics functions

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

<table>
  <thead>
    <tr>
      <th style="text-align:left">Pin</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Unit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">condenser inlet temperature</td>
      <td style="text-align:left">Temperature sensor</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">condenser outlet heat flow</td>
      <td style="text-align:left">Heat flux sensor</td>
      <td style="text-align:left">kW</td>
    </tr>
    <tr>
      <td style="text-align:left">condenser outlet temperature</td>
      <td style="text-align:left">Temperature sensor</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">condenser volume flow</td>
      <td style="text-align:left">Volume flow sensor</td>
      <td style="text-align:left">l/s</td>
    </tr>
    <tr>
      <td style="text-align:left">evaporator inlet heat flow</td>
      <td style="text-align:left">Heat flux sensor</td>
      <td style="text-align:left">kW</td>
    </tr>
    <tr>
      <td style="text-align:left">evaporator inlet temperature</td>
      <td style="text-align:left">Temperature sensor</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">evaporator outlet temperature</td>
      <td style="text-align:left">Temperature sensor</td>
      <td style="text-align:left">&#xB0;C</td>
    </tr>
    <tr>
      <td style="text-align:left">evaporator volume flow</td>
      <td style="text-align:left">Volume flow sensor</td>
      <td style="text-align:left">l/s</td>
    </tr>
    <tr>
      <td style="text-align:left">malfunction message</td>
      <td style="text-align:left">
        <p>Digital datapoint for malfunction shutdown of component.</p>
        <p>True for &quot;malfunction&quot;.</p>
      </td>
      <td style="text-align:left">binary</td>
    </tr>
    <tr>
      <td style="text-align:left">operating message</td>
      <td style="text-align:left">
        <p>Digital datapoint for operation of component.</p>
        <p>True for &quot;on&quot;.</p>
      </td>
      <td style="text-align:left">binary</td>
    </tr>
    <tr>
      <td style="text-align:left">power consumption</td>
      <td style="text-align:left">Power sensor</td>
      <td style="text-align:left">kW</td>
    </tr>
  </tbody>
</table>#### Available analytics functions

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

#### Available analytics functions

### thermal storage

#### Pins

#### Available analytics functions

### thermally activated systems

#### Pins

#### Available analytics functions

### weather station

#### Pins

#### Available analytics functions

## Analytics functions

### efficiency ratio

The efficiency ratio is a KPI defined in by the beneficial energy divided by the energy expenses in an energy conversion:

$$
\eta = Q_{benefit}/P_{expenses}
$$

This KPI is outputted as an overall integral over the analyzed time interval as well a time series with continuous values \(change of value based\).

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
</table>### heat flux

### ordered load duration

### 

## Information

The libraries of component data models and analytics functions are constantly expanded. If you are missing a component and/or analytics function or wish to implement your own functions or want us to implement it, feel free to [contact us](https://docs.aedifion.io/docs/~/drafts/-L_HZNFlbsllp3SXA5Xn/primary/contact).

