---
description: Guide on accessing analytics functionality through the HTTP API.
---

# Analytics

## Overview

### Preliminaries

The examples provided in this article partly build on each other. For the sake of brevity, boiler plate code such as imports or variable definitions is only shown once and left out in subsequent examples.

To execute the examples provided in this tutorial, the following is needed:

* A valid login \(username and password\) to the aedifion.io platform. If you do not have a login yet, please [contact us](../../../contact.md) regarding a demo login. The login used in the example will not work!
* A project with mapped components on which analyses have been configured and successfully computed.
* Optionally, a working installation of [Python](https://www.python.org/) or [Curl](https://curl.haxx.se/).

### Basic concepts

We briefly revise the basic terminology used in this tutorial. Make sure to check our [Glossary](../../../glossary.md) when something seems strange to you.

* **Abstract components:** aedifion provides a set of curated components that abstractly model the \(expected\) functioning of various physical or virtual technical building equipment such as boilers, heat pumps, control loops, rooms, etc. The API refers to these abstract components simply as _component_ \(see Component-tagged API resources\).
* **Pins:** Each component has a set of pins that are placeholders for a real component's datapoints. E.g., the heat pump component has pins for its supply and return temperature.
* **Mapped components:** The process of attaching real datapoints collected from a real building, factory, district, etc. to a component is referred to as _mapping_. Mapping makes from an abstract component a component instance which is bound to the project that holds the mapped datapoint. On the API, we refer to a mapped component as a _componentInProject_ \(see Project-tagged API resources\)_._
* **Analysis functions:** aedifion provides a suite of algorithms to analyze building performance. The API calls these _analysis functions_. Each analysis function is applicable to one or more components. E.g., an analysis of the temperature spread might apply only to the _heat pump_ component, while an analysis of duty cycles might apply to any physical, active component.
* **Analysis instance:** To run an analysis on real data, a configuration of which analysis functions are applied to which mapped component is required first. By this configuration, an analysis function is instantiated \(similar to a component that is mapped\) and becomes and _analysis instance._ The configured instance can subsequently be executed on arbitrary time ranges, e.g., days, weeks, months, or whole years. 
* **Analysis result:** Each execution of an instance on a time range provides a single result object that can be queried using its unique reference.

## Abstract components

Let's first explore the available abstract component models. We provide information about [the details of these models in the engineering part of this documentation](../../../engineers/specifications/component-data-models.md). In this article, we focus on developers' needs and use the HTTP API's `GET /v2/components` endpoint to query the most basic information about them.

{% tabs %}
{% tab title="Python" %}
```python
import requests
auth = ("john.doe@aedifion.com", "s3cr3tp4ssw0rd")
r = requests.get("https://api.aedifion.io/v2/components", auth=john)
print(r.text)
```
{% endtab %}

{% tab title="Curl" %}
```bash
curl -X GET
    -u john.doe@aedifion.com:s3cr3tp4ssw0rd
    https://api.aedifion.io/v2/components
```
{% endtab %}
{% endtabs %}

The response is a JSON-formatted list of the available components. Each component as a unique numeric id, an english and german name, and an optional free-text description. 

```javascript
[
  {
    "description": "",
    "id": 179,
    "nameDE": "Niedertemperaturkessel",
    "nameEN": "low-temperature_boiler"
  },
  {
    "description": "",
    "id": 180,
    "nameDE": "Brennwertkessel",
    "nameEN": "condensing_boiler"
  },
  {
    "description": "",
    "id": 181,
    "nameDE": "Ventilator",
    "nameEN": "fan"
  },
  {
    "description": "",
    "id": 182,
    "nameDE": "thermischer Regelkreis",
    "nameEN": "thermal_control_loop"
  },
  
  ...
  
  {
    "description": "",
    "id": 132,
    "nameDE": "Wärmepumpe",
    "nameEN": "heat_pump"
  },
  {
    "description": "",
    "id": 134,
    "nameDE": "Kraftwärmekopplung",
    "nameEN": "combined_heat_and_power"
  }
]
```

### Component pins and attributes

Components have _pins_ and _attributes._ On _abstract components_ these are placeholders which are filled with real datapoints \(pins\) and meta information for the analysis \(attributes\) in [_mapped components_](analytics.md#mapped-components)_._ To view pins or attribute placeholders for abstract components, the `GET /v2/component/{component_id}/pins` and `GET /v2/component/{component_id}/attributeDefinitions` endpoints are used.

Let's have a closer look at the _thermal control loop_ component with id = 182.

{% tabs %}
{% tab title="Python" %}
```python
import requests
auth = ("john.doe@aedifion.com", "s3cr3tp4ssw0rd")
r = requests.get("https://api.aedifion.io/v2/components/182/pins", auth=john)
print(r.text)

r = requests.get("https://api.aedifion.io/v2/components/182/attributeDefinitions",
                 auth=john)
print(r.text)
```
{% endtab %}

{% tab title="Curl" %}
```bash
curl -X GET
    -u john.doe@aedifion.com:s3cr3tp4ssw0rd
    https://api.aedifion.io/v2/components/182/pins
    
curl -X GET
    -u john.doe@aedifion.com:s3cr3tp4ssw0rd
    https://api.aedifion.io/v2/components/182/attributeDefinitions
```
{% endtab %}
{% endtabs %}

The _thermal control loop_ component has thirteen pin placeholders. Note that not all [analysis functions](analytics.md#analysis-functions) require all pins to be filled and some group of pins may make another group of pins redundant.

```javascript
[
  {
    "attributes": [],
    "description": "",
    "id": 23,
    "location": "",
    "nameDE": "Wärmestrom",
    "nameEN": "heat flow"
  },
  {
    "attributes": [],
    "description": "",
    "id": 22,
    "location": "",
    "nameDE": "Wärmemengezähler",
    "nameEN": "heat meter"
  },
  {
    "attributes": [],
    "description": "",
    "id": 18,
    "location": "",
    "nameDE": "Eintrittstemperatur",
    "nameEN": "inlet temperature"
  },
  {
    "attributes": [],
    "description": "",
    "id": 24,
    "location": "",
    "nameDE": "Betriebsmeldung",
    "nameEN": "operating message"
  },
  {
    "attributes": [],
    "description": "",
    "id": 20,
    "location": "",
    "nameDE": "Austrittstemperatur",
    "nameEN": "outlet temperature"
  },
  {
    "attributes": [],
    "description": "",
    "id": 25,
    "location": "",
    "nameDE": "Versorgungstemperatursollwert",
    "nameEN": "outlet temperature setpoint"
  },
  {
    "attributes": [],
    "description": "",
    "id": 471,
    "location": "",
    "nameDE": "Außenlufttemperatur",
    "nameEN": "outside air temperature"
  },
  {
    "attributes": [],
    "description": "",
    "id": 26,
    "location": "",
    "nameDE": "Pumpenbetriebsmeldung",
    "nameEN": "pump operating message"
  },
  {
    "attributes": [],
    "description": "",
    "id": 30,
    "location": "",
    "nameDE": "Pumpdrehzahl",
    "nameEN": "pump revolution speed"
  },
  {
    "attributes": [],
    "description": "",
    "id": 19,
    "location": "",
    "nameDE": "Rücklauftemperatur",
    "nameEN": "return temperature"
  },
  {
    "attributes": [],
    "description": "",
    "id": 28,
    "location": "",
    "nameDE": "Ventilposition",
    "nameEN": "valve position"
  },
  {
    "attributes": [],
    "description": "",
    "id": 29,
    "location": "",
    "nameDE": "Ventilpositionssollwert",
    "nameEN": "valve position setpoint"
  },
  {
    "attributes": [],
    "description": "",
    "id": 21,
    "location": "",
    "nameDE": "Volumenstrom",
    "nameEN": "volume flow"
  }
]
```

In addition to the pins, the component defines twelve placeholders for additional attributes. While pins are used to attach real time series data to a component, attributes are used to hold additional meta-data such as regional holidays, timezones, schedules etc.

```javascript
[
  {
    "id": 40,
    "key": "cooling_activation_limit",
    "nameDE": "Kühlaktivierungsgrenze",
    "nameEN": "Cooling activation limit",
    "value_type": "float"
  },
  {
    "id": 41,
    "key": "heating_activation_limit",
    "nameDE": "Heizaktivierungsgrenze",
    "nameEN": "Heating activation limit",
    "value_type": "float"
  },
  {
    "id": 42,
    "key": "lower_deviation_tolerance",
    "nameDE": "Untere Abweichtoleranz",
    "nameEN": "Lower deviation tolerance",
    "value_type": "float"
  },
  {
    "id": 43,
    "key": "reference_curve",
    "nameDE": "Referenzkurve",
    "nameEN": "Reference curve",
    "value_type": "json"
  },
  {
    "id": 44,
    "key": "upper_deviation_tolerance",
    "nameDE": "Obere Abweichtoleranz",
    "nameEN": "Upper deviation tolerance",
    "value_type": "float"
  },
  {
    "id": 54,
    "key": "schedule",
    "nameDE": "Fahrplan",
    "nameEN": "Schedule",
    "value_type": "json"
  },
  {
    "id": 55,
    "key": "regional_holiday_key",
    "nameDE": "Schlüssel regionaler Feitertage",
    "nameEN": "Regional holiday key",
    "value_type": "string"
  },
  {
    "id": 56,
    "key": "schedule_timezone",
    "nameDE": "Fahrplanzeitzone",
    "nameEN": "Schedule timezone",
    "value_type": "string"
  },
  {
    "id": 57,
    "key": "custom_day_schedules",
    "nameDE": "Benutzerdefinierte Tagespläne",
    "nameEN": "Custom day schedules",
    "value_type": "json"
  },
  {
    "id": 58,
    "key": "custom_holiday",
    "nameDE": "Benutzerdefinierte Ferien",
    "nameEN": "Custom holiday",
    "value_type": "json"
  },
  {
    "id": 59,
    "key": "pre_conditioning_periode",
    "nameDE": "Vorkonditionierungszeit",
    "nameEN": "Pre conditioning periode",
    "value_type": "float"
  },
  {
    "id": 60,
    "key": "shutdown_flexibility",
    "nameDE": "Abschaltflexibilität",
    "nameEN": "Shutdown flexibility",
    "value_type": "float"
  }
]
```

## Mapped components

Filling the pin placeholders of an abstract component with datapoints from an actual project creates a _mapped component_ from an abstract one. While the initial mapping process is usually carried out by aedifion, the mapped components can be viewed, modified, and deleted ones added through the `GET/PUT/DELETE /v2/project/{project_id}/componentInProject/{componentinproject_id}` endpoints. 

{% hint style="info" %}
You may even create your own mapped components using the following endpoints

* Use`POST /v2/project/{project_id}/componentInProject` to instantiate an abstract component into your project.
* `POST/PUT/DELETE /v2/project/{project_id}/componentInProject/pin/datapoint` allows you to map datapoints to the pins of your new component instance.
* `POST/PUT/DELETE /v2/project/{project_id}/componentInProject/attribute`allows you to fill the attribute placeholders for your component with the desired info.

A detailed walk-through for these endpoints is omitted here for the sake of brevity.
{% endhint %}

Let's list the already available mapped components using the `GET /v2/project/{project_id}/componentsInProject` endpoint.

{% tabs %}
{% tab title="Python" %}
```python
import requests
auth = ("john.doe@aedifion.com", "s3cr3tp4ssw0rd")
r = requests.get("https://api.aedifion.io//v2/project/1/componentsInProject", 
                 auth=john)
print(r.text)
```
{% endtab %}
{% endtabs %}

The response contains a list of mapped components each with a unique id, a name and abbreviation, as well as the id of the abstract component from which it was instantiated.

### Mapped pins and attributes

```javascript
[
  {
    "abbreviation": "CTL",
    "component_id": 182,
    "id": 178,
    "nameEN": "Thermal Cont Loop",
    "project_id": 1
  },
  {
    "abbreviation": "WS",
    "component_id": 183,
    "id": 179,
    "nameEN": "Weather station",
    "project_id": 1
  },
  {
    "abbreviation": "Fan schedule",
    "component_id": 181,
    "id": 185,
    "nameEN": "Fan schedule",
    "project_id": 1
  },
  
  ...
  
]
```

Among others, we find mapped components for thermal control loops \(a virtual component that has no direct physical pendant\), a weather station \(corresponding to an actual physical weather station\), and a fan schedule \(a virtual component, again\).

Using id = 179 of the mapped _weather station_ component, we can query the details of the component using the `GET /v2/project/{project_id}/componentInProject/{componentinproject_id}` endpoint. The answer, among other information, contains the mapped datapoints and filled-in attributes. In this example, we have mapped only a single datapoint \(the ambient air temperature\) and filled in geo-position as attributes. Note that the answer model also contains the definition of the base component from which this mapped component was instantiated.

```javascript
{
  "abbreviation": "WS",
  "attributes": [
    {
      "id": 39,
      "key": "longitude",
      "value": "6.0516348"
    },
    {
      "id": 40,
      "key": "latitude",
      "value": "50.7893012"
    }
  ],
  "component": {
    "description": "",
    "id": 183,
    "nameDE": "Wetterstation",
    "nameEN": "weather_station"
  },
  "id": 179,
  "nameEN": "Weather station",
  "pins": [
    {
      "dataPointID": "bacnet58-4120-Ambient-air-temperature-T_Amb",
      "description": "",
      "id": 66,
      "location": "",
      "nameDE": "Temperatur",
      "nameEN": "temperature",
      "pin_attributes": []
    }
  ],
  "project_id": 1,
  "tags": []
}
```

## Analysis functions

We have explored the available components and seen how they filled with life by mapping them. Mapped components are useful for a lot of use cases beyond analytics, e.g., creating pre-defined monitoring dashboards. Here, however, we focus on how we can analyze performance of these components. 

We detail [all available analysis functions in the engineering part of this docs](../../../engineers/specifications/analytics.md) and just list the most basic information about them using the API endpoint `GET /v2/analytics/functions`. This endpoints requires a single query parameter `component` which should be the english name of the component for which you want to list available analyses. In the next example, we query analyses for the _thermal control loop_ component with id = 182. 

{% tabs %}
{% tab title="Python" %}
```python
import requests
auth = ("john.doe@aedifion.com", "s3cr3tp4ssw0rd")
r = requests.get("https://api.aedifion.io//v2/analytics/functions",
                 params={'component': 'thermal_control_loop'} 
                 auth=john)
print(r.text)
```
{% endtab %}
{% endtabs %}

The answer tells us that there is a total of six analysis functions ready to be run on a mapped _thermal control loop_ component. Each analysis function has a unique numeric id, a name and an optional free-text description.

```javascript
[
  {
    "description": "",
    "id": 307,
    "nameDE": "Reglerschwingungsanalyse",
    "nameEN": "control_loop_oscillation_analysis"
  },
  {
    "description": "",
    "id": 308,
    "nameDE": "Heizkurvenanalyse",
    "nameEN": "heating_curve_analysis"
  },
  {
    "id": 196,
    "nameDE": "Taktanalyse",
    "nameEN": "operating_cycle_analysis"
  },
  {
    "description": "",
    "id": 314,
    "nameDE": "Fahrplananalyse",
    "nameEN": "schedule_analysis"
  },
  {
    "description": "",
    "id": 315,
    "nameDE": "Sollwertabweichung",
    "nameEN": "setpoint_deviation_analysis"
  },
  {
    "description": "",
    "id": 318,
    "nameDE": "Temperaturspreizungsanalyse",
    "nameEN": "temperature_spread_analysis"
  }
]
```

## Analysis instances

In order to apply an analysis function to a mapped component, we need to create an _analysis instance_ using the `POST /v2/analytics/instance` endpoint. For the sake of brevity, we omit a full example here and proceed with examining an existing analysis instance with id = 318 for the above queried _thermal control loop_ component using the `GET /v2/analytics/instance/{instance_id}` endpoint. This endpoint requires the `project_id` as query parameter and the `instance_id` as path parameter. 

Note that you can also use the `GET /v2/analytics/instances`endpoint to query a list of _all_ analysis instance within a project.

{% tabs %}
{% tab title="Python" %}
```python
import requests
auth = ("john.doe@aedifion.com", "s3cr3tp4ssw0rd")
r = requests.get("https://api.aedifion.io//v2/analytics/instance/318",
                 params={'project_id': 1} 
                 auth=john)
print(r.text)
```
{% endtab %}
{% endtabs %}

The answer contains the configuration of the queried analysis instance. We can see that it references a single analysis function with id = 318 \(the equality of analysis instance id and analysis function id is pure coincidence\). This is the _temperature spread analysis_ [as we can tell from the previously listed analysis functions for the _thermal control loop_ component](analytics.md#analysis-functions).   

```javascript
{
  "config": {
    "analysisfunction_ids": [
      318
    ],
    "componentinproject_id": 182,
    "description": "temperature_spread_on_thermal_control_loop",
    "name": "ts_on_tcl"
  },
  "id": 318,
  "project_id": 1
}
```

## Analysis results

To run the analysis instace id = 318 that we have examined in the previous section, we would invoke the `POST /v2/analytics/instance/{instance_id}/run` endpoint and provide a time range using the `start` and `end` parameters for which we want to run the analysis. For the sake of brevity, we omit a full example here and continue with examining already computed results.

### Listing all results

First, using the `GET /v2/analytics/instance/{instance_id}/results` endpoints, we can list all available results for an analysis instance. We continue with analysis instance id = 318, and query all available results.

{% tabs %}
{% tab title="Python" %}
```python
import requests
auth = ("john.doe@aedifion.com", "s3cr3tp4ssw0rd")
r = requests.get("https://api.aedifion.io//v2/analytics/instance/318/results",
                 params={'project_id': 1} 
                 auth=john)
print(r.text)
```
{% endtab %}
{% endtabs %}

The response contains a list of results \(two in this example\). For each result, we get a unique reference, the time range over which the analysis was conducted, and a status indicating success or failure.

```javascript
[
  {
    "input_parameters": {
      "end": "2018-10-15 00:00:00+00:00",
      "start": "2018-10-10 00:00:00+00:00"
    },
    "result_id": "d927629a-a144-4f8b-b7c2-b0b256585371",
    "status": "Success."
  },
  {
    "input_parameters": {
      "end": "2019-10-15 00:00:00+00:00",
      "start": "2019-10-10 00:00:00+00:00"
    },
    "result_id": "dfd3a3e7-c17f-4ac9-9a8b-f93c2f56cee4",
    "status": "Success."
  }
]
```

### Getting result details

To get the details of a single result, we supply the unique result\_id to the `GET /v2/analytics/instance/{instance_id}/result/{result_id}` endpoint.

{% tabs %}
{% tab title="Python" %}
```python
import requests
auth = ("john.doe@aedifion.com", "s3cr3tp4ssw0rd")
r = requests.get("https://api.aedifion.io//v2/analytics/instance/318/result/"\
                 "dfd3a3e7-c17f-4ac9-9a8b-f93c2f56cee4",
                 params={'project_id': 1, 'result_language': 'en'} 
                 auth=john)
print(r.text)
```
{% endtab %}
{% endtabs %}

The response contains a lot of information, the most important parts being:

* the core _KPIs_ that were computed,
* an _interpretation_ of what these KPIs mean from an engineering perspective,
* a list of _recommendations_ that should be taken to improve operation of the analyzed component,
* a high level _signal color_ \(red, orange, green\) summarizing how _well_ the component operated w.r.t. the specified time range and analysis function.

```javascript
{
  "note": "This endpoint and returned data model are in state preview and due to change.",
  "results": {
    "analysisconfig_id": 318,
    "project_id": 1,
    "results": [
      {
        "end": "2019-10-15 00:00:00+00:00",
        "evaluations": [
          {
            "info": [],
            "interpretation": "The temperature spread between flow and return temperature during operating times is too small.",
            "recommendation": [
              "A small temperature spread can lead to low heat absorption by the consumers and results from a too high volume flow. A (temporary) shutdown should be considered."
            ],
            "signal_color": "red"
          }
        ],
        "function_name": "temperature_spread_analysis",
        "kpi": [
          {
            "name": "temperature spread.average",
            "unit": "°C",
            "value": -1.2
          },
          {
            "name": "temperature spread.minimum",
            "unit": "°C",
            "value": -8.48
          },
          {
            "name": "temperature spread.maximum",
            "unit": "°C",
            "value": 14
          }
        ],
        "start": "2019-10-10 00:00:00+00:00"
      }
    ]
  },
  "status": "Success."
}
```

