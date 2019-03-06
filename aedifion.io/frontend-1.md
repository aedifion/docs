---
description: >-
  Overview and documentation of the beta-frontend of the aedifion.io cloud
  platform.
---

# Frontend

## Overview

At the moment, aedifion provides a beta version of its web frontend offering access to some functionalities of aedifion.io, i.e. data exploration and limited data management, such as plotting, saving and calling saved plots, so-called views, adding favorites, adding [datapoint keys](../glossary.md#datapointkey) and renaming [datapoints](../glossary.md#datapoint).

{% hint style="info" %}
aedifion is currently developing an all-new frontend for holistic platform management and data access. 
{% endhint %}

## Data exploration

The frontend visualizes historical data and almost real time data using line and heat map plots. It plots up to seven lines in one line plot. One datapoint at a time can be plotted as heat map.

![Beta frontend with multiple lines plot](../.gitbook/assets/screenshot-beta-frontend.JPG)

![Beta frontend with heatmap plot of a CO2 time series over multiple weeks](../.gitbook/assets/screenshot-beta-frontend_heatmap.JPG)

The datapoint list is searchable.

![Searched datapoints list and multiple CO2 time series as line plot](../.gitbook/assets/screenshot-beta-frontend_search-co2.JPG)

The frontend supports switching projects as soon as one user has access to multiple projects.

## Saved views

After a user has generated a certain plot, he can add this plot to his so-called "saved views". This guarantees for easy-access to the most important views.

## Favorites

A datapoint can be marked as favorite. Once marked, the datapoint list can be sorted by favorites. Each favorite will be pinned at the top end of the list. This serves for easy access to the most important datapoints.

## Datapoint keys

The frontend supports multiple [datapoint keys](../glossary.md#datapointkey). A user can add new datapoint keys and can rename each datapoint. He can alter the datapoints list by datapoint keys.

## Meta data

Within the datapoint list, the frontend shows available meta data for each datapoint. 

![Meta data access](../.gitbook/assets/screenshot-beta-frontend_meta.JPG)

## 

This documentation continues with an introduction of the used data models in aedifion.io

