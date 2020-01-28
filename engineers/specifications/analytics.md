---
description: Detailed specification of available analytics functions and their application.
---

# Analytics

## Application notes

* **Unit sensitivity:** To this state, our algorithms are unit sensitive. Every [pin ](../../glossary.md#pin)and [attribute ](../../glossary.md#attribute)is specified with an unit. Mind the specifications.

{% hint style="danger" %}
If unit conventions are disregarded, this can lead to errors and even misleading results of algorithms.
{% endhint %}

* **Improving results:** We continuously improve our analyses functions, interpretations and recommendations. Thus, performing exactly the same analysis at a later point in time can lead to different results.

## Example Analysis

This example leads through our description of [analytics functions](../../glossary.md#analysis-function) by providing exemplary values and descriptions. The first paragraph of an analysis function provides a short introduction to the analysis.

{% tabs %}
{% tab title="Quick Start" %}
## Value

_In other words: Why should I spend time to dig deeper into this analysis._

* Quick entry into analytics function specifications
* Exemplary template for analysis function specifications and documentation structure

## Recommended for component types

* All components in need for an example
{% endtab %}

{% tab title="Description" %}
The _****_**Example Analysis** is an example on how we structure the documentation of our analysis functions. The goal is to show you where to expect which kind of information to make your work as effective as possible.

In general the description provides information about the concepts of the analysis function, its use-case scenario, and value. 
{% endtab %}

{% tab title="Results" %}
## **Qualitative warning level**

Enum representations of traffic light colors providing a quick overview over the __analysis result.

| Enum | Color |
| :--- | :--- |
| 0 | green |
| 1 | yellow |
| 2 | red |

## Interpretations

Interpretation text of the condition analysed.

## Recommendations

Recommendation texts on how to improve the operation of the analysed component.

## KPIs

Providing deeper insights to the cycle behavior. KPIs support human reasoning.

#### Example KPI

Brief description of the example KPI, its determination, application, and value.

| KPI Identifier | Value Range | Unit |
| :--- | :--- | :--- |
| example.kpi | 0 to 1 | kW |



#### Another Example KPI

...
{% endtab %}

{% tab title="Example" %}
_The **Example** tab provides a hands-on example for each analysis function._

The example building is equipped with an [Example Component](component-data-models.md#example-component). This setup is used as a real test bench for the Example Analysis function...

_In general you can expect a short demonstration on how we applied the analysis during our development and which results we got from our test bench._
{% endtab %}

{% tab title="Components" %}
## Pins

* example pin
* ...

_Most of the analysis functions require a mapping for the same pins regardless of the component data model. These pins are listed here. Component-related variations, exceptions, and deeper insights are provided component-wise further down this chapter._

## **Attributes**

* example attribute
* ...

_Some of the analysis functions require attributes regardless of the component data model. These attributes are listed here. Component-related variations, exceptions, and deeper insights are provided component-wise further down this chapter._

## Components

### [Example Component](component-data-models.md)

The short text snippets below a component provide information about how to utilize the analysis function for this component. In case you are new to [component data models](../../glossary.md#component-data-model), we recommend to read the [high-level introduction to components](../../aedifion.io/data/semantic-data-model.md) and check out the [component data models](component-data-models.md) for deeper insights.



### [Another Example Component](component-data-models.md#example-component)

...
{% endtab %}

{% tab title="Application" %}
_The **Application** tab offers further information for real application of the analysis._

## Recommended Time Span

### 4 weeks

_The **Recommended Time Span** gives the recommended time span for  which the analysis function is intended. The time span is oriented to the time constant of the phenomenon investigated, e.g. in order to analyse weekly accruing patterns, a time span of 4 weeks is recommended. In shorter periods of time, the analysed phenomenon might not occur, while longer periods might lead to superposition of other effects such as seasonal influences and thus blurring of the analysis results. **Of course, analysis functions can be used over flexible periods of time at your personal discretion.**_

## **Recommended Repetition**

### every 3 months

_The Recommended Repetition provides a recommendation when the analysis should be repeated. The recommendation refers to a continuous operation of the analysed system. In the case of changes to the system, both physical and operational, a repetition of the analysis is generally recommended. **Of course, analysis can be repeated at your personal discretion.**_

_\*\*\*\*_

## Another recommendation

...
{% endtab %}
{% endtabs %}

## Setpoint Deviation Analysis

{% tabs %}
{% tab title="Quick Start" %}
## Value

Setpoint deviation is a strong symptom for faulty control loop operation, e.g. caused by

* Technical defects in the control loop supply,
* Control loop malfunctions, and
* Faulty control loop parameter settings.

Benefits of improving insufficient setpoint value attainment are:

* Higher occupants comfort
* Lower operating costs
* Higher energy efficiency

## Recommended for component types

Control loops, such as

* Heating systems
* Ventilation systems
* Air-conditioning systems
{% endtab %}

{% tab title="Description" %}
The Setpoint Deviation Analysis aims to identify issues regarding the deviations between the desired value and the actual value within a building’s many control systems. Deviations can occur based on a whole host of reasons. Maybe the system is not supplied with a high enough temperature during certain hours to begin with, or there might be an issue with the controlling software. Maybe a blocked valve is to blame. The Setpoint Deviation Analysis’ key performance indicators \(KPIs\) can help pin down the root cause of the problem, or it might just confirm that everything is working as it should.
{% endtab %}

{% tab title="Results" %}
## **Qualitative warning level**

Enum representations of traffic light colours providing a quick overview over the general setpoint compliance within the analysed period.

| Enum | Color |
| :--- | :--- |
| 0 | green |
| 1 | yellow |
| 2 | red |

## Interpretations

Interpretation of the setpoint compliance of the component in the analysed period.

## Recommendations

Recommendation texts on what actions to take, in case of sub-optimal setpoint compliance as well as recommendations on how to further investigate the root causes for such behavior.

## KPIs

Providing deeper insights to the cycle behavior. KPIs support human reasoning and provide full transparency of the algorithms reasoning.

### Incidence of setpoint deviation

Duration of the setpoint deviations, bundled by threshold value ranges.

| KPI Identifier | Description | Value Range | Unit |
| :--- | :--- | :--- | :--- |
| setpoint deviation.largerX.relative | Duration with setpoint deviation larger than X | 0 to 100 | % |
| setpoint deviation.largerYsmallerX.relative | Duration with setpoint deviation larger than Y and smaller than X | 0 to 100 | % |
| setpoint deviation.smallerY.relative | Duration with setpoint deviation smaller than Y | 0 to 100 | % |
| setpoint deviation.largerX | Duration with setpoint deviation larger than X | 0 to inf | h |
| setpoint deviation.largerYsmallerX | Duration with setpoint deviation larger than Y and smaller than X | 0 to inf | h |
| setpoint deviation.smallerY | Duration with setpoint deviation smaller than Y | 0 to inf | h |

### Operating time

Operating time KPIs provide information on the total time of operation of the analysed component during the analysed time frame.

| KPI Identifier | Description | Value Range | Unit |
| :--- | :--- | :--- | :--- |
| operating time | Total operating time | 0 to inf | h |
| operating time.relative | Relative operating time | 0 to 100 | % |

### Statistics of setpoint deviation

General information KPIs to give further insight into the setpoint compliance over the analysed time frame.

| KPI Identifier | Description | Value Range | Unit |
| :--- | :--- | :--- | :--- |
| setpoint deviation.maximum | Largest setpoint deviation | 0 to inf | - |
| setpoint deviation.minimum | Smallest setpoint deviation | 0 to inf | - |
| setpoint deviation.mean | Average setpoint deviation | 0 to inf | - |
| setpoint deviation.median | Median setpoint deviation | 0 to inf | - |
{% endtab %}

{% tab title="Example" %}
The setpoint deviation analysis was applied to a real test bench, a heating system at the E.ON Energy Research Center, RWTH Aachen University. Thus, a thermal control loop component model was instanced and the respective datapoints mapped to this component.

![](../../.gitbook/assets/sda%20%281%29.svg)

In this scenario, the figure above shows the time series recorded for an exemplary period of 36 hours on a November workday. The temperature setpoint and the actual measured value started to drift apart around 12 am on the 19th. Since then, the control loop did not comply with the setpoint temperatures although the control loop was operating.

The automated interpretation confirms our visual analysis of the time series shown in the figure, summed up by the qualitative warning level "red". The recommendations provide further instruction on how to isolate and fix the cause for the inadequate setpoint compliance. Further, the result offers an advanced set of KPIs, providing additional insights into the control loop behaviour. They support human reasoning for a case-by-case analysis.

For example, the drop in temperatures is peculiar and could point to a technical defect or malfunction, such as a blocked valve. Another cause might be a sudden drop in the temperatures supplied to the distribution system, such as an heat-pump or boiler issue. Further investigation of the root cause is possible via data visualization on the aedifion front-end.
{% endtab %}

{% tab title="Components" %}
## \*\*\*\*[**thermal\_control\_loop**](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#thermal-control-loop)\*\*\*\*

#### Pins

* outlet temperature
* inlet temperature
* operating message

## \*\*\*\*[**room**](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#room)\*\*\*\*

#### Pins

* temperature
* temperature setpoint
* operating message
{% endtab %}

{% tab title="Application" %}
## Recommended Time Span

* 1 to 7 days for significant deviation
* hourly scale for in detail analysis

## **Recommended Repetition** <a id="recommended-repetition-1"></a>

* Every few weeks, since control loops are very sensitive to operational conditions.
* After the start of the heating or cooling period.
* If an issue is suspected.
{% endtab %}
{% endtabs %}

## Room Air Quality Analysis

{% tabs %}
{% tab title="Quick Start" %}
## Value

* Occupant comfort
* Occupant health

## Recommended for component types

* Room
{% endtab %}

{% tab title="Description" %}
The Room Air Quality Analysis checks and interprets the compliance of carbon dioxide concentration in the air to the recommendations of DIN EN 13776: 2007-09. In case of poor air quality, measures for improvement are recommended. In addition, the algorithms identifies calibration errors by physical plausibility checks.
{% endtab %}

{% tab title="Results" %}
## **Qualitative warning level**

Enum representations of traffic light colors providing a quick overview over the air quality over the analysed period.

| Enum | Color |
| :--- | :--- |
| 0 | green |
| 1 | yellow |
| 2 | red |

## Interpretations

Evaluations of the general air quality over the analysed period.

## Recommendations

Recommendation texts on what actions to take, in case of mediocre or poor air quality.

## KPIs

### Air quality classification

How long was the air quality in the room \(based on carbon dioxide concentrations\) considered “good”, “medium”, “moderate” or “poor”? Assessments are based on EU regulation classifications of Indoor Air Quality \(IDA\) classes 1 \(“good”\) to 4 \(“poor”\).

| KPI Identifier | Description | Value Range | Unit |
| :--- | :--- | :--- | :--- |
| co2 duration.IDA1.relative | Duration with “good“ indoor air quality | 0 to 100 | % |
| co2 duration.IDA2.relative | Duration with “medium “ indoor air quality | 0 to 100 | % |
| co2 duration.IDA3.relative | Duration with “moderate “ indoor air quality | 0 to 100 | % |
| co2 duration.IDA4.relative | Duration with “poor “ indoor air quality | 0 to 100 | % |
| co2 duration.IDA1.absolute | Duration with “good“ indoor air quality | 0 to inf | h |
| co2 duration.IDA2.absolute | Duration with “medium “ indoor air quality | 0 to inf | h |
| co2  duration.IDA3.absolute | Duration with “moderate “ indoor air quality | 0 to inf | h |
| co2 duration.IDA4.absolute | Duration with “poor “ indoor air quality | 0 to inf | h |

### Statistics of CO2 concentration

Providing deeper insights to the carbon dioxide concentrations over the analysed period.

| KPI Identifier | Description | Value Range | Unit |
| :--- | :--- | :--- | :--- |
| co2.maximum | Largest CO2 concentrations | 0 to inf | ppm |
| co2.minimum | Smallest CO2 concentrations | 0 to inf | ppm |
| co2.mean | Average CO2 concentrations | 0 to inf | ppm |
| co2.median | Median CO2 concentrations | 0 to inf | ppm |
{% endtab %}

{% tab title="Components" %}
## \*\*\*\*[**room**](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#room)\*\*\*\*

#### Pins

* co2
{% endtab %}

{% tab title="Application" %}
##  Recommended Time Span

* &gt;1 days
* Utilize on days with room occupation

## **Recommended Repetition** <a id="recommended-repetition-1-1"></a>

* On changes of room occupation or usage
* Every few months in order to check sensor calibration
* If an issue is suspected
{% endtab %}
{% endtabs %}

## Virtual Heat Meter

{% tabs %}
{% tab title="Quick Start" %}
## Value

The Virtual Heat Meter helps to

* virtual heat meter, quantifies heat fluxes
* health check of heat meters

## Recommended for component types

Heating systems, such as

* Thermal control loops
* Heat pumps
* Boilers
* Heat meters
{% endtab %}

{% tab title="Description" %}
The Virtual Heat Meter determines the heat flux and energy delivered in heating/cooling piping networks such as thermal control loops or energy conversion plants. The determination is based on the temperature difference and volume flow over measurement point.
{% endtab %}

{% tab title="Results" %}
## KPIs

#### Statistics of heat flux

Providing insights into value range of the heat flux.

{% hint style="danger" %}
Negative values indicate cooling, while positive indicate heating.
{% endhint %}

{% hint style="info" %}
**Heat pump:** The KPI identifiers are extended by the prefix _condenser_ or _evaporator_ to specify the side of the heat pump the virtual heat meter is applied on. E.g.:

heat flux.maximum will be evaporator
{% endhint %}

| KPI Identifier | Description | Value Range | Unit |
| :--- | :--- | :--- | :--- |
| heat flux.maximum | Largest heat flux | -inf to inf | kW |
| heat flux.minimum | Smallest heat flux | -inf to inf | kW |
| heat flux.mean | Average heat flux | -inf to inf | kW |
| heat flux.medium | Median heat flux | -inf to inf | kW |
| heat | Total heat transferred | -inf to inf | kWh |

## Timeserieses

| KPI Identifier | Description | Value Range | Unit |
| :--- | :--- | :--- | :--- |
| heat flux.timeseries | The complete timeseries of transferred heat | -inf to inf | kW |
| heat flux.cumulated | The complete timeseries of cumulated transferred heat | -inf to inf | kWh |
{% endtab %}

{% tab title="Components" %}
## [**heat\_meter**](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#heat-meter)\*\*\*\*

#### Pins

* outlet temperature
* inlet temperature
* volume flow

#### Attributes

* volume\_flow\_unit

## [boiler](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#boiler), [condensing\_boiler](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#condensing-boiler), [low-temperature\_boiler](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#low-temperature-boiler)

#### Pins

* outlet temperature
* inlet temperature
* operating message
* volume flow

**Attributes**

* volume\_flow\_unit

## \*\*\*\*[**heat\_pump**](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#heat-pump)\*\*\*\*

{% hint style="warning" %}
The Virtual Heat Meter is determined on the condenser and evaporator side depending on the mapped datapoints. 
{% endhint %}

#### Pins

* condenser inlet temperature
* condenser outlet temperature
* condenser volume flow
* evaporator inlet temperature
* evaporator outlet temperature
* evaporator volume flow
* operating message

#### Attributes

* volume\_flow\_unit \(same for evaporator and condenser

## Attributes

#### volume\_flow\_unit:

The unit used in this datapoint needs to be specified in order for the analysis to yield correct result. If unspecified, the default unit assumed for this measurement is _cubic meters per second_. Acceptable inputs for this attribute include: 

* _cubicMetersPerSecond_
* _cubicMetersPerMinute_
* _cubicMetersPerHour_
* _litersPerSecond_
* _litersPerMinute_
* _litersPerHour_
{% endtab %}

{% tab title="Application" %}


## Recommended Time Span

* 1 - 3 days

## **Recommended Repetition**

### Every month
{% endtab %}
{% endtabs %}



## Operating Cycle Analysis

The **Operating Cycle Analysis** investigates and interprets the cycle behavior of components. Besides identifying sub-optimal cycle behavior, the algorithm provides recommendations on how to improve cycle rates, and KPIs for deeper insights.

{% tabs %}
{% tab title="Quick Start" %}
## **Value**

* Lower operating costs
* Higher energy efficiency
* Peak energy consumption reduction
* Longer equipment and component life times
* Smoother system integration

## Recommended for component types

* Energy conversion plants
* Components with high start-up energy consumption
* Heat pumps
* Combined heat and power
* etc.
{% endtab %}

{% tab title="Description" %}
The **Operating Cycle Analysis** identifies excessive start and stop processes which lead to energy losses, energy consumption peaks due to higher energy consumption on plant start and higher wear and tear of the component compared to a constant operation. Further, a frequently alternating operation of a component, e.g. a heat pump, has negative effects on adjacent components, which are enforced to alternate as well.

Further, the algorithm takes low cycle rates as an indication of a possible under-supply of the adjacent systems.
{% endtab %}

{% tab title="Results" %}
## **Qualitative warning level**

Enum representations of traffic light colors providing a quick overview over the operation quality regarding cycle rates.

| Enum | Color |
| :--- | :--- |
| 0 | green |
| 1 | yellow |
| 2 | red |

## Interpretations

Interpretation text of the cycle rates of the analysed component in regard to its overall operation time.

## Recommendations

Recommendation texts on how to improve cycle rates of the analysed component including adjustment of control parameter as well as recommendations on how to investigate the root cause for unpleasant cycle behavior.

## KPIs

  
Providing deeper insights to the setpoint compliance. KPIs support human reasoning.

### Operating time

Operating times KPIs provide information on the total time of operation of the analysed component during the analysed time span.

| KPI Identifier | Description | Value Range | Unit |
| :--- | :--- | :--- | :--- |
|  operating time | Total time of operation | 0 to inf | h |
|  operating time.relative | Total time of operation divided by total time span | 0 to 100 | % |

### Start

The Start KPI is the count of starts of the analysed component during the analysed time span.

| KPI Identifier | Description | Value Range | Unit |
| :--- | :--- | :--- | :--- |
| starts | Count of starts | 0 to inf | count |

### Closed operating cycle

A _closed operating cycle_ is defined as _period of time between a start_ $$n_i$$_of the component an the next start_ $$n_{i+1}$$. The KPI _closed operating cycles_ represents the number of cycles observed during the analysed time span used to determine the operating cycle KPIs \(_cycle times, duty times, switch-off times_\).

| KPI Identifier | Description | Value Range | Unit |
| :--- | :--- | :--- | :--- |
| closed operating cycles | Count of closed operating cycles | 0 to inf | count |

### Cycle times

Cycle time KPIs evaluate the cycle times of the closed cycles observed during the analysed time span. The mean, time-weighted average, minimum, and maximum cycle period are determined. In case there were no closed operating cycles observed during the analysed time span, none of the KPI variables are returned on API call.

| KPI Identifier | Description | Value Range | Unit |
| :--- | :--- | :--- | :--- |
| cycle times.median | Median of cycle periods | 0 to inf | h |
| cycle times.average | Time-weighted average of cycle periods | 0 to inf | h |
| cycle times.maximum | Longest cycle period | 0 to inf | h |
| cycle times.minimum | Shortest cycle period | 0 to inf | h |

### Duty times

Duty time KPIs evaluate the duty times of the closed cycles observed during the analysed time span. Duty time is defined as the time of component operation in a closed cycle. The mean, time-weighted average, minimum, and maximum duty period are determined. In case there were no closed operating cycles observed during the analysed time span, none of the KPI variables are returned on API call.

| KPI Identifier | Description | Value Range | Unit |
| :--- | :--- | :--- | :--- |
| duty times.median | Median of duty periods | 0 to inf | h |
| duty times.average | Time-weighted average of duty periods | 0 to inf | h |
| duty times.maximum | Longest duty period | 0 to inf | h |
| duty times.minimum | Shortest duty period | 0 to inf | h |

### Switch-off times

Switch-off time KPIs evaluate the shutdown times of the closed cycles observed during the analysed time span. Switch-off time is defined as the time of component shutdown in a closed cycle. The mean, time-weighted average, minimum, and maximum switch-off period are determined. In case there were no closed operating cycles observed during the analysed time span, none of the KPI variables are returned on API call.

| KPI Identifier | Description | Value Range | Unit |
| :--- | :--- | :--- | :--- |
| switch-off times.median | Median of switch-off periods | 0 to inf | h |
| switch-off times.average | Time-weighted average of switch-off periods | 0 to inf | h |
| switch-off times.maximum | Longest switch-off period | 0 to inf | h |
| switch-off times.minimum | Shortest switch-off period | 0 to inf | h |
{% endtab %}

{% tab title="Example" %}
The **Operating Cycle Analysis** was applied to a real test bench, the heat pump of the E.ON Energy Research Center, RWTH Aachen University. Thus, a [heat pump component model ](component-data-models.md#heat-pump)was instanced and the respective datapoint mapped to the pin _operating message_. __Figure 1 shows the time series recorded for an exemplary period of 6 hours on a winter day.

![Plot of operating message time series from 12:00 am till 6:00 pm](../../.gitbook/assets/operating_cycle_analysis.png)

Short shut-down times between periods of duty indicate excessive start and stop processes of the heat pump, leading not only to energy losses and electricity consumption peaks but also to increased wear and tear of the heat pumps compressor. 

The automated interpretation confirms our visual analysis of the time series shown in the figure, summed up by the qualitative warning level "red". The recommendations provide further instruction on how to isolate and fix the cause for the increased number of start and stop processes. Further, the result offers an advanced set of KPIs, providing additional insights into the cycle behavior of the heat pump.
{% endtab %}

{% tab title="Components" %}
## Pins

* operating message

## Components

### [Boiler](component-data-models.md#boiler)

#### [Condensing Boiler](component-data-models.md#condensing-boiler)

#### [Low-Temperature Boiler](component-data-models.md#low-temperature-boiler)

### [Combined Heat and Power](component-data-models.md#combined-heat-and-power)

### [Fan](component-data-models.md#heat-pump)

Results are limited to KPIs.

### [Heat Pump](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#heat-pump)

### 
{% endtab %}

{% tab title="Application" %}
## Recommended Time Span

### 1 - 3 days

* Mind weekends

## **Recommended Repetition**

### Every month

* Cycle rates have a strong seasonal effect
* Frequent repetition allows to identify operational bad points
{% endtab %}
{% endtabs %}

## Outdoor Temperature Sensor Analysis

This Analysis detects if a sensor is influenced by sun radiation or if the sensor has an offset to reference weather data from a different source.

{% tabs %}
{% tab title="Quick Start" %}
## Value

* Lower operating costs
* Make sure your building automation systems can work with reliable information about outside conditions
* Make sure your sensor readings are not influenced by sun radiation
* Make sure your sensor is working correctly

## Recommended for component types

* Weather Station
{% endtab %}

{% tab title="Description" %}
HVAC and heating systems of buildings are controlled with the help of sensors. The controller of these systems can't verify the correctness of the measured values and will control in accordance with the measured values.

Sensors are calibrated by the manufacturer or installation technician and will deliver a reading that is accurate to the required accuracy class. With an increasing operating time of the sensor, readings will shift and errors will increase compared to the moment of installation. Additionally to these sensor inherent errors, there can also be errors introduced that originate from the surrounding area the sensor is placed. Latter errors can be significantly higher than the sensor inherent errors because there is no accurate measure for them.

Therefore the placement of sensors should be well-considered. An important measurement for HVAC and heating systems is the outside air temperature, which helps the controller decide how much heating or cooling is required to deliver a comfortable indoor climate. Errors during the measurement of the outside air temperature directly correspond to an over or undersupply of the building and can lead to poor user comfort and a waste of energy.

To measure the outside air temperature the sensor must be placed outside of the building and often enough the placement is sub-optimal and sunlight can reach the sensor housing which results in a temperature reading that is higher than the actual outside air temperature.

This Analysis will compare the outside air temperature measurement of the sensor to measurement data from a different source \(f.e. weather service\) for each observation to determine if the sensor shows any discrepancies.
{% endtab %}

{% tab title="Results" %}
## KPIs

| KPI Identifier | Value Range | Unit |
| :--- | :--- | :--- |
| radiation influenced.relative | 0 to 100 | % |
| radiation influenced.total | 0 - inf | days |

#### radiation influenced.total

radiation influenced.total is equal to the number of days that show one or more hours of radiation influence.

#### radiation influenced.relative

radiation influenced.relative is a ratio between radiation influenced.total and the observed period expressed in percent.
{% endtab %}

{% tab title="Example" %}
_In general you can expect a short demonstration on how we applied the analysis during our development and which results we got from our test bench._
{% endtab %}

{% tab title="Components" %}
## Pins

* temperature

## **Attributes**

* **latitude**
  * latitude position \[deg\] of the examined component
* **longitude**
  * longitude position \[deg\] of the examined component

## Components

* [Weather Station](component-data-models.md#weather-station)
{% endtab %}

{% tab title="Application" %}
## Recommended Time Span

* Try to find an assessment period of several weeks or month to get a good chance of detecting influenced days
* time spans around summertime are more likely to have days with a lot of sunshine and thus increase the chance of detecting these days

## **Recommended Repetition**

#### every 3 months

#### quaterly
{% endtab %}
{% endtabs %}

## **Temperature Spread Analysis**

{% tabs %}
{% tab title="Quick Start" %}
## Value

* energy-efficient operation of thermal control loops
* energy savings that lead to reduced costs

## Recommended for component types

* **thermal control loop**
{% endtab %}

{% tab title="Description" %}
The Temperature Spread Analysis assesses the difference between two temperature pins while an operating message is active.

A thermal control loop is a typical case where outlet and return temperature are different and a temperature spread that is higher indicates a good utilization of this loop. A lower temperature spread suggests a reduced energy demand of the connected consumers and thus should lead to a reduction of volume flow caused by a reduced pump speed that saves electricity.
{% endtab %}

{% tab title="Results" %}
## KPIs

| KPI Identifier | Value Range | Unit |
| :--- | :--- | :--- |
| temperature spread.average | - | °C |
| temperature spread.minimum | - | °C |
| temperature spread.maximum | - | °C |

**temperature spread.average**

This corresponds to the average temperature spread across the period of the analysis.

**temperature spread.minimum**

This corresponds to the smallest temperature spread across the period of the analysis.

**temperature spread.maximum**

This corresponds to the largest temperature spread across the period of the analysis.
{% endtab %}

{% tab title="Example" %}
_In general you can expect a short demonstration on how we applied the analysis during our development and which results we got from our test bench._
{% endtab %}

{% tab title="Components" %}
## Pins

* outlet temperature
* return temperature
* operating message

## Components

### Thermal Control Loop
{% endtab %}

{% tab title="Application" %}
## Recommended Time Span

### 1 day - several weeks

## **Recommended Repetition**

### every 3 months
{% endtab %}
{% endtabs %}

## **Heating Curve Analysis**

{% hint style="warning" %}
Documentation currently under construction 🚧
{% endhint %}

## Schedule Analysis

{% hint style="warning" %}
Documentation currently under construction 🚧
{% endhint %}

## **Control Loop Oscillation Analysis**

{% hint style="warning" %}
Documentation currently under construction 🚧
{% endhint %}

## **Heat Flux Analysis**

{% hint style="warning" %}
Documentation currently under construction 🚧
{% endhint %}

## Information

The library of analytics functions is constantly expanded. If you are missing an analytics function, wish to implement your own functions, or want us to implement it for you, feel free to [contact us](../../contact.md#support).

