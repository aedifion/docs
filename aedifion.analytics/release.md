---
description: aedfion .analytics  versioning
---

# Release

## 1.0.0. - 2020-03-20

### Analytics framework

* Abstract component models
  * aedifion provides a library of curated component models that abstractly model the functioning of various physical or virtual technical equipment such as boilers, heat pumps, control loops, rooms, etc.
  * Each abstract component model has a set of pins that are placeholders for a real component's datapoints, e.g., the component model _heat pump_ has pins for its condenser supply and return temperature.
  * Each abstract component model has a set of attributes that are placeholders for a real component's meta data such as installation parameters, nominal performance data, operation parameters, etc. E.g., the component model _weather station_ has attributes for the longitude and latitude of its installation.
  * Instancing an abstract component model to a project and mapping of its pins and attributes creates a digital twin of physical or virtual technical equipment. Mapping is the process of attaching datapoints and attributes to a component data model instanced for a project.
* Analysis functions
  * aedifion provides a library of algorithm functions to analyze building performance.
  * Each analysis function is applicable to one or more components. E.g., an analysis of the room air quality might apply only to the component _room_, while an analysis of duty cycles might apply to any physical, active component.
* Analysis configuration
  * Configuration of which analysis functions shall be applied to which mapped component, stored as _analysis instance_.
  * _Analysis instances_ can be executed on arbitrary time ranges, e.g., days, weeks, months, or whole years. 
* Analysis result
  * Each execution of an instance on a time range provides a result.
  * Results comprise of signal colors, interpretations, recommendations, key performance indicators and timeserieses and are utilized individually for each analysis. Signal colors provide a visual evaluation of the analyzed operational conditions. Interpretations provide a textual summary of the analyzed operational condition. Recommendations provide a textual measure to further investigate or fix the cause of suboptimal operational performance. Key performance indicators and timeseries create transparency of the operational conditions analyzed. 
  * Analysis results are stored in an aedifion database for later queries.
* Utilize analytics framework
  * Analytics framework utilizable to the full extend via API. Comprehensive guides and tutorials available.
  * The aedifion.io frontend offers the display signal colors, interpretations, recommendations and key performance indicators of the most recent analysis results for each analysis instance, including instancing and mapping of component models.

### Component models

* [Boiler](../engineers/component-data-models.md#boiler)
* [Combined heat and power](../engineers/component-data-models.md#combined-heat-and-power)
* [Fan](../engineers/component-data-models.md#fan)
* [Heat meter](../engineers/component-data-models.md#heat-meter)
* [Heat pump](../engineers/component-data-models.md#heat-pump)
* [Room](../engineers/component-data-models.md#room)
* [Thermal control loop](../engineers/component-data-models.md#thermal-control-loop)
* [Weather station](../engineers/component-data-models.md#weather-station)

### Analysis functions library

* [Control loop oscillation analysis](../engineers/analytics.md#control-loop-oscillation-analysis)
* [Operating cycle analysis](../engineers/analytics.md#operating-cycle-analysis)
* [Reduced load analysis](../engineers/analytics.md#reduced-load-analysis)
* [Room air quality analysis](../engineers/analytics.md#room-air-quality-analysis)
* [Schedule analysis](../engineers/analytics.md#schedule-analysis)
* [Setpoint deviation analysis](../engineers/analytics.md#setpoint-deviation-analysis)
* [Synchronized operation analysis](../engineers/analytics.md#synchronized-operation-analysis)
* [Temperature spread analysis](../engineers/analytics.md#temperature-spread-analysis)
* [Virtual heat meter](../engineers/analytics.md#temperature-spread-analysis)
* [Weather station analysis](../engineers/analytics.md#weather-station-analysis)

### Serviced analytics

* Serviced analytics is an optionally bookable add-on to the .analytics basic license.
* Minimal service level:
  * Component mapping as a service
  * Analysis configuration as a service
  * Monthly curated analysis results as a service
* The service is provided by aedifion professionals.

