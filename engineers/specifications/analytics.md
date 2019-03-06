---
description: Detailed information on aedifion.analytics' algorithms
---

# Analytics

## Components

Detailed specification of generic component models and available analysis functions for a component.

### Library

<table>
  <thead>
    <tr>
      <th style="text-align:left">Component</th>
      <th style="text-align:left">Pins</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">heat pump</td>
      <td style="text-align:left">
        <ul>
          <li>condenser inlet temperature</li>
          <li>condenser outlet temperature</li>
          <li>evaporator inlet temperature</li>
          <li>evaporator outlet temperature</li>
          <li>condenser volume flow</li>
          <li>condenser outlet heat</li>
          <li>evaporator volume flow</li>
          <li>evaporator inlet heat</li>
          <li>power consumption</li>
          <li>operating message</li>
          <li>malfunction message</li>
          <li>release</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">control loop</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">energy meter</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">thermally activated system</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">combined heat and power</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">boiler</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">heat exchanger</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">thermal storage</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">room</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">weather station</td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>### Algorithms

<table>
  <thead>
    <tr>
      <th style="text-align:left">Component</th>
      <th style="text-align:left">Available algorithms</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">heat pump</td>
      <td style="text-align:left">
        <ul>
          <li>ordered load duration of
            <ul>
              <li>power consumption</li>
              <li>evaporator inlet heat flux</li>
              <li>condenser outlet heat flux</li>
            </ul>
          </li>
          <li>heat flux determination of
            <ul>
              <li>condenser</li>
              <li>evaporator</li>
            </ul>
          </li>
          <li>cycle analysis
            <ul>
              <li>overall number of cycles</li>
              <li>cycles per hour</li>
              <li>cycle time
                <ul>
                  <li>minimum</li>
                  <li>maximum</li>
                  <li>average</li>
                </ul>
              </li>
              <li>operation time
                <ul>
                  <li>minimum</li>
                  <li>maximum</li>
                  <li>average</li>
                </ul>
              </li>
            </ul>
          </li>
          <li>coefficient of performance</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">control loop</td>
      <td style="text-align:left">
        <ul>
          <li>dummy</li>
          <li>bla</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>## Algorithms

### Coefficient of performance

#### Functionality

The coefficient of performance is a KPI defined in by the beneficial energy divided by the energy expenses in an energy conversion:

$$
\eta = Q_{benefit}/P_{expenses}
$$

#### Output

This KPI can is determined as an overall integral over the analyzed time interval and as a time series with continuous values \(change of value based\).

#### Application

This KPI is a summary of the overall energy efficiency of a component. Therefore, it is applicable on every energy conversion, transfer, storage, or exchange component. A high value represents a good energy efficiency.

### PMV/PDD

* Wirkungsgrad
* Lastdauerlinie

## Information

More control algorithms will be described here. If you wish to implement your own algorithm or want us to implement it, feel free to [contact us](https://docs.aedifion.io/docs/~/drafts/-L_HZNFlbsllp3SXA5Xn/primary/contact).

