---
description: >-
  Overview of integrations with and extensions of the aedifion.io cloud
  platform.
---

# Integrations

{% hint style="danger" %}
Under construction
{% endhint %}

## Excel

## Chatbots

## 3D Visualization

## Alexa

## Cloud services

### Cumulocity

### AWS

...

## Weather data

This chapter explains the free of charge integration of weather \(prediction\) data for a specific project.

### Overview

The aedifion.io platform allows integration of localized weather data from external weather services. Once activated, the weather data will be accessible and visualizable on the platform like any other datapoint on the platform. Weather data include current and predicted states with different prediction horizons of several meteorological conditions, e.g., temperature or dew point. 

The scope of this article is to explain our basic concept of weather data integration. In case your use-case is not covered by the weather data services provided, do not hesitate to [contact ](https://docs.aedifion.io/docs/contact)us.

We are using the services of [the Dark Sky Company, LLC](https://darksky.net/) for weather data integration.

### Meteorological conditions

This list provides the meteorological conditions which can be integrated by default:

<table>
  <thead>
    <tr>
      <th style="text-align:left">​Meteorological condition</th>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">
        <p>Unit /</p>
        <p>Value set</p>
      </th>
      <th style="text-align:left">Info</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Apperent ("feels like") temperature</td>
      <td style="text-align:left">apparentTemperature</td>
      <td style="text-align:left">°C</td>
      <td style="text-align:left">Human felt temperature, determined by air temperature, wind speed, and
        humidity.</td>
    </tr>
    <tr>
      <td style="text-align:left">Cloud coverage ratio</td>
      <td style="text-align:left">cloudCover</td>
      <td style="text-align:left">[0, 1]</td>
      <td style="text-align:left">Ratio of sky occluded by clouds.</td>
    </tr>
    <tr>
      <td style="text-align:left">Dew point</td>
      <td style="text-align:left">dewpoint</td>
      <td style="text-align:left">°C</td>
      <td style="text-align:left">Ambient steam saturation temperature.</td>
    </tr>
    <tr>
      <td style="text-align:left">Relative humidity</td>
      <td style="text-align:left">humidity</td>
      <td style="text-align:left">[0, 1]</td>
      <td style="text-align:left">Ambient ratio of steam saturation.</td>
    </tr>
    <tr>
      <td style="text-align:left">Quantity of ozone</td>
      <td style="text-align:left">ozone</td>
      <td style="text-align:left">DU</td>
      <td style="text-align:left">Quantity of ozone substance over an area unit in Dobson Unit.</td>
    </tr>
    <tr>
      <td style="text-align:left">Precipitation intensity</td>
      <td style="text-align:left">precipIntensity</td>
      <td style="text-align:left">mm/h</td>
      <td style="text-align:left">Amount of precipitation per time unit.</td>
    </tr>
    <tr>
      <td style="text-align:left">Precipitation probability</td>
      <td style="text-align:left">precipProbability</td>
      <td style="text-align:left">[0, 1]</td>
      <td style="text-align:left">Precipitation probability based on historical meteorological conditions.</td>
    </tr>
    <tr>
      <td style="text-align:left">Sea level air pressure</td>
      <td style="text-align:left">pressure</td>
      <td style="text-align:left">hPA</td>
      <td style="text-align:left">Air pressure, measured at the height of the weather station, reduced to
        sea level.</td>
    </tr>
    <tr>
      <td style="text-align:left">Temperature</td>
      <td style="text-align:left">temperature</td>
      <td style="text-align:left">°C</td>
      <td style="text-align:left">Ambient air temperature.</td>
    </tr>
    <tr>
      <td style="text-align:left">UV index</td>
      <td style="text-align:left">uvIndex</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left">Defined by WMO, WTO, and ICNIRP commision.</td>
    </tr>
    <tr>
      <td style="text-align:left">Average visibility</td>
      <td style="text-align:left">visibility</td>
      <td style="text-align:left">km</td>
      <td style="text-align:left">Measurement of the transparency of ambient air.</td>
    </tr>
    <tr>
      <td style="text-align:left">Wind direction</td>
      <td style="text-align:left">windBearing</td>
      <td style="text-align:left">° [0,360]</td>
      <td style="text-align:left">Direction <b>from</b> which the wind is coming. 0° at true north, clockwise.
        Not defined for wind speed = 0.</td>
    </tr>
    <tr>
      <td style="text-align:left">Wind gust speed</td>
      <td style="text-align:left">windGust</td>
      <td style="text-align:left">m/s</td>
      <td style="text-align:left">Maximum gust speed.</td>
    </tr>
    <tr>
      <td style="text-align:left">Wind speed</td>
      <td style="text-align:left">windSpeed</td>
      <td style="text-align:left">m/s</td>
      <td style="text-align:left">Horizontal wind speed.</td>
    </tr>
  </tbody>
</table>We store every meteorological condition as a separate datapoint on the aedifion.io platform to historicize its state. More on this in the subchapter [Datapoint and observation convention](integrations.md#datapoint-and-observation-convention).

### Prediction horizons

Besides the current state of a meteorological condition some use-cases require prediction data. We offer hourly predictions up to 168 hours \(7 days\) in the future and update them every hour. On ordering you can flexibly choose which horizons you need. The predicted states are aligned to the top of the prediction’s timestamp.

We combine every prediction horizon with the meteorological state monitored to create unique datapoints. You can use the unique datapoints to address the historicized meteorological conditions and their predications on the platform. More on this in the subchapter [Datapoint and observation convention](integrations.md#datapoint-and-observation-convention).

### Datapoint and observation convention

Like any other datapoint on the aedifion.io platform the weather datapoints are identifiable by an alphanumeric identifier which is unique for each project. The timeseries data for particular weather datapoints is stored as observations with a tuple of _value_ and _timestamp_.

 The naming convention for weather datapoints is: 

_aedifion\_weather-&lt;name of meteorological condition&gt;\_&lt;preiction horizon&gt;_

How we handle predictions: Every predication exists of a predicted value and the timestamp in the future the predication is made for. This timestamp is equal to the prediction horizon. We hold on to this _prediction value_ and _prediction horizon_ combination to make predictions accessible on aedifion.io. It’s easier to explain in an example:

{% hint style="info" %}
Imagine the following setup:

* The project _aedifion office_ wants to use temperature and _dewpoint_ conditions.
* This data is needed for a prediction horizon of _1h_ and _3h_ in the future.
* The current date time is 2020-02-20 22:00:00.

Now, how would the datapoint string-id for _temperature_ with _1h_ prediction be, and which timestamps will be used for the most recent predictions of the dewpoint datapoints at the current date time?

* The datapoint string-id for temperature with 1h prediction horizon will be _aedifion\_weather-temperature\_1h_ .
* For datapoint _aedifion\_weather- dewpoint \_1h_: 2020-02-20 23:00:00
* For datapoint _aedifion\_weather- dewpoint \_3h_: 2020-02-21 01:00:00
{% endhint %}





