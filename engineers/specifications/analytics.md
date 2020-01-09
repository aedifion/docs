---
description: Detailed specification of available analytics functions and their application.
---

# Analytics

## Available analysis functions

* \*\*\*\*[**Control Loop Oscillation Analysis**](analytics.md#control-loop-oscillation-analysis)\*\*\*\*
* \*\*\*\*[**Heat Flux Analysis**](analytics.md#heat-flux-analysis)\*\*\*\*
* \*\*\*\*[**Heating Curve Analysis**](analytics.md#heating-curve-analysis)\*\*\*\*
* \*\*\*\*[**Operating Cycle Analysis**](analytics.md#operating-cycle-analysis)\*\*\*\*
* \*\*\*\*[**Outside Air Temperature Sensor Analysis**](analytics.md#outside-air-temperature-sensor-analysis)\*\*\*\*
* \*\*\*\*[**Room Air Quality Analysis**](analytics.md#room-air-quality-analysis)\*\*\*\*
* \*\*\*\*[**Schedule Analysis**](analytics.md#schedule-analysis)\*\*\*\*
* \*\*\*\*[**Setpoint Deviation Analysis**](analytics.md#setpoint-deviation-analysis)\*\*\*\*
* \*\*\*\*[**Temperature Spread Analysis**](analytics.md#temperature-spread-analysis)\*\*\*\*

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

## **Control Loop Oscillation Analysis**

{% hint style="warning" %}
Documentation currently under construction 🚧
{% endhint %}

## **Heat Flux Analysis**

{% hint style="warning" %}
Documentation currently under construction 🚧
{% endhint %}

## **Heating Curve Analysis**

{% hint style="warning" %}
Documentation currently under construction 🚧
{% endhint %}

## Operating Cycle Analysis

The **Operating Cycle Analysis** investigates and interprets the cycle behavior of components. Besides identifying sub-optimal cycle behavior, the algorithm provides recommendations on how to improve cycle rates, and KPIs for deeper insides.

{% tabs %}
{% tab title="Quick Start" %}
## **Value**

* Lower operating costs
* Higher energy efficiency
* Longer equipment and component life times
* Smoother system integration

## Recommended for component types

* Energy conversion plants
* Components with high start-up expenses
* Heat pumps
* Combined heat and power
* etc.
{% endtab %}

{% tab title="Description" %}
The **Operating Cycle Analysis** aims towards a more constant operation of components and thereby towards a reduction of operational costs. Frequent start and stop processes lead to energy losses and higher wear and tear of the component compared to a constant operation. Further, a frequently alternating operation of a component, e.g. a heat pump, has negative effects on adjacent components, which are enforced to alternate as well. At the same time, too low cycle rates are an indication of a possible under-supply.

The analysis function identifies excessive as well as too low cycle rates on the basis of the datapoint _operating message_ of the analysed component. It provides interpretations of the current cycle behavior and recommendations on how to improve it. The recommended measures include adaptions of control parameter, simple structural measures, as well as instructions for diagnosis and adjustment of the cause of sub-optimal cycle behavior. Additionally to the automated generation of interpretations and recommendations, the algorithm offers a set of KPIs. They provide deeper insides in the cycle behavior of the analysed component.
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

Recommendation texts on how to improve cycle rates of the analysed component including adjustment of control parameter as well as recommendations on how to investigate the deeper cause for unpleasant cycle behavior.

## KPIs

Providing deeper insights to the cycle behavior. KPIs support human reasoning.

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

![Figure 1: Plot of operating message time series from 12:00 am till 6:00 pm](../../.gitbook/assets/operating_cycle_analysis.png)

The short shut-down times between periods of duty are conspicuous. This is a hint for unnecessary frequent start and stop processes of the heat pump, leading not only to energy losses but also to high wear and tear of the heat pumps scroll compressor.

In order to apply the **Operating Cycle Analysis** for the instanced and mapped heat pump, we defined an analysis config via the aedifion API. Executing the **Operating Cycle Analysis** for the same period of time as figure 1 returns following results:

**Qualitative warning level:** red \(as in traffic light signal color\)

**Interpretation:** Highly increased number of start procedures/operation cycles. A reduction of the starting procedures or a more continuous operation is recommended.

**Recommendations:**

* Check whether the system flow temperature runs into the safety limiter and causes safety shutdowns.
* Check whether the heat requirement is covered too quickly by the producer. If yes, check the modulation operation of the system and, if available, the integration of the buffer tank and the interconnected operation of several generation systems.
* Check the installation of a power choke. By reducing the electrical power consumed, the amount of heat provided can be reduced and thus the effects of an oversized plant. Pay attention to the manufacturer's specifications as to whether the installation of a choke is technically possible.

**KPIs:**

| KPI | Value | Unit |
| :--- | :--- | :--- |
| operating time | 4.31 | h |
| operating time.relative | 71.8 | % |
| starts | 16 | count |
| closed operating cycles | 15 | count |
| cycle times.median | 0.36 | h |
| cycle times.average | 0.369 | h |
| cycle times.maximum | 0.41 | h |
| cycle times.minimum | 0.34 | h |
| duty times.median | 0.254 | h |
| duty times.average | 0.264 | h |
| duty times.maximum | 0.304 | h |
| duty times.minimum | 0.234 | h |
| switch-off times.median | 0.105 | h |
| switch-off times.average | 0.106 | h |
| switch-off times.maximum | 0.106 | h |
| switch-off times.minimum | 0.105 | h |

The automated interpretation confirms our visual analysis of the time series shown in figure 1, summed up by the qualitative warning level "red". The recommendations provide further instruction on how to isolate and fix the cause for the increased number of start and stop processes.

Further, the result offers an advanced set of KPIs, providing additional insights into the cycle behavior of the heat pump. They support human reasoning for case-by-case analysis. For example, the similarity of switch-off times per cycle are peculiar. Since a coincidence would be surprising for such a dynamically operated plant, one could reason, that the heat pump is permanently requested by the higher level controller, but runs into a condition causing a component shutdown for a predefined period of time. Deeper human investigation of the condition is possible via data visualization of aedifion.io and the combination to insights of other analysis functions.
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

### Every months

* Cycle rates have a strong seasonal effect
* Frequent repetition allows to identify operational bad points
{% endtab %}
{% endtabs %}

## **Outside Air Temperature Sensor Analysis**

{% hint style="warning" %}
Documentation currently under construction 🚧
{% endhint %}

## **Room Air Quality Analysis**

{% hint style="warning" %}
Documentation currently under construction 🚧
{% endhint %}

## Schedule Analysis

{% hint style="warning" %}
Documentation currently under construction 🚧
{% endhint %}

## **Setpoint Deviation Analysis**

{% hint style="warning" %}
Documentation currently under construction 🚧
{% endhint %}

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

## Information

The library of analytics functions is constantly expanded. If you are missing an analytics function, wish to implement your own functions, or want us to implement it for you, feel free to [contact us](../../contact.md#support).

