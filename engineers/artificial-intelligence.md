---
description: >-
  The list of classes used in the current classification scheme as well as a
  short description of each of them.
---

# Artificial intelligence

1. `L1_analog_digital`

   1. `/analog`

      The datapoint is providing analog values.

   2. `/digital`

      The datapoint is providing binary \(0 and 1\) values.

   3. `/multi-state`

      The datapoint is providing a set of different discrete values \(e.g. 1,2,3,4\). This is frequently done for status messages where each id represents a status message.

2. `L2_virtual:` 

   1. `/physical` 

      The datapoint reflects a real physical measurement. Replicated datapoints of physical measurements are also physical.

   2. `/virtual`

      The datapoint represents a purely virtual timeseries.

   \`\`

3. `L3_direction:`

   1. `/input`

      Datapoint is an input to a control instance.

   2. `/output`

      Datapoint is an output to a control instance.

   \`\`

4. `L4_type:`

   1. `/counter`

      Accumulating counter. Monotonically increasing.

   2. `/electric/current`

      Measurand for the current

   3. `/electric/frequency`

      Measurand for the utility frequency.

   4. `/electric/power factor`

      Measurand for the power factor \(cos phi\).

   5. `/electric/resistor`

      Measurand for electrical resistance.

   6. `/electric/voltage`

      Measurand for the voltage

   7. `/energy/chill energy`

      Measurand for chill energy e.g. in an AC unit.

   8. `/energy/electrical energy`

      Measurand for electrical energy.

   9. `/energy/heat energy`

      Measurand for heat energy e.g. in a radiator.

   10. `/gas concentration/CO2`

       Measurand for the CO2 concentration.

   11. `/gas concentration/VOC`

       Measurand for the concentration of Volatile Organic compounds.

   12. `/global radiation`

       Global ambient radiation.

   13. `/illumination intensity`

       Measures total luminous flux incident on a surface.

   14. `/message/alarm`

       The datapoint displays error states of a device.

   15. `/message/available`

       A message indicating that a device is ready to be enabled.

   16. `/message/modus`

       A notification on the current operation mode of a device.

   17. `/message/operating`

       The datapoint displays messages about a devices' operation.

   18. `/message/presence`

       A binary notification of a presence sensor.

   19. `/message/switch command`

       A request to turn a device on or off.

   20. `/operating time`

       Datapoint displays the cumulative operation time of a device.

   21. `/parameter/parameter`

       System parameters e.g. maximum valve opening. Basically do not change.

   22. `/parameter/setpoint`

       Set points, e.g. pressure set points.

   23. `/position/damper position`

       Position of air duct dampers

   24. `/position/misc`

       Position of all other actuators.

   25. `/position/valve position`

       Position of a valve.

   26. `/power/electric`

       Electric consumption or production.

   27. `/power/thermal`

       Thermal power, i.e. enthalpy flux or power transferred over a heat exchanger.

   28. `/pressure/gaseous/absolute`

       Measurand of absolute pressure in a gas.

   29. `/pressure/gaseous/differential`

       Measurand of differential pressure in a gas.

   30. `/pressure/liquid/absolute`

       Measurand of absolute pressure in a liquid.

   31. `/pressure/liquid/differential`

       Measurand of differential pressure in a liquid.

   32. `/relative humidity`

       Measurand of relative humidity.

   33. `/rotational speed`

       The datapoint represents a rotational speed of rotating equipment \(e.g. fans or pumps\).

   34. `/temperature/difference`

       This datapoint represents a temperature difference between to locations.

   35. `/temperature/gaseous/exhaust`

       The temperature represents a temperature in a gas leaving the building.

   36. `/temperature/gaseous/extract`

       The temperature represents a temperature in a gas leaving a room.

   37. `/temperature/gaseous/indoor`

       The temperature represents a temperature in a gas inside the building.

   38. `/temperature/gaseous/misc`

       The temperature represents a temperature in a gas which does not fall into any other category.

   39. `/temperature/gaseous/outdoor`

       The temperature represents a temperature in a gas outside or gas coming directly from outside..

   40. `/temperature/gaseous/recirculation`

       The temperature represents a temperature in a gas which is the output of a system and being reintroduced into a system.

   41. `/temperature/gaseous/supply`

       The temperature represents a temperature in a gas flowing into a room or a system.

   42. `/temperature/liquid/cold/input flow`

       The datapoint represents a temperature of a cold input flow of a liquid.

   43. `/temperature/liquid/cold/return flow`

       The datapoint represents a temperature of a cold return flow of a liquid.

   44. `/temperature/liquid/cold/storage tank`

       The datapoint represents a temperature in a cold storage tank for liquid.

   45. `/temperature/liquid/hot/input flow`

       The datapoint represents a temperature of a hot input flow of a liquid.

   46. `/temperature/liquid/hot/return flow`

       The datapoint represents a temperature of a hot return flow of a liquid.

   47. `/temperature/liquid/hot/storage tank`

       The datapoint represents a temperature in a hot storage tank for liquid.

   48. `/temperature/liquid/warm/input flow`

       The datapoint represents a temperature of a warm input flow of a liquid.

   49. `/temperature/liquid/warm/return flow`

       The datapoint represents a temperature of a warm return flow of a liquid.

   50. `/temperature/liquid/warm/storage tank`

       The datapoint represents a temperature in a warm storage tank for liquid.

   51. `/volume flow/gaseous`

       Measurand of a volume flow inside a gas.

   52. `/volume flow/liquid`

       Measurand of a volume flow inside a liquid medium.

   53. `/wind speed`

       Measurand of the windspeed usually from a weather station.

   \`\`

5. `L5_unit:`
   1. `/electric/Ampere`

      The datapoints observations are measured in Ampere.

   2. `/electric/Ohm`

      The datapoints observations are measured in Ohm.

   3. `/electric/Volt`

      The datapoints observations are measured in Volt.

   4. `/energy/Joule`

      The datapoints observations are measured in Joule.

   5. `/energy/KiloBTU`

      The datapoints observations are measured in 1000 British thermal units.

   6. `/energy/KiloWattHours`

      The datapoints observations are measured in Kilowatt hours.

   7. `/energy/TonHours`

      The datapoints observations are measured in Ton hours \(of refrigeration\).

   8. `/energy/WattHours`

      The datapoints observations are measured in Watt hours.

   9. `/frequency/Hertz`

      The datapoints observations are measured in Hertz.

   10. `/frequency/rpm`

       The datapoints observations are measured in revolutions per minute.

   11. `/light/Candela`

       The datapoints observations are measured in Candela.

   12. `/light/Lumen`

       The datapoints observations are measured in Lumen.

   13. `/light/Lux`

       The datapoints observations are measured in Lux.

   14. `/power/KiloBTUsPerHour`

       The datapoints observations are measured in 1000 British thermal units per hour.

   15. `/power/Kilowatt`

       The datapoints observations are measured in Kilowatt.

   16. `/power/TonsRefrigeration`

       The datapoints observations are measured in tons of refrigeration.

   17. `/power/Watt`

       The datapoints observations are measured in Watt.

   18. `/pressure/Pascal`

       The datapoints observations are measured in Pascal.

   19. `/pressure/bar`

       The datapoints observations are measured in bar.

   20. `/scalar/degrees`

       The datapoints observations are angle measured in degrees.

   21. `/scalar/generic`

       The datapoints observations are generic without unit \(e.g. counts, status messages, ...\).

   22. `/scalar/percent`

       The datapoints observations are percentage values.

   23. `/scalar/ppm`

       The datapoints observations are parts per million counts.

   24. `/scalar/radiant`

       The datapoints observations are angle measured in radians.

   25. `/speed/kmPerHour`

       The datapoints observations are measured in kilometers per hour.

   26. `/speed/meterPerSecond`

       The datapoints observations are measured in meters per seconds.

   27. `/temperature/Celsius`

       The datapoints observations are measured in degrees Celsius.

   28. `/temperature/Fahrenheit`

       The datapoints observations are measured in degrees Fahrenheit.

   29. `/temperature/Kelvin`

       The datapoints observations are measured in Kelvin.

   30. `/time/hours`

       The datapoints observations are measured in hours.

   31. `/time/minutes`

       The datapoints observations are measured in minutes.

   32. `/time/seconds`

       The datapoints observations are measured in seconds.

   33. `/volume flow/cubicMetersPerHour`

       The datapoints observations are measured in cubic meters per hour.

   34. `/volume flow/gallonsPerMinute`

       The datapoints observations are measured in cubic gallons per minute.

   35. `/volume flow/litersPerMinute`

       The datapoints observations are measured in cubic liters per minute.

   36. `/volume flow/litersPerSecond`

       The datapoints observations are measured in liters per second.

\*\*\*\*

