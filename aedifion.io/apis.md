---
description: Overview of the APIs of the aedifion.io cloud platform.
---

# APIs

## Overview

At aedifion, we firmly believe that any successful future building monitoring, analysis and control systems must be _open._ Such an open solution allows freely integrating, e.g., third party data sources, services, platforms, databases, output channels, and so forth. And, vice versa, it can also be integrated into third party services, platforms, databases, output channels and so forth.

The single most important feature of an _open_ platform is a comprehensive, well-documented, free and open application programming interfaces \(APIs\). The aedifion.io platform provides such APIs to any customer. 

This article provides a short overview of the APIs of the aedifion.io platform, their features and primary use cases.

## HTTP API

All data and functionality offered by the aedifion.io platform can be accessed and manipulated through a [RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer) HTTP API. This includes, e.g.,

* importing, querying, and exporting time series data
* creating and managing alerts on data and meta data
* management of users, projects, datapoints, and access permissions 
* creating and managing all kinds of meta data such as tags, renamings, favorites, etc.
* setpoint and schedule writing for active building control
* ...

The aedifion.io HTTP API is actively used in different places, e.g.,

* the frontend \([www.aedifion.io](https://www.aedifion.io)\) interacts with the backend through the HTTP APIs
* the [Excel plugin](integrations.md#excel) pulls data from the HTTP API into Excel worksheets
* the interactive [3D visualization](integrations.md#3d-visualization) uses the HTTP API to write setpoints
* voice commands to [Alexa](integrations.md#alexa) trigger calls to the HTTP API to retrieve thermal comfort and to write setpoints
* ...

_Learn more? Go through_ [_the step-by-step tutorials_](../tutorials/api/) _on how to interact with this API or explore the additional_ [_developer resources_](../developers/api-documentation.md)_._ 

## MQTT API

Most use cases of the aedifion.io platform center around collecting and acting upon time series data collected in buildings, plants, or whole districts. When such data should be streamed \(in real time\) in or out of the platform, the strict request/response pattern of HTTP APIs is often not the right choice. Thus, we complement the HTTP API by an MQTT API. MQTT follows a publish-subscribe model that provides convenient and efficient streaming access to live data. The features of the MQTT API include, e.g.,

* subscribing to all measurement data collected from your buildings, plants, and districts at the level of whole projects or down to single datapoints
* publishing data into the aedifion.io platform via MQTT
* receiving  to notifications and alarms via MQTT
* websockets support
* ...

The aedifion.io MQTT API is actively used in different places, e.g.,

* by aedifion [edge devices](gateway.md) to send collected data and meta data to the aedifion.io platform
* the interactive [3D visualization](integrations.md#3d-visualization) subscribes to live data via MQTT directly from within your browser using websockets
* the aedifion.io platform publishes alarms created by the user on datapoints via MQTT

_Learn more? Go through_ [_the step-by-step tutorials_](../tutorials/mqtt/) _on how to interact with the MQTT API or explore the additional_ [_developer resources_](../developers/mqtt-api.md)_._ 

## 



