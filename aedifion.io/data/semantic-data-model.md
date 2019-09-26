---
description: >-
  Overview of the semantic data model used to structure time series and meta
  data of plants, buildings, districts, or energy systems in order to apply
  generic algorithms.
---

# Semantic data model

## Overview

The aedifion.io platform allows semantic and unified data modelling of [time series ](../../glossary.md#time-series)and meta data by mapping this data to [data models of components](../../glossary.md#component-data-model). Data models of components are based on real [components](../../glossary.md#component) of buildings and energy systems and thus enable intuitive modelling of such systems. The semantic data model allows generic application of analysis, control or any other algorithm using the semantic structured data as an input. It can be applied in third party algorithms via our [API](../apis.md).

In this article, we first provide information on [semantic modelling of energy systems](semantic-data-model.md#semantic-modelling) via component data models, followed by an explanation of the [mapping process](semantic-data-model.md#mapping-process) and application of the semantic data model  [for generic algorithms](semantic-data-model.md#application-in-algorithms).

## Semantic modelling

In the context of plants, buildings, districts, and energy systems, a granular approach of semantic modelling allows to master the individuality of such systems. For example a heating circuit can thus be composed of semantic component models of boiler\(s\), pump\(s\), valve\(s\), pipe\(s\), and heat consumer\(s\). Since those semantic models are describing [components](../../glossary.md#component), we named the data structures to store semantic data [component data models](../../glossary.md#component-data-model). Component data models provide generic placeholders describing a certain semantic to assign data to. [Pins](../../glossary.md#pin) are the placeholders for [datapoints ](../../glossary.md#datapoint)and their [time series ](../../glossary.md#time-series)data. [Attributes ](../../glossary.md#attribute)are the placeholders for meta data. To utilize a component data model for a specific [project](../../glossary.md#project), a component data model is instanced for this project. The [instanced component](../../glossary.md#instanced-component) is individualized for the project by [mapping](../../glossary.md#mapping) specific datapoints and meta data to their placeholders.

For more information view the list of [available component data models](../../engineers/specifications/component-data-models.md).

{% hint style="info" %}
To put it in a nutshell: Plants, buildings, districts, and energy systems are semantically modeled by individual sets of component data models. Mapping datapoints and attributes to those components structures time series and meta data for the modeled system.
{% endhint %}

### Mapping process

Mapping is the process of linking datapoints and meta data to the semantic data model of a specific project. Thus, it is the process of describing the assignment of datapints to pins and attributes to instanced component.

**Mapping datapoints to pins** of instanced components specifies the semantic of datapoints and their component affiliation. Information on a component affiliation can be taken from planning documentation, system overviews, and also native meta data generated and collected by aedifion.

**Mapping attributes to components** specifies the instanced component with information about dimension and characteristics of the component. Attribute values can be taken from data sheets of technical equipment, planning documentation of the energy system, or can simply be estimated.

As soon as an instanced component is mapped with datapoints and/or attributes, generic algorithms of the aedifion platform are applicable.

## Application in algorithms

The semantic structure of time series and meta data is the glue which connects data of a specific project and algorithms. To make this statement more tangible: If a datapoint of a specific project is semantically described to be the sensor of the supply temperature of a boiler in a specific project, any algorithm executing determinations on time series data from a supply temperature sensor of a boiler can use this information to address the specific datapoint. This means, algorithms use the component data model pins and attributes as input variables and the mapping information of instantiated components as the input variable values.

For more information on the availability of algorithms for a specific component data model, refer to [analysis functions](../../engineers/specifications/analytics.md).

