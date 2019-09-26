---
description: >-
  Detailed information on aedifion.analytics' component data models and
  analytics functions.
---

# Analytics

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
\dot Q = \rho \cdot c_p \cdot \dot V \cdot (\vartheta_{outlet}-\vartheta_{inlet})
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

