---
description: Detailed specification of available analytics functions and their application.
---

# Analytics

## Available analysis functions

* \*\*\*\*[**Operating Cycle Analysis**](analytics.md#operating-cycle-analysis)\*\*\*\*
* \*\*\*\*[**Schedule Analysis**](analytics.md#schedule-analysis)\*\*\*\*

## Application notes

* **Unit sensitivity:** To this state, our algorithms are unit sensitive. Every [pin ](../../glossary.md#pin)and [attribute ](../../glossary.md#attribute)is specified with an unit. Mind the specifications.

{% hint style="danger" %}
If unit conventions are disregarded, this can lead to errors and even misleading results of algorithms.
{% endhint %}

* **Improving results:** We continuously improve our analyses functions, interpretations and recommendations. Thus, performing exactly the same analysis at a later point in time can lead to different results.

## Analysis Results

{% hint style="warning" %}
Documentation currently under construction 🚧
{% endhint %}

## Example Analysis

This example leads through our description of [analytics functions](../../glossary.md#analysis-function) by providing exemplary values and descriptions. The first paragraph of an analysis function provides a short introduction to the analysis.

{% tabs %}
{% tab title="Description" %}
#### **Value**

* Quick entry into analytics function specifications
* Exemplary template for analysis function specifications and documentation structure



#### **Concept**

In general the description provides information about the concepts of the analysis function, its use-case scenario, and value. 
{% endtab %}

{% tab title="Application" %}
_The **Application** tab offers further information for real application of the analysis._

\_\_

#### Recommended Time Span

* 4 weeks

_The **Recommended Time Span** gives the recommended time span for  which the analysis function is intended. The time span is oriented to the time constant of the phenomenon investigated, e.g. in order to analyse weekly accruing patterns, a time span of 4 weeks is recommended. In shorter periods of time, the analysed phenomenon might not occur, while longer periods might lead to superposition of other effects such as seasonal influences and thus blurring of the analysis results. **Of course, analysis functions can be used over flexible periods of time at your personal discretion.**_

_\*\*\*\*_

#### **Recommended Repetition**

* every 3 months

_The Recommended Repetition provides a recommendation when the analysis should be repeated. The recommendation refers to a continuous operation of the analysed system. In the case of changes to the system, both physical and operational, a repetition of the analysis is generally recommended. **Of course, analysis can be repeated at your personal discretion.**_

_\*\*\*\*_

#### Another recommendation

...
{% endtab %}

{% tab title="KPIs" %}
_The **KPIs** tab provides the list of KPIs determined by the analysis function._

\_\_

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
### Pins

* example pin
* ...

_Most of the analysis functions require a mapping for the same pins regardless of the component data model. These pins are listed here. Component-related variations, exceptions, and deeper insights are provided component-wise further down this chapter._

\_\_

### **Attributes**

* example attribute
* ...

_Some of the analysis functions require attributes regardless of the component data model. These attributes are listed here. Component-related variations, exceptions, and deeper insights are provided component-wise further down this chapter._

\_\_

### [Example Component](component-data-models.md)

The short text snippets below a component provide information about how to utilize the analysis function for this component. In case you are new to [component data models](../../glossary.md#component-data-model), we recommend to read the [high-level introduction to components](../../aedifion.io/data/semantic-data-model.md) and check out the [component data models](component-data-models.md) for deeper insights.



### [Another Example Component](component-data-models.md#example-component)

...
{% endtab %}
{% endtabs %}

## Operating Cycle Analysis

{% hint style="warning" %}
Documentation currently under construction 🚧
{% endhint %}

## Schedule Analysis

{% hint style="warning" %}
Documentation currently under construction 🚧
{% endhint %}

## Information

The library of analytics functions is constantly expanded. If you are missing an analytics function, wish to implement your own functions, or want us to implement it for you, feel free to [contact us](../../contact.md#support).

