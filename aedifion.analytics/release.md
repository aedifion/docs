---
description: aedfion.analytics  versioning
---

# Release

## 1.2.0

#### Release date: 2020-07-23

#### What's new in v1.2.0?

#### Component models

* New component models
  * [Filter](../engineers/component-data-models.md#filter)
  * [Humidity conditioner](../engineers/component-data-models.md#humidity-conditioner)
* Further pins were added to component models to enable new analysis
  * [Fan](../engineers/component-data-models.md#fan): _speed_
  * [Weather station](../engineers/component-data-models.md#weather-station)_: relative humidity, reference relative humidity_

#### Analysis function library

* New analysis functions
  * [Fan speed analysis](../engineers/analytics.md#fan-speed-analysis)
  * [Filter servicing analysis](../engineers/analytics.md#filter-servicing-analysis)
  * [Humidity conditioning analysis](../engineers/analytics.md#humidity-conditioning-analysis)
* Extended [weather station analysis](../engineers/analytics.md#weather-station-analysis)
  * Added offset analysis of humidity sensor

## 1.1.0

#### Release date: 2020-06-19

#### What's new in v1.1.0?

#### Analysis framework

* Analytics API endpoints were simplified and are henceforth downward compatible
  * Response model of API endpoint get [GET](https://api3.aedifion.io/ui/#!/Analytics/get_instance_result) [/v2/analytics/instance/{instance\_id}/result/{result\_id}](https://api3.aedifion.io/ui/#!/Analytics/get_instance_result) was simplified
  * [POST](https://api3.aedifion.io/ui/#!/Analytics/post_analysis_instance) [/v2/analytics/instance](https://api3.aedifion.io/ui/#!/Analytics/post_analysis_instance) and [PUT](https://api3.aedifion.io/ui/#!/Analytics/put_analysis_instance) [/v2/analytics/instance/{instance\_id}](https://api3.aedifion.io/ui/#!/Analytics/put_analysis_instance) were updated to enforce a one-to-one linking between _componentInProject_ and _analysisfunctions_
* Analytics endpoint [GET](https://api3.aedifion.io/ui/#!/Analytics/get_analysis_functions) [/v2/analytics/functions](https://api3.aedifion.io/ui/#!/Analytics/get_analysis_functions) was extended by accepting _component\_id_ as a ****query parameter to query available functions

#### Component models

* Further pins were added to component models to enable new analysis
  * [Boiler](../engineers/component-data-models.md#boiler): _alarm message_, _heat_ and _heat flow_
  * [Combined heat and power](../engineers/component-data-models.md#combined-heat-and-power): _alarm message_, _heat_ and _heat flow_
  * [Fan](../engineers/component-data-models.md#fan): _alarm message_
  * [Heat meter](../engineers/component-data-models.md#heat-meter): _heat_ and _heat flow_
  * [Heat pump](../engineers/component-data-models.md#heat-pump): _alarm message_, _condenser heat, condenser heat flow, evaporator heat,_ and _evaporator heat flow_
  * \_\_[Room](../engineers/component-data-models.md#room): _dew point alarm, humidity_ and _outside air temperature_
  * [Thermal control loop](../engineers/component-data-models.md#thermal-control-loop): _alarm message_

#### Analysis function library

* New analysis functions
  * [Alarm state analysis](../engineers/analytics.md#alarm-state-analysis)
  * [Dew point alert analysis](../engineers/analytics.md#dew-point-alert-analysis)
  * [Sensor outage analysis](../engineers/analytics.md#sensor-outage-analysis)
  * [Thermal comfort analysis](../engineers/analytics.md#thermal-comfort-analysis)
* Extended [reduced load analysis](../engineers/analytics.md#reduced-load-analysis)
  * Added schedule analysis of load reduction
  * Refinement of _temperature level shift_ of load reduction, limiting load reduction analysis to the pin _temperature setpoint_ for higher accuracy
* Extended [virtual heat meter](../engineers/analytics.md#virtual-heat-meter)
  * Added comparison of heat meter to virtually determined heat

### 1.0.1

#### Release date: 2020-05-08

#### What's new in v1.0.1?

* Fixed [setpoint deviation analysis](../engineers/analytics.md#setpoint-deviation-analysis) sensibility to operating message
* More tolerant signal color thresholds for [temperature spread analysis](../engineers/analytics.md#temperature-spread-analysis) on CHPs
* Added KPI _co2.minimum_ to [room air quality analysis](../engineers/analytics.md#room-air-quality-analysis)
* Renamed pin of [thermal control loop](../engineers/component-data-models.md#thermal-control-loop): _return temperature_ to _inlet temperature recirculation_, the component therefore approaches 3-way valves in its definition

## 1.0.0.

#### Release date: 2020-03-20

#### Analytics framework

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

#### Component models

* [Boiler](../engineers/component-data-models.md#boiler)
* [Combined heat and power](../engineers/component-data-models.md#combined-heat-and-power)
* [Fan](../engineers/component-data-models.md#fan)
* [Heat meter](../engineers/component-data-models.md#heat-meter)
* [Heat pump](../engineers/component-data-models.md#heat-pump)
* [Room](../engineers/component-data-models.md#room)
* [Thermal control loop](../engineers/component-data-models.md#thermal-control-loop)
* [Weather station](../engineers/component-data-models.md#weather-station)

#### Analysis functions library

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

#### Serviced analytics

* Serviced analytics is an optionally bookable add-on to the .analytics basic license.
* Minimal service level:
  * Component mapping as a service
  * Analysis configuration as a service
  * Monthly curated analysis results as a service
* The service is provided by aedifion professionals.

