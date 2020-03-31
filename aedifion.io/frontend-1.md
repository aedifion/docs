---
description: Overview and documentation of the frontend of the aedifion.io cloud platform.
---

# Frontend

## Overview

With every project aedifion provides a web-based frontend offering access to some functionalities of aedifion.io, i.e. company- and user-management, data exploration, data management, tagging and filtering functionality, setpoint writing, schedule writing, component mapping or analytics results viewing. These functionalities are described below as an overview.

![landing page with project overview](../.gitbook/assets/image%20%2834%29.png)

It is developed and tested for current standard-browsers like Chrome, Firefox, Safari and Edge. It is optimized Desktop PCs, Notebooks and Tablets.

## Company- and user-management

From every subpage accessible are the account-management functionalities. Depending on the logged-in user's rights, configurations on company and user-level can be done, like creating new users, granting access to projects, creating and deleting projects and so on. Also you can switch between two languages, english and german.

![](../.gitbook/assets/image%20%2837%29.png)

## Data exploration and data management

The frontend visualizes historical data and real time data using line and heat map plots. Up to seven lines can be plotted in one line plot. One datapoint at a time can be plotted as heat map.

![Line plotting of two datapoints, metrics of the timeseries in the picked interval below, multiple y-axis](../.gitbook/assets/image%20%2823%29.png)

![heatmap plot of a CO2 time series over multiple weeks](../.gitbook/assets/image%20%2825%29.png)

## Tagging and filtering functionality

Beside a plain text datapoint search, filtering via tags is provided. AI-classification tags, user-added tags and Building Automation protocol-tags are used. Access the list of tags of each datapoint by clicking on the pen-symbol next to the datapoint's name.

![tags of a single datapoint; the list of datapoints can filtered by single tag or by a combination of tags](../.gitbook/assets/image%20%284%29.png)

Plotviews are a list of saved views of specific datapoints and can easibily be accessed by the bookmark-button next to the user's menu. Beside of that is also a star-button for prominent filtering of the list of datapoints after user's favourised datapoints. Add a new plotview by clicking on the plus-button in the lower right corner of each plotting window.

![list of plotviews](../.gitbook/assets/image%20%2814%29.png)

## Setpoint writing

By clicking on the pen-symbol next to a writeable datapoint, setpoint writing options are shown above the datapoint's specific tags.

![priority based writing functionality including a writing history](../.gitbook/assets/image%20%2831%29.png)

## Schedule writing

Possibility to setup and configure schedules regardless of the used building automation protocol. For example, this can be used to switch on an Air Handling Unit in the morning, switch it off in the evening and make it recurring daily.

![Setup and track schedules on specific datapoints](../.gitbook/assets/image%20%281%29.png)

## Component mapping

Choose one of the existing, generic components and map the related pins to your project specific datapoints by the use of the AI classification results. Mapped components are the basis for further analytics and automatically created dashboards, just to name two of many usecases.

![component thermal control loop with four already mapped pins](../.gitbook/assets/image%20%2815%29.png)

![Mapping of the pin temperature of the component weather station with the use of AI classification results ](../.gitbook/assets/image%20%282%29.png)

## Analytics results

Get an overview of your components. They are sorted according to the quality of the belonging analysis results. By selecting one component you see the timeseries plots of the mapped datapoints and a list of belonging analysis results. By clicking on each results you get an interpretation with the underlying KPIs and a recommendation, what could be improved.

![sorted list of components on the left hand side and analytics results, interpretations and recommendations](../.gitbook/assets/image%20%2841%29.png)

_This documentation continues with an introduction of the used data models in aedifion.io_

