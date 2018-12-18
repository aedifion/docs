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

* via CSV upload as treated in the [following section](data-import.md#csv-upload)
* via MQTT as treated in the [MQTT Tutorial](../mqtt.md) section

### CSV Upload

The `POST /v2/project/{project_id}/importTimeseries` endpoint allows uploading timeseries data in  CSV format. It accepts the following parameters:

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

The preferred format for the uploaded data is

```text
<DataPointID_1>,<value_1>,<datetime_1>
<DataPointID_2>,<value_2>,<datetime_2>
...
<DataPointID_n>,<value_n>,<datetime_n>
```

where `<datetime>` is in [RFC3339](https://www.ietf.org/rfc/rfc3339.txt)-format and `<value>` is parsable as a float.

Different CSV dialects \(column delimiters, quote chars, ...\) as well as datetime formats may be used. Differing formats are detected and processed automatically.
{% endhint %}

#### Example of correct file upload

As a first example, let's upload a correct file. We choose to abort on error, so that either the file is uploaded as a whole or nothing is uploaded at all.

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
curl https://api.aedifion.io/v2/project/100/importTimeseries?format=csv&on_error=abort
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
        "total_lines_success": 12,
        "total_lines_error": 0,
        "errors":[],        
        "project_id": 1
    }
}
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

### JSON Export

