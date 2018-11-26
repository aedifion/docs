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

## Weather data

This chapter explains the free of charge integration of weather \(prediction\) data for a specific project.

### Overview

The aedifion.io platform allows integration of localized weather data from external weather services. Once activated, the weather data will be accessible and visualizable on the platform like any other datapoint on the platform. Weather data include current and predicted states with different prediction horizons of several meteorological conditions, e.g., temperature or dew point. 

The scope of this article is to explain our basic concept of weather data integration. In case your use-case is not covered by the weather data services provided, do not hesitate to [contact ](https://docs.aedifion.io/docs/contact)us.

We are using the services of [the Dark Sky Company, LLC](https://darksky.net/) for weather data integration.

### Meteorological conditions

This list provides the meteorological conditions which can be integrated by default:

| ​Meteo. cond. | Name | Unit | Info |
| :--- | :--- | :--- | :--- |
| Apperent \(felt\) temperature | apparentTemperature |  |  |
| Percentage of cloud coverage | cloudCover |  |  |
| Dew point | dewpoint |  |  |
| Relative humidity | humidity |  |  |
| Quantity of ozone | ozone | DU \(Dobson unit\) |  |
| Precip intensity | precipIntensity |  |  |
| Precip probability | precipProbability |  |  |
|  | pressure |  |  |
|  | temperature |  |  |
|  | uvIndex |  |  |
|  | visibility |  |  |
|  | windBearing |  |  |
|  | windGust |  |  |
|  | windSpeed |  |  |

We store every meteorological condition as a separate datapoint on the aedifion.io platform to historicize its state. More on this in the subchapter [Datapoint and observation convention](integrations.md#datapoint-and-observation-convention).

### Prediction horizons

Besides the current state of a meteorological condition some use-cases require prediction data. We offer hourly predictions up to 168 hours \(7 days\) in the future and update them every hour. On ordering you can flexibly choose which horizons you need. The predicted states are aligned to the top of the prediction’s timestamp.

We combine every prediction horizon with the meteorological state monitored to create unique datapoints. You can use the unique datapoints to address the historicized meteorological conditions and their predications on the platform. More on this in the subchapter [Datapoint and observation convention](integrations.md#datapoint-and-observation-convention).

### Datapoint and observation convention

Like any other datapoint on the aedifion.io platform the weather datapoints are identifiable by an alphanumeric identifier which is unique for each project. The timeseries data for particular weather datapoints is stored as observations with a tuple of _value_ and _timestamp_.

 The naming convention for weather datapoints is: 

_aedifion\_weather-&lt;meteorological condition&gt;\_&lt;preiction horizon&gt;_

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

### Order weather data

If you are interested in this weather data service for your project, [contact](https://docs.aedifion.io/docs/contact) us and let us know the project you want this data to be included in, the location the weather data shall be localized for, e.g., the address or geographical coordinates of your aedifion monitored building, the meteorological conditions, and the prediction horizons \(if any\) you are interested in. We will setup the service for you – free of charge.

{% hint style="info" %}
An order email could look like this:

Hey aedifion,

I’m interested in the weather data for:

* Project: _aedifion office_
* At: _Kupferstraße 14, 52070 Aachen, Germany_
* Meteorological conditions: _temperature, dewpoint, cloudCoverage_
* Prediction horizons: _current, 1h, 3h, 12h, 24h_

Thanks!
{% endhint %}

