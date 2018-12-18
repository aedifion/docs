---
description: How to ingest and retrieve data.
---

# Data import and export

## Overview

In this article, we will go through the process of first ingesting timeseries data into the platform by hand using CSV upload and later retrieving that timeseries data from the platform using different search criteria.

### Preliminaries

The examples provided in this article partly build on each other. For the sake of brevity, boiler plate code such as imports or variable definitions is only shown once and left out in subsequent examples.

To execute the examples provided in this tutorial, the following is needed:

* A valid login \(username and password\) to the aedifion.io platform. If you do not have a login yet, please [contact us](../../contact.md) regarding a demo login. The login used in the example will not work!
* A project configured for access to timeseries data.
* Optionally, a working installation of [Python](https://www.python.org/) or [Curl](https://curl.haxx.se/).

## Importing data

There are two ways of ingesting data into the aedifion.io platform:

* via [CSV upload](data-import.md#csv-upload)
* via MQTT as treated in the [MQTT Tutorial](../mqtt.md) section

### CSV Upload

The `POST /v2/project/{project_id}/importTimeseries` endpoint allows uploading timeseries data in  [CSV format](https://en.wikipedia.org/wiki/Comma-separated_values). It accepts the following parameters:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:center">Datatype</th>
      <th style="text-align:center">Type</th>
      <th style="text-align:center">Required</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>project_id</b>
      </td>
      <td style="text-align:center">int</td>
      <td style="text-align:center">path</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The numeric id of the project which to upload data to.</td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>format</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">query</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The format of the uploaded data. Currently, only <code>csv</code> format
        is accepted.</td>
      <td style="text-align:left">csv</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>on_error</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">query</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">How to handle parsing errors, either<em> </em><code>abort</code><em> </em>or <code>continue</code>.</td>
      <td
      style="text-align:left">abort</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>import_<br />file</b>
      </td>
      <td style="text-align:center">file</td>
      <td style="text-align:center">formData</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The file containing the data to upload. The CSV dialect (delimiter, quote
        char, ...) is detected automatically.</td>
      <td style="text-align:left">
        <p>Datapoint1,20,2018-12-18</p>
        <p>Datapoint2,21,2018-12-19 9:00:00</p>
        <p>Datapoint3,39.5,2018-12-19 10:00:00+01</p>
      </td>
    </tr>
  </tbody>
</table>{% hint style="info" %}
**CSV Format**

The preferred CSV format for the uploaded data uses `,` as delimiter and `"` as quote character and has exactly the following three columns:

1. The datapoint identifier
2. The measurement value
3. The RFC3339-format timestamp of the measurement

E.g.,

```text
SimpleDPName,100,2018-12-18 8:00:00
"DP with spaces",102.2,2018-12-18 9:00:00
"DP with 'single' quotes, ""double"" qutoes, and delimiter",30,2018-12-18
...
```

Different CSV dialects \(column delimiters, quote chars, ...\) as well as datetime formats may be used. Differing formats are detected and processed automatically.
{% endhint %}

#### Example of correct file upload

As a first example, let's upload a correct file. We choose to abort on error, so that either the file is uploaded as a whole or nothing is uploaded at all. We're using the following test file which demonstrates different correct formats for datapoint identifiers and timestamps:

```text
"DP with blanks and delimiter ,",10,2018-12-17
DP with forward / slashes // in it,11.1,2018-12-17 1:00:00.000
"DP with single 'qutoes', double ""qutoes"", and the delimiter ','",12.3,2018-12-17T3:00:00+01
Emojimania 😄😁😅😂😌😍,100,2018-12-17 4:00:00+01:00
SimpleASCIIDatapoint,-1,2018-12-17 04:00:00Z
```

This test file is posted to the API endpoint together with the desired query parameters.

{% tabs %}
{% tab title="Python" %}
```python
from requests import post
api_url = "https://api.aedifion.io"
auth = ('john.doe@aedifion.com', 'mys3cr3tp4ssw0rd')
project_id = 1
query = {'format':'csv', 'on_error':'abort'}
filename = 'test_upload.csv'
r = post(f"{api_url}/v2/project/{project_id}/importTimeseries",
         auth=john,
         files={'import_file':open(fielname, 'rb')})
print(r.status_code, r.text)
```
{% endtab %}

{% tab title="Curl" %}
```bash
curl https://api.aedifion.io/v2/project/1/importTimeseries?format=csv&on_error=abort
    -X POST 
    -u john.doe@aedifion.com:mys3cr3tp4assw0rd
    --header 'Content-Type: multipart/form-data' 
    -F 'import_file=@test_upload_file.csv'   
```
{% endtab %}

{% tab title="Swagger UI" %}
1. Point your browser to [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/).
2. Click _Authorize_ on the upper right and provide your login.
3. From the main tags \(Meta, Company, ...\) select the _Project_ tag ,then the `POST /v2/project/{project_id}/importTimeseries` endpoint \(green\).
4. Choose `on_error = abort` and select a `test_upload_file.csv` from disk.
5. Click "_Try it out!_".
6. Inspect the response body and code.
{% endtab %}
{% endtabs %}

The JSON-formatted response confirms how many lines were parsed and uploaded successfully and how many lines could not be processed. Those lines that could not be parsed are bounced back in the _error_ subfield of the _resource_ field.

```javascript
{
    "success":true,
    "operation": "create",
    "resource": {
        "total_lines_success": 5,
        "total_lines_error": 0,
        "errors":[],        
        "project_id": 1
    }
}
```

Go on and query the project's datapoints through `GET /v2/project/{project_id}` and verify that the uploaded datapoints are all there.

```javascript
[
  ...,
  "DP with blanks and delimiter ,",
  "DP with forward / slashes // in it",
  "DP with single 'qutoes', double \"qutoes\", and the delimiter ','",
  "Emojimania 😄😁😅😂😌😍",
  "SimpleASCIIDatapoint",
  ...
]
```

#### Example of incorrect file upload

As a second example, we now upload the following file that is full of errors:

```text
CorrectDatapoint,100,2018-12-18 4:00:00
"Datapoint with backslash \ in it",100,2018-12-18 4:00:00
IncorrectTimestamp,100,181218 40000
IncorrectValue,asdf,2018-12-18 4:00:00
UnescapedDelimiter,inDatapointName,100,2018-12-18 4:00:00
,100,2018-12-18 4:00:00
```

Using `on_error = abort`, the API will abort on the first encountered error, discarding any lines that have potentially been parsed successfully prior to the error:

```javascript
{
    "error": "Error in line 2: Datapoint identifiers must not contain backslashes.",
    "operation": "create",
    "success": false
}
```

Using `on_error = continue`, the API will continue on error and try to parse the other lines. Note that the response code will always be `200 - OK` but will include all lines that could not be parsed.

```javascript
{
    "operation": "create",
    "resource": {
        "total_lines_success": 1,
        "total_lines_error": 5,
        "errors": [
            {"line_no":2, "error":"Datapoint identifier contains backslash: 'Datapoint with backslash \\ in it'"},
            {"line_no":3, "error":"Could not parse third field as datetime: '181218 40000'"},
            {"line_no":4, "error":"Could not parse second field as float: 'asdf'"},
            {"line_no":5, "error":"Could not parse second field as float: 'inDatapointName'"},
            {"line_no":6, "error":"Datapoint identifier is empty"}
        ]   
       "project_id": 1,
    },
    "success":true
}
```

The response tells us that only one line was imported successfully \(the _CorrectDatapoint_\) while all other lines contained different kinds of errors. Again, note that the endpoint will always return `200 - OK` if `on_error = continue` is set, even if not a single line could be successfully parsed.

## Exporting data

There are two ways of retrieving data from the aedifion.io platform:

* via JSON export
* via MQTT as treated in the [MQTT Tutorial](../mqtt.md) section

### JSON export

The endpoint `GET /v2/datapoint/timeseries` allows querying the data of single [datapoints](../../glossary.md#datapoint) by start and end while optionally using down-sampling or limiting the number of returned [observations](../../glossary.md#observation).

| Parameter | Datatype | Type | Required | Description | Example |
| :--- | :---: | :---: | :---: | :--- | :--- |
| **project\_id** | int | query | yes | The numeric id of the project from which to query a datapoint. | 1 |
| **dataPointID** | string | query | yes | The alphanumeric identifier of the datapoint to query. | bacnet100-4120-CO2 |
| **start** | datetime | query | no | Return only observations _after_ this time. If _start_ is provided without _end_, the first _max_ elements after _start_ are returned. | 2018-12-18 00:00:00 |
| **end** | datetime | query | no | Return only observations _before_ this time. If _end_ is provided without _start_, the last _max_ elements before _end_ are returned. | 2018-12-18 23:59:00 |
| **max** | int | query | no | Maximum number of observations to return. This option is ignored when both _start_ and _end_ are provided. Setting `max = 0` returns all __available data points. | 0 |
| **samplerate** | string | query | no | The returned observations are sampled down to the specified interval. Allowed intervals are integers combined with durations, like seconds \(s\), minutes \(m\), hours \(h\), and days \(d\), e.g. `10s`or `2m`. |  |

Before we use that endpoint to shoot a couple of example queries for the data, let's insert some more useful data.

{% file src="../../.gitbook/assets/sun\_degrees\_over\_horizon.csv" caption="Sun\'s position over the horizon" %}

This dataset describes the degree of the sun over the horizon by calendar days. With day 0 equal to new year's, the data looks like this:

![](../../.gitbook/assets/sun-degrees.png)

#### Querying by time window

Let's assume we only want to know the data for one year, e.g., 2018.

{% tabs %}
{% tab title="Python" %}

{% endtab %}

{% tab title="Curl" %}
```bash
curl 'http://localhost:5001/v2/datapoint/timeseries?project_id=1&dataPointID=SunDegreeOverHorizon&start=2018-01-01&end=2018-12-31'
    -u john.doe@aedifion.com:mys3cr3tp4ssw0rd
```
{% endtab %}
{% endtabs %}

