---
description: >-
  Detailed specification of analytics functions, their benefits, and
  application.
---

# Analytics

## Introduction

Analytics peruses only one goal: Guide technicians and building users to improve operational performance of buildings and energy systems, while the benefits of improved operational performance are multilateral:

* Higher **comfort**, **well being** and therefore **performance** of people in buildings.
* Higher **energy efficiency** providing comfort and energy services.
* Lower effort **maintaining** and **servicing** complex technical facilities.

## Tabs explained

Each analysis function has unique specifications. For a target-oriented browsing, the specifications are ordered in tabs

* Quick Start,
* Description,
* Example,
* Results,
* Components, and
* Application.

### Quick Start

This tab summarizes the value offered by the analysis function and for which components it is recommended. If the analysis function catches your attention, brows the other tabs and find further applications.

### Description

In general the description provides a summary of the concepts of the analysis function, its use-case scenario, and value. 

### Example

In general you can expect a short case study on how the analysis function was applied during development or a test bench.

### Results

Results of analytics functions are structured to deliver simple to navigate insights and fast to apply measures on how to improve operational performance. 

Therefor, each result regardless of the analytics function includes

* one qualitative **warning level**, ****aka. traffic light color,
* one **interpretation,**
* zero to n **recommendations,**
* zero to n **KPIs, and**
* zero to n **timeserieses.**

These categories are explained below. While the warning level, interpretation, and recommendation are specified for all analysis functions equally, KPIs and timeserieses differ between each analysis function. Thus, they are specified individually in the _Results_ tab of each function specification.

#### Warning level

The warning level represents the urgency of looking into the analyzed condition and taking action to improve it. It can have one of these traffic light states:

{% hint style="danger" %}
**Red:** Suboptimal performance identified. It can be expected that either improving the identified condition will have a strong effect on the performance or the effort to realize the optimization is moderate compared to its benefit.
{% endhint %}

{% hint style="warning" %}
**Yellow:** Suboptimal performance identified. The effort to optimize might consume its benefit. To reduce the effort, implement the measure with the maintenance work that is required anyway. Observation of the analyzed condition is recommended.
{% endhint %}

{% hint style="success" %}
**Green:** Performance is satisfactory. No action recommended.
{% endhint %}

#### **Interpretation**

The interpretation delivers a summary over the observed performance and state of the condition analyzed. In general it describes either a symptom of a suboptimal operation or condition could be found or not.

In the engineering vocabulary of Fault Detection and Diagnosis, the interpretation represents the Fault Detection.

#### Recommendation

Actually, it is a list of recommendations to amend the detected symptom for suboptimal operation. Either by providing recommendations on how to correct the source of the symptom itself or on how to narrow down its cause.

In the engineering vocabulary of Fault Detection and Diagnosis, the recommendations represent the Fault Diagnosis respectively guidance on how to further proceed the diagnosis.

#### KPIs and timeserieses

KPIs and timeserieses offer insights and transparency. They enable reporting and manual investigation of the operational behavior the component or system analyzed. KPIs and timeserieses are highly individual for each function and are explained in the respective specification of each analysis function in _Results_. 

### Components

The _Components_ tab contains the API identifier of

* the components the analysis function is available for,
* the pins of the components which need to be mapped, and
* the attributes of the component required.

### Application

The _Application_ tab provides information on the application of the analysis function.

* **Recommended time span:** Most of the analysis functions have a sweet spot for the amount of historical data required to derive accurate results.
* **Recommended repetition:** Components of building energy systems are subject to seasonal effects and wear out. Follow the recommended repetition to limit the amount of analysis to the required ones without risking blind spots in the continuous monitoring.

## Setpoint Deviation Analysis

{% tabs %}
{% tab title="Quick Start" %}
## Value

Setpoint deviation is a strong symptom for faulty control loop operation, e.g. caused by

* Technical defects in the control loop supply,
* Control loop malfunctions, and
* Faulty control loop parameter settings.

Benefits of improving insufficient setpoint value attainment are:

* Higher occupant comfort, health and performance
* Lower operating costs
* Higher energy efficiency

## Recommended for component types

Control loops, such as

* Heating systems
* Ventilation systems
* Air-conditioning systems
{% endtab %}

{% tab title="Description" %}
The _Setpoint Deviation Analysis_ identifies insufficient setpoint attainment by comparing the actual value of a controlled system to its setpoint value. Insufficient setpoint attainment is a symptom which can be traced back to plenty of different causes. E.g., insufficient supply of a controlled system with the required temperature level, suboptimal controller software and parameters, or a blocked valve. The _Setpoint Deviation Analysis_ supports narrowing down the root cause of insufficient setpoint attainment and is specially recommended in complex energy systems.
{% endtab %}

