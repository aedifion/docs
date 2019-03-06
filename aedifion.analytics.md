---
description: >-
  aedifion.analytics offers deep insights into operational quality of energy
  systems and their plants. This chapter provides an introduction to this
  service.
---

# aedifion.analytics

## Introduction

aedifion.analytics is a framework for data analytics of heating, ventilation & air conditioning \(HVAC\) systems, energy-related plants, buildings, as well as energy networks such as district heating and cooling grids. The framework has a special focus on scalable analytics of large sets of time series data.

The goal of aedifion.analytics is to support technicians and engineers who want to optimize \(or commision\) systems in respect of indoor comfort, energy efficiency, and maintenance expenditure. For this purpose, aedifion.analytics aims at profound transparency and interpretation of system operation and is continuously extended.

 Its configuration and analysis workflow is straightforward:

1. Instantiate [components ](aedifion.analytics.md#components)of the energy system.
2. [Map data points](aedifion.analytics.md#mapping) to components.
3. Choose [analysis functions](aedifion.analytics.md#analysis-functions) of interest to [configure analysis](aedifion.analytics.md#analysis-config) on the chosen components.
4. Get and [explore analytics results](aedifion.analytics.md#explore-analysis-results).

{% hint style="info" %}
[Ingested ](aedifion.io/features.md#data-ingress)as well as [AI-generated](aedifion.io/features.md#ai-generated-meta-data) meta data can be used to support the mapping process.
{% endhint %}

Please refer to the figure below for a schematic overview of the aedifion.analytics framework.

![Schematic overview of aedifion.analytics](.gitbook/assets/tmp_analytics.png)

In the following, we will explain the ingredients of aedifion.analytics. 

## Technical overview

The framework aedifion.analytics consists of a component library, a library of algorithms for data analytics, a knowledge & fault pattern database, a decision engine, an analysis configuration pattern, and an analysis runtime environment. 

All required interaction is available via APIs. 

### Components

Components are generic data models of physical, virtual or logical objects within a building or energy-related plants, such as e.g. pumps, boilers, thermal zones, control loops and so forth. The aedifion component library inherits all contemporary available components within aedifion.analytics.

A component can be instantiated for a specific project. This means that e.g. a _project A_ has a component _control loop B_. After instantiating, time series data can be [mapped ](aedifion.analytics.md#mapping)to the pins of a component.

A pin of a component represents an expected data point of this component. It is a generic placeholder for [mapping ](aedifion.analytics.md#mapping)a data point to an instantiated component within a specific project. Analyses are performed on these generic pins. E.g., the generic component _control loop_ has the pin _controller output_. As soon as the component gets instanced in _project A_ as the component _control loop B_ the data point _bacnet-101\_cont loop B-DO2_ can get mapped to the pin _controller output_ and is therefore available for analysis on the pin _controller output_.

Additionally, each component has or can have certain meta data.

_Learn more? Explore the_ [_available components_](engineers/specifications/analytics.md) _and_ corresponding __[_API endpoints_](developers/api-documentation/guides-and-tutorials/analytics.md)_._

### Analysis functions

Analysis functions are granular and generic algorithms to analyse the operation of building equipment or energy-related plants. The aedifion analysis function library inherits all contemporary available analysis functions within aedifion.analytics.

Analysis functions are available per component model and get executed on [mapped ](aedifion.analytics.md#mapping)pins and meta-data of instanced [components](aedifion.analytics.md#components). E.g., an analysis of plant cycles is available for several components like heat pump, air handling unit, boiler and so forth. This analysis requieres a [mapping ](aedifion.analytics.md#mapping)of the pin _operating message_ of the analyzed component.

_Learn more? Explore the_ [_available analysis functions_](engineers/specifications/analytics.md) _and corresponding_ [_API endpoints_](developers/api-documentation/guides-and-tutorials/analytics.md)_._

### Analysis runtime

* gets config, executes it, provides & restructures analysis results
* depending on the ana-fct it gets decision & knowledge from other services

### Knowledge & fault pattern databse

* knowledge: decision thresholds, interpretations, recommendations, knowledge about fault patterns in plant operation

### Decision engine

* compars analysis results with knowledge to decide on operational quality

### Results

* via API, can be used by several services
* optimization measures, etc.

## Process 

### Mapping

Mapping is the process of connecting a components pin to a data point, respectively its time series.

To adjust a [generic component model](aedifion.analytics.md#components) to a specific project it only requires the mapping of data points to the corresponding pins of this component. The [data management functionalities](aedifion.io/features.md#data-management-and-structuring) and [meta-data](aedifion.io/features.md#meta-data) available on aedifion.io are supporting this process.

_Learn more? Try the_ [_API tutorial._](developers/api-documentation/guides-and-tutorials/analytics.md)\_\_

### Configuring analysis

Configuring analysis is the process to individualize the analysis which should be run on an instanced [component](aedifion.analytics.md#components). Choices are:

* Which [analysis functions](aedifion.analytics.md#analysis-functions) shall be run on the component? This can be a subset of the analysis functions available for the component model.
* It is possible to define several configurations on one instanced component and thus create individual analysis sets.
* Advanced settings: Analyse multi-time intervals. This option allows to perform analyses over a fixed number of time intervals, a fixed interval length or a combination of both. _Learn more on mult-time intervals? Try the_ [_API tutorial_](developers/api-documentation/guides-and-tutorials/analytics.md)_._

The configuration will be passed to the [analytics runtime](aedifion.analytics.md#analysis-runtime) when [analysis results](aedifion.analytics.md#results) of this configuration are queried.

_Learn more? Try the_ [_API tutorial_](developers/api-documentation/guides-and-tutorials/analytics.md)_._

## Example

Analysis functionality is offered component wise. Therefore, choose the components of interest from the growing library of components, e.g., the component _boiler_. These components are generic data models which describe the link between individual data points and specific plants, e.g., the link of the component _boiler_ to its _supply temperature_ and _return temperature_ sensor. This generic link is called _pin._

## Use cases

