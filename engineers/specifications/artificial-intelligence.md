---
description: >-
  This document contains the list of each class used in the Classes V1
  classification scheme as well as a short description of each of the classes.
---

# Artificial intelligence

1. **Alarm Message \(AL\)**

   This datapoint represents an error in the system. A component might trigger this alarm by switching the signal from 0 to 1. Therefore, the signal is binary. 

2. **Counter \(C\)**

   A datapoint counting events. Counters usually are a monotonous signal counting events inside a component \(e.g. amount of rain, cumulative energy usage\). Some counters do reset at a specified maximum value. 

3. **CO\_2 concentration \(CO2\)**

   This datapoint measures the CO\_2 concentration \(usually in ppm\). The CO\_2 concentration is an indicator for air quality. 

4. **Heat Flow \(HF\)**

   The heat flow between different circuits \(e.g. within AC units\) are annotated with this class. 

5. **Operational Message \(OM\)**

   This datapoint represents messages of components. These messages can represent binary modes \(e.g. on/off, or summer/winter mode\) or multistate modes \(modeA/modeB/modeC\). These datapoints are digital. 

6. **Power \(P\)**

   The current power consumption of a component is annotated with this class. 

7. **Pressure \(pressure\)**

   This datapoint denotes pressure sensors \(e.g. Air pressure from weather stations or pressure of liquids\). 

8. **revolutions \(revs\)**

   Revolutions or frequencies of rotors or pumps are annotated with this class. 

9. **Relative humidity \(rh\_BAS\)**

   A datapoint measuring the relative humidity in the air. The value ranges from 0 to 100 percent. 

10. **Setpoint for operation \(SP\_O\)**

    This datapoint is a user defined set point for an operational mode. For example, a user can switch a component on or off. 

11. **Set Point in percent \(SP\_Percent\)**

    A user defined set point similar to SP\_O for a percental variable \(e.g. valve position\). 

12. **Setpoint of temperature \(SP\_T\)**

    This datapoint is similar to SP\_O. It denotes the output of a controller for temperature. Inside offices the temperature can be regulated \(within constraints\) to maximize the user comfort. 

13. **Setpoint of temperature of potential meter \(SP\_T\_Pot\)**

    This datapoint is similar to SP\_T and also annotates a user defined setpoint for temperature. However, this datapoint is regulated by a potential meter. 

14. **Temperature of gas \(T\_g\)**

    This datapoint shows the temperature of a gas \(e.g. air, …\). It is usually annotated in Degree Celsius, Degree Fahrenheit or Kelvin. 

15. **Temperature of liquid \(T\_l\)**

    This datapoint shows the temperature of a liquid \(e.g. water, …\). It is usually annotated in Degree Celsius, Degree Fahrenheit or Kelvin. 

16. **Volume flow for gases \(V\_dot\_g\)**

    This datapoint denotes the differential flow of a gas \(e.g. air inside AC units\). 

17. **Volume flow for liquids \(V\_dot\_l\)**

    This datapoint is similar to V\_dot\_g. However, this datapoint annotates the differential flow of a liquid \(e.g. water\).

18. **Volatile organic compounds \(VOC\)**

    Volatile organic compounds \(CO\_2, …\) influence the air quality. This datapoint shows a combined concentration of these compounds.

19. **Valve Position \(VP\)**

    This datapoint indicates the position of a valve. Its values ranges from 0 to 100 and show the current close/open status of the underlying valve. 

20. **Electric work \(w\)**

    This datapoint describes the electrical work of a component, I.e. the cumulative electrical power over time. 

21. **Working set point in percent \(WSP\_Percent\)**

    This datapoint denotes a working set point \(i.e. the current target of a control algorithm\) for percentual measurements, e.g. valve positions\). The control algorithm tries to match the actual valve position as close as possible to this working set point. 

22. **Working set point for temperature \(WSP\_T\)**

    This datapoint is similar to WSP\_Percent. However, this datapoint specifies target temperatures for a control algorithm. Its unit is either Degree Celsius, Degree Fahrenheit or Kelvin.