{% tab title="Example" %}
The setpoint deviation analysis was applied to a real test bench, a heating system at the E.ON Energy Research Center, RWTH Aachen University. Thus, a thermal control loop component model was instanced and the respective datapoints mapped to this component.

![](../../.gitbook/assets/sda%20%281%29.svg)

In this scenario, the figure above shows the time series recorded for an exemplary period of 36 hours on a November workday. The temperature setpoint and the actual measured value started to drift apart around 12 am on the 19th. Since then, the control loop did not comply with the setpoint temperatures although the control loop was operating.

The automated interpretation confirms our visual analysis of the time series shown in the figure, summed up by the qualitative warning level "red". The recommendations provide further instruction on how to isolate and fix the cause for the inadequate setpoint compliance. Further, the result offers an advanced set of KPIs, providing additional insights into the control loop behaviour. They support human reasoning for a case-by-case analysis.

For example, the drop in temperatures is peculiar and could point to a technical defect or malfunction, such as a blocked valve. Another cause might be a sudden drop in the temperatures supplied to the distribution system, such as an heat-pump or boiler issue. Further investigation of the root cause is possible via data visualization on the aedifion front-end.
{% endtab %}

{% tab title="Results" %}
## KPIs

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

* Higher occupant comfort, health and performance

## Recommended for component types

* Rooms
* Occupied indoor areas
{% endtab %}

{% tab title="Description" %}
The _Room Air Quality Analysis_ checks and interprets the compliance of carbon dioxide concentration in the air to the recommendations of DIN EN 13776: 2007-09. In case of poor air quality, measures for improvement are recommended. Human performance is significantly influenced by air quality.

In addition, the algorithms identifies calibration errors by physical plausibility checks
{% endtab %}

{% tab title="Example" %}


The setpoint deviation analysis was applied to a real test bench, a heating system at the E.ON Energy Research Center, RWTH Aachen University. Thus, a room component model was instanced and the respective datapoints mapped to this component.

![](../../.gitbook/assets/image%20%2819%29.png)

In this scenario, the figure above shows the timeseries recorded for an exemplary period of 12 hours on a working day in August. The CO2 concentration in the air remained between what is considered "good" and "medium" for most of the day. However, for about 7 percent of the period, air quality was poor, with a maximum CO2 concentration of 1463 ppm, so that a complete evaluation on that day indicates poor air quality. The results provide an advanced set of KPIs that provide quantitative insights into the air quality of the rooms and support the human reasoning for analysis. A number of suggestions for possible countermeasures are given, and further investigation of the root cause of air quality problems is possible through the aedifion front-end data visualization.
{% endtab %}

{% tab title="Results" %}
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

Quantifies heat fluxes and heat for

* Trace energy fluxes
* Enable other analytics functions
* Enables comparison to hardware heat meters

## Recommended for component types

Heat and cold conversion or distribution systems, such as

* Thermal control loops
* Heat pumps
* Boilers
* Heat meters
{% endtab %}

{% tab title="Description" %}
The _Virtual Heat Meter_ determines the heat flux and energy delivered in heating/cooling piping networks such as thermal control loops or energy conversion plants. The determination is based on the temperature difference and volume flow over measurement point.

It substitutes physical heat meters and enables energy flux tracing.
{% endtab %}

{% tab title="Example" %}


