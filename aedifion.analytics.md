---
description: >-
  aedifion.analytics offers deep insights into operational quality of energy
  systems and their plants. This chapter provides an introduction to this
  service.
---

# aedifion.analytics

## Overview

aedifion.analytics is a data-driven service for deep analysis of heating, ventilation & air conditioning \(HVAC\) systems, energy-related plants, buildings, as well as energy networks such as district heating and cooling grids. This service is an embedded functionality of the aedifion.io cloud platform. 

The goal of analytics is to support technicians and engineers who want to optimize theses systems in respect of indoor comfort, energy efficiency, and maintenance expenditure. For this purpose, aedifion.analytics creates a profound transparency of system operation through extended visualization options, data aggregation to concise key performance indicators \(KPIs\) and automated fault detection and diagnosis \(FDD\) including the recommendation of measures for optimization.

### Fault detection and diagnosis \(FDD\)

FDD is a failure and operational analysis of energy systems and their individual plants. For this purpose, the operational data of the systems under consideration are analyzed, interpreted, and evaluated with the help of engineering knowledge in order to ultimately identify measures for system optimization. aedifion.analytics automates this process. Its configuration is straightforward:

1. Choose the components of the energy system.
2. Map datapoints to components with the help of the AI mapping service of aedifion.
3. Choose analysis functions of interest.

Once configured, the FDD is performed automatically: Engineering algorithms and artificial intelligence algorithms analyze the operation of the plant under investigation. The analysis results are evaluated in the "decision engine" by comparing them with digitalized engineering knowledge of the "knowledge & fault pattern database". First, the analysis result is interpreted, and a decision is made as to whether the operation is suboptimal or faulty at all. Once such an operation has been identified, the knowledge of the "knowledge & fault pattern database” is used to recommend measures for its correction and prioritize them according to their impact.

![TEMP! aedifion.analytics process](.gitbook/assets/tmp_analytics.png)

### Key performance indicators \(KPIs\)

A significant part of the engineering algorithms of aedifion.analytics FDD service is realized by data aggregation to KPIs. These KPIs are known from thermodynamics and engineering and target one purpose: **Quick overview.** They summarize the operational behavior of the observed system within the time interval under consideration. Therefore, they are also suitable for a manual operation analysis or for documentation of the operating behavior. 

Such KPIs can be quite trivial like a maximum temperature of a specific measuring point, more advanced like a coefficient of performance for a heat pump or very high level like energy demand per square meter and year of a building. These examples show the varying degrees of data aggregation and abstraction that a KPI can provide to describe the operational quality of a system.

### Advanced visualization and timeseries manipulation

In addition, aedifion.analytics offers advanced timeseries visualizations and manipulations for manual analysis and documentation that extend beyond the basic visualization functions of aedifion.io. An example for this timeseries manipulation is the rearrangement of a timeseries of a power sensor into a load duration curve for a yearly energy report of a combined heat and power plant, or the determination of head fluxes from two temperatures and a volume flow sensor. The manipulation of timeseries allows a use-case driven focus on certain aspects which - if visualized - can be analyzed immediately. E.g., back to the load duration example: A visualized load duration curve will provide information over the operating hours and load distribution of the plant right away and therefor helps to interpret the dimensioning and system integration of this plant. This information is hardly recognizable from the original timeseries of the power sensor.

