---
description: We introduce the aedifion.io platform and its components in this chapter.
---

# aedifion.io

## 30,000 ft overview 

aedifion.io is an IoT platform tailored for heating, ventilation & air conditioning \(HVAC\) systems, energy-related plants, and energy networks, such as district heating and cooling grids. 

Its main purpose is to provide everything it needs for energy and cost efficient system operation with high effectiveness \(in terms of need fulfillment, e.g. thermal comfort, indoor air quality, and other generic energy services\). 

aedifion.io's leading use case is to connect to a local plant, file all available data, use analytics to gain insights, and use controls to optimize local plants' operation.

![Overview of the aedifion.io ecosystem](../.gitbook/assets/aedifion.io-overview%20%281%29.png)

We introduce aedifion.io's main ingredients in the following, along the schematic overview of the aedifion.io ecosystem, which you see above. 

At its heart, boxed in red, the aedifion.io platform provides data filing and processing, AI & domain specific [analytics](../aedifion.analytics.md), [control functionalities & algorithms](../aedifion.controls.md), data management and structuring, role-based user management, and many more [features](features.md), all dedicated to energy system operation and optimization. 

The aedifion [edge device](gateway.md) provides plug-and-play connectivity to building automation and control systems \(BACS\) and to automation systems of energy-related plants, such as e.g. larger scale combined heat and power plants, air handling units, heat pumps, etc.

The aedifion [frontend ](frontend.md)is a browser-based human machine interface \(HMI\). It offers data visualization, data management, platform administration and provides access to various further [features](features.md).

aedifion.io's [application programming interfaces ](apis.md)\(APIs\) offer access to all mayor platform features and functionalities. If you are a techie, you can operate the platform directly over APIs. If you are a software developer, please feel free to program you own applications based on the aedifion.io API. 

Based on these APIs, aedifion offers applications like its floor plan app and its [3D HMI](integrations.md#3d-hmi), as well as services like control algorithm development, and custom-purpose AI development. as well as consulting. 

aedifion's partners use aedifion.io to offer building, multi-building and district-wide energy management as well as building performance optimization as-a-service.

aedifion.io integrates 3rd party data, such as e.g. [weather predictions](integrations.md#weather-data) and [Microsoft Exchange](integrations.md#microsoft-exchange) serves, connects to 3rd party platforms and [cloud services](integrations.md#cloud-services), such as Cumulocity, and is integrated into 3rd party applications, such as [Microsoft Excel](integrations.md#excel). Further, it offers full integrations of 3rd party apps like [Grafana](https://grafana.com/).  Moreover, [chat bots](integrations.md#chatbots) in external messaging systems, like Telegram etc., serve as output channel for aedifion.io's alarming system.

aedifion has defined the following further products, that all use aedifion.io as their base technology.

## Further products

[aedifion.analytics](../aedifion.analytics.md) features deep analytics for technical equipment within your plant. It focuses on the calculation of key performance indicators for components within local plants and the derivation of recommendations for action. See [aedifion.analytics](../aedifion.analytics.md) for detailed information.

[aedifion.controls](../aedifion.controls.md) provides a framework of basic control functionalities that are a prerequisite for modern controls as well as predefined control algorithms, such as optimizer for air handling unit operations or MS Exchange augmented room controls. See [aedifion.controls](../aedifion.controls.md) for detailed information

aedifion.custom adapts .io, .analytics, and .controls to your needs with e.g. picked modules and functionalities as well as tailored APIs. [Contact us](../contact.md) to discuss your requirements.

## aedifion.io in detail

Within the next subpages, you will find more in-depth information about aedifion.io:

* [Features ](features.md)gives an overview of all relevant key functionalities of aedifion.io. 
* [Edge device](gateway.md) explains what the edge device is and how it works.
* [APIs](apis.md) introduces the aedifion.io’s APIs. 
* [Frontend](frontend.md) introduced introduces the main platform HMI.
* [Data](data.md) explains the main specifications of used data models.
* [Integrations](integrations.md) describes integrations to aedifion.io
* [Security ](security.md)discusses aedifion's approaches to deliver high ICT security.



**We go on with introducing aedifon.io's** [**features**](features.md) **on the next page.**