The **Virtual Heat Meter** was tested in the field, on a boilder at the E.ON Energy Research Center, RWTH Aachen University. Thus, a [boiler ](component-data-models.md#heat-pump)was instanced and the respective datapoints pinned to it. __The figure below shows the inlet- and outlet timeseries recorded for an exemplary period, along with the heat flow calculated. The volume flow during the observed period was constant.

![](../../.gitbook/assets/image%20%2821%29.png)
{% endtab %}

{% tab title="Results" %}
## KPIs

### Statistics of heat flux

Providing insights into value range of the heat flux.

{% hint style="warning" %}
Negative values indicate cooling, while positive indicate heating.
{% endhint %}

{% hint style="info" %}
**Heat pump:** The KPI identifiers are extended by the prefix _condenser_ or _evaporator_ to specify the side of the heat pump the virtual heat meter is applied on. E.g.:

_heat flux.maximum_ will be _evaporator heat flux.maximum_
{% endhint %}

| KPI Identifier | Description | Value Range | Unit |
| :--- | :--- | :--- | :--- |
| heat flux.maximum | Largest heat flux | -inf to inf | kW |
| heat flux.minimum | Smallest heat flux | -inf to inf | kW |
| heat flux.average | Average heat flux | -inf to inf | kW |
| heat flux.median | Median heat flux | -inf to inf | kW |
| heat | Aggregated heat transferred | -inf to inf | kWh |

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

## [boiler](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#boiler)

### [condensing\_boiler](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#condensing-boiler)

### [low-temperature\_boiler](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#low-temperature-boiler)

#### Pins

* outlet temperature
* inlet temperature
* operating message
* volume flow

**Attributes**

* volume\_flow\_unit

## [combined\_heat\_and\_power](component-data-models.md#combined-heat-and-power)

#### Pins

* outlet temperature
* inlet temperature
* operating message
* volume flow

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
The _Operating Cycle Analysis_ ****identifies excessive start and stop processes which lead to energy losses, energy consumption peaks due to higher energy consumption on plant start and higher wear and tear of the component compared to a constant operation. Further, a frequently alternating operation of a component, e.g. a heat pump, has negative effects on adjacent components, which are enforced to alternate as well.

Further, the algorithm takes low cycle rates as an indication of a possible under-supply of the adjacent systems.
{% endtab %}

{% tab title="Example" %}
The **Operating Cycle Analysis** was applied to a real test bench, the heat pump of the E.ON Energy Research Center, RWTH Aachen University. Thus, a [heat pump component model ](component-data-models.md#heat-pump)was instanced and the respective datapoint mapped to the pin _operating message_. __Figure 1 shows the time series recorded for an exemplary period of 6 hours on a winter day.

![Plot of operating message time series from 12:00 am till 6:00 pm](../../.gitbook/assets/operating_cycle_analysis.png)

Short shut-down times between periods of duty indicate excessive start and stop processes of the heat pump, leading not only to energy losses and electricity consumption peaks but also to increased wear and tear of the heat pumps compressor. 

The automated interpretation confirms our visual analysis of the time series shown in the figure, summed up by the qualitative warning level "red". The recommendations provide further instruction on how to isolate and fix the cause for the increased number of start and stop processes. Further, the result offers an advanced set of KPIs, providing additional insights into the cycle behavior of the heat pump.
{% endtab %}

{% tab title="Results" %}
## KPIs

### Operating time

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

{% tab title="Components" %}
## [boiler](component-data-models.md#boiler)

### [condensing\_boiler](component-data-models.md#condensing-boiler)

### [low-temperature\_boiler](component-data-models.md#low-temperature-boiler)

#### Pins

* operating message

## [combined\_heat\_and\_power](component-data-models.md#combined-heat-and-power)

#### Pins

* operating message

## [fan](component-data-models.md#heat-pump)

#### Pins

* operating message

### [heat\_pump](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#heat-pump)

#### Pins

* operating message
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

This analysis detects, if a sensor is influenced by sun radiation, surrounding components, or if the sensor shows an offset.

{% tabs %}
{% tab title="Quick Start" %}
## Value

* Higher operational performance due to reliable information about outside air conditions
* Higher occupant comfort, health and performance
* Lower operating costs
* Better system coordination in systems with redundant sensors

## Recommended for component types

* Weather station
{% endtab %}

{% tab title="Description" %}
The outdoor air temperature sensor is one of the most important sensors for HVAC system control, since many control decisions, e.g. which amount of heat provided or switching between heating and cooling mode, are made based on the measured outdoor air temperature. Outdoor air temperature sensors wear out over the life time of the building. Further, the sensor is often influenced by sun radiation or heat emitting components in its surrounding. Wrongly measured outside air temperature directly corresponds to a thermal over or under supply of the building, often leads to poor user comfort and an exaggerated energy consumption.

The _Outdoor Temperature Sensor Analysis_ identifies installation errors and measurement offsets of the outdoor air temperature sensor and derives optimization measures for better outdoor air temperature measuring.
{% endtab %}

{% tab title="Results" %}
## KPIs

### Sun radiation influence

| KPI Identifier | Description | Value Range | Unit |
| :--- | :--- | :--- | :--- |
| radiation influenced.relative | Ratio of days with more than one hour of sun radiation influence to days analyzed | 0 to 100 | % |
| radiation influenced.total | Days with more than one hour of sun radiation influence | 0 - inf | days |
{% endtab %}

{% tab title="Example" %}
For this Outdoor Temperature Sensor Analysis we instanciated a "[weather station](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#weather-station)" component and analyzed a week of weather data. The following plot shows the measured temperature of a sensor located at a building facade. During the reviewed period in the summer the sensor is influenced in the afternoon.

![](../../.gitbook/assets/example_outdoortemperaturesensoranalysis.png)

In the plot you can see a significant difference between sensor and weather reference. This is also reflected in the value of the calculated KPIs. During the analysis period all 7 days are recognized by the KPI "radiation influenced days". Additionaly the offset at night is elevated and thus a larger "sensor offset squared error" is present.

| KPI | Value | Unit |
| :--- | :--- | :--- |
| radiation influenced.relative | 100 | % |
| radiation influenced days | 7 | count |
| sensor offset squared error | 17.3 | K^2/h |
{% endtab %}

{% tab title="Components" %}
## [weather\_station](component-data-models.md#weather-station)

#### Pins

* temperature

#### **Attributes**

* latitude
* longitude
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

* Higher occupant comfort, health and performance
* Higher energy efficiency
* Lower operating costs

## Recommended for component types

Heat and cold distribution systems, such as

* Thermal control loop
{% endtab %}

{% tab title="Description" %}
The _Temperature Spread Analysis_ assesses the temperature difference between a supply and return temperature sensor of a heat or cold distribution system during the systems operation. While a small temperature spread indicates the potential for volume flow and therefor pump power consumption reduction, a huge spread indicates thermal under supply of the downstream systems and consumers.
{% endtab %}

{% tab title="Example" %}
The **temperature spread analysis** was applied to a heating circuit instanciated as a [thermal control loop](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#thermal-control-loop).

![](../../.gitbook/assets/example_temperaturespreadanalysis.png)

A analysis for one week in the beginning of September 2018 is shown in the plot above. A small temperature spread is detected through the KPI "**temperature spread.average**" of 1.56 K.

| KPI | Value | Unit |
| :--- | :--- | :--- |
| temperature spread.average | 1.56 | Kelvin |
| temperature spread.minimum | -0.716 | Kelvin |
| temperature spread.maximum | 22.8 | Kelvin |
{% endtab %}

{% tab title="Results" %}
## KPIs

### Statistics of temperature spread

| KPI Identifier | Description | Value Range | Unit |
| :--- | :--- | :--- | :--- |
| temperature spread.median | Median of temperature spread | -inf to inf | °C |
| temperature spread.average | Time-weighted average of temperature spread | -inf to inf | °C |
| temperature spread.minimum | Minimum of temperature spread | -inf to inf | °C |
| temperature spread.maximum | Maximum of temperature spread | -inf to inf | °C |
{% endtab %}

{% tab title="Components" %}
## [thermal\_control\_loop](component-data-models.md#thermal-control-loop)

#### Pins

* outlet temperature
* return temperature
* operating message
{% endtab %}

{% tab title="Application" %}
## Recommended Time Span

### 1 day - several weeks

## **Recommended Repetition**

### every 3 months
{% endtab %}
{% endtabs %}

## Schedule Analysis

{% tabs %}
{% tab title="Quick Start" %}
## Value

* Reduce usage times of HVAC machines for a longer service life
* Reduce energy cost

## Recommended for component types

* fan
{% endtab %}

{% tab title="Description" %}
The schedule analysis is used to compare the actual occured switch on/switch off times of the component with a schedule/timetable stored inside analytics. This analysis aims at identifying the amount of hours the component is active outside of the scheduled times.
{% endtab %}

{% tab title="Results" %}
## KPIs

| KPI Identifier | Description | Value Range | Unit |
| :--- | :--- | :--- | :--- |
| operating time | The amount of time component is active | inf | h |
| operating time.reducible | Amount of time component could be switched off \(outside of schedule\) | inf | h |
| operating time.reducible.relative | Percentage of time reducible relative to the total operating time | 0 to 100 | % |
| operating time.scheduled | Amount of time the component is active during schedule | inf | h |
{% endtab %}

{% tab title="Example" %}
This example shows a **schedule analysis** for a component "[fan](https://docs.aedifion.io/docs/engineers/specifications/component-data-models#fan)" connected to a supply fan operating message of a HVAC machine. The switch on/off times of the machine are shown as a blue line in the plot below, blue regions in the background correspond to the anticipated schedule.

![](../../.gitbook/assets/example_scheduleanalysis.png)

The following KPIs show that a reduction of ~9% of the total operating time is possible. With the help of the plot we can also see, that the times were we can reduce the operating time are distributed over the workdays of the week.

| KPI | Value | Unit |
| :--- | :--- | :--- |
| operating time | 74 | h |
| operating time.reducible | 6.94 | h |
| operating time.reducible.relative | 9.38 | % |
| operating time.scheduled | 67.1 | h |
{% endtab %}

{% tab title="Components" %}
## [fan](component-data-models.md#thermal-control-loop)

#### Pins

* operating message

#### **Attributes**

* schedule
{% endtab %}

{% tab title="Application" %}
## Recommended Time Span

* 1 week

## **Recommended Repetition** <a id="recommended-repetition-1-1"></a>

* every week
{% endtab %}
{% endtabs %}

## **Heating Curve Analysis**

{% hint style="warning" %}
Documentation currently under construction 🚧
{% endhint %}

## **Control Loop Oscillation Analysis**

{% hint style="warning" %}
Documentation currently under construction 🚧
{% endhint %}

## Information

The library of analytics functions is constantly expanded. If you are missing an analytics function, wish to implement your own functions, or want us to implement it for you, feel free to [contact us](../../contact.md#support).

