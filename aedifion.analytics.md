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

A component can be instantiated for a project. This means that e.g. a project A has a component _control loop_ B. After instantiating, time series data can be mapped to the pins of a component.

A pin of a component is...

Additionally, each component has or can have certain meta data.

_Learn more? Explore the_ [_available components_](engineers/specifications/analytics.md) _and_ corresponding __[_API endpoints_](developers/api-documentation/guides-and-tutorials/analytics.md)_._

### Analysis functions

### Analysis config

### Knowledge & fault pattern databse

### Decision engine

### Analysis runtime

### Results

## Process 

### Mapping

Mapping is the process of connecting a components pin to a time series.

To adjust a [generic component model](aedifion.analytics.md#components) to a specific project it only requires the mapping of datapoints to the corresponding pins of this component. The [data management functionalities](aedifion.io/features.md#data-management-and-structuring) and [meta-data](aedifion.io/features.md#meta-data) available on aedifion.io are supporting this process.

_Learn more? Try the_ [_API tutorial._](developers/api-documentation/guides-and-tutorials/analytics.md)\_\_

## Example

Analysis functionality is offered component wise. Therefore, choose the components of interest from the growing library of components, e.g., the component _boiler_. These components are generic data models which describe the link between individual data points and specific plants, e.g., the link of the component _boiler_ to its _supply temperature_ and _return temperature_ sensor. This generic link is called _pin._

## Use cases

