---
description: >-
  This article deals with managing favorites, renamings, and tags for
  datapoints.
---

# Favorites, tagging, renaming

## Overview

This article deals with creating favorites, renamings, and tags on datapoints as well as modifying and removing them. We start by adding, querying, and removing favorite datapoints. We then cover the concept of datapointkeys, showing how datapoints can be assigned alternate names \(renamings\). Finally, we cover creating tags, assigning them to datapoints, modifying them and their association with a datapoint, and finally removing associations with a datapoint or removing tags completely.

We provide concrete examples on how to use the aedifion [HTTP API](../../developers/api-documentation.md) to create, inspect, modify and delete favorites, renamings, tags and tag assignments to datapoints.

### Preliminaries

The examples provided in this article partly build on each other. For the sake of brevity, boiler plate code such as imports or variable definitions is only shown once and left out in subsequent examples.

To execute the examples provided in this tutorial, the following is needed:

* A valid login \(username and password\) to the aedifion.io platform. If you do not have a login yet, please [contact us](../../contact.md) regarding a demo login. The login used in the example will not work!
* A project with datapoints.
* Optionally, a working installation of [Python](https://www.python.org/) or [Curl](https://curl.haxx.se/).

## Favorites

Favorites are a way of flagging datapoints that you frequently view. They are, e.g., used in the [frontend](../../aedifion.io/frontend.md) to sort the datapoint selector list where they are marked by a star ⭐️. Favorites are scoped to the user, i.e., each user can have different favorites and is not able to see other users' favorites.

### Adding a favorite

You flag a datapoint as a favorite using the `POST /v2/datapoint/favorite` endpoint which takes the following parameters:

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
      <td style="text-align:center">integer</td>
      <td style="text-align:center">query</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The numeric id of the project to which the datapoint belongs.</td>
      <td
      style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>dataPointID</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">query</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The alphanumeric identifier of the datapoint to flag as favorite.</td>
      <td
      style="text-align:left">
        <p>datapoint_</p>
        <p>within_</p>
        <p>your_office</p>
        </td>
    </tr>
  </tbody>
</table>Note that all parameters are _query parameters_, i.e., they're added in [url-encoded form](https://de.wikipedia.org/wiki/URL-Encoding) to the query part of the requested URL \(the part following the domain and path separated by a question mark\).

{% tabs %}
{% tab title="Python" %}
```python
import requests
api_url = "https://api.aedifion.io"
john = ("john.doe@aedifion.com", "mys3cr3tp4ss0wrd")
query_params = {'project_id': 1, 'dataPointID': 'datapoint_within_your_office'}
r = requests.post(f"{api_url}/v2/datapoint/favorite", 
                  auth=john,
                  params=query_params)
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="Curl" %}
```bash
curl 'https://api.aedifion.io/v2/datapoint/favorite?project_id=1&dataPointID=datapoint_within_your_office'
    -X POST
    -u john.doe@aedifion.com:mys3cr3tp4ssw0rd
```
{% endtab %}

{% tab title="Swagger UI" %}
1. Point your browser to [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/).
2. Click _Authorize_ on the upper right and provide your login \(if you haven't already\).
3. From the main tags \(Meta, Company, ...\) select the _Datapoint_ tag ,then the `POST /v2/datapoint/favorite` endpoint \(green\).
4. Provide both the _project\_id_ and _dataPointID_ to identify the datapoint that you want to flag as a favorite.
5. Click "_Try it out!_".
6. Inspect the response body and code.
{% endtab %}
{% endtabs %}

Posting a favorite creates a relation between you \(your user account\) and a datapoint. On success, this relation \(user, datapoint\) is returned in the _resource_ field of the answer.

```javascript
{
  "operation": "create",
  "resource": {
    "datapoint": {
      "dataPointID": "datapoint_within_your_office",
      "hash_id": "Jw0EdgdG",
      "id": 121,
      "project_id": 1
    },
    "user": {
      "company_id": 1,
      "email": "john.doe@aedifion.com",
      "firstName": "John",
      "id": 1,
      "lastName": "Doe"
    }
  },
  "success": true
}
```

Let's shoot a few more requests and flag the following as favorites, too:

* CO2\_within\_my\_office
* TEMP\_within\_my\_office

You can only flag datapoints as favorites that exist in your project. If you pass a non-existing dataPointID, you will get the following error:

```javascript
{
    "error":"Datapoint does not exist in project: 'does_not_exist'",
    "operation":"create",
    "success":false
}
```

### Querying favorites

What use are favorites when you cannot quickly access them? That's what the GET /v2/project/{project\_id}/datapoints/favorites endpoint is there for. It takes the _project\_id_ as single parameter and returns all of the logged-in user's favorites.

| Parameter | Datatype | Type | Required | Description | Example |
| :--- | :---: | :---: | :---: | :--- | :--- |
| **project\_id** | integer | query | yes | The numeric id of the project from which to retrieve favorites. | 1 |

{% tabs %}
{% tab title="Python" %}
```python
r = requests.get(f"{api_url}/v2/project/{project_id}/datapoints/favorite", 
                 auth=john)
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="Curl" %}
```bash
curl 'https://api3.aedifion.io/v2/project/1/datapoints/favorites'
    -X GET
    -u john.doe@aedifion.com:mys3cr3tp4ssw0rd
```
{% endtab %}

{% tab title="Swagger UI" %}
1. Point your browser to [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/).
2. Click _Authorize_ on the upper right and provide your login \(if you haven't already\).
3. From the main tags \(Meta, Company, ...\) select the _Project_ tag ,then the `GET /v2/project/{project_id}/datapoints/favorites` endpoint \(blue\).
4. Provide the _project\_id_ of the project for which you want to retrieve favorites.
5. Click "_Try it out!_".
6. Inspect the response body and code.
{% endtab %}
{% endtabs %}

The response is a list of all your favorite datapoints in the specified project.

```javascript
[
  ...,
  "CO2_within_my_office",
  "datapoint_within_your_office",
  "TEMP_within_my_office",
  ...
]
```

### Deleting a favorite

Maybe that _datapoint\_within\_your\_office_ is not that important, after all. Removing it from the list of your favorites is a simple matter of calling `DELETE /v2/datapoint/favorite` with the usual parameters:

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
      <td style="text-align:center">integer</td>
      <td style="text-align:center">query</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The numeric id of the project to which the datapoint belongs.</td>
      <td
      style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>dataPointID</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">query</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The alphanumeric identifier of the datapoint to remove as favorite.</td>
      <td
      style="text-align:left">
        <p>datapoint_</p>
        <p>within_</p>
        <p>your_office</p>
        </td>
    </tr>
  </tbody>
</table>{% tabs %}
{% tab title="Python" %}
```python
r = requests.delete(f"{api_url}/v2/project/{project_id}/datapoints/favorite", 
                  auth=john)
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="Curl" %}
```bash
curl 'https://api.aedifion.io/v2/datapoint/favorite?project_id=1&dataPointID=datapoint_within_your_office'
    -X POST
    -u john.doe@aedifion.com:mys3cr3tp4ssw0rd
```
{% endtab %}

{% tab title="Swagger UI" %}
1. Point your browser to [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/).
2. Click _Authorize_ on the upper right and provide your login \(if you haven't already\).
3. From the main tags \(Meta, Company, ...\) select the _Datapoint_ tag ,then the `DELETE /v2/datapoint/favorite` endpoint \(red\).
4. Provide both the _project\_id_ and _dataPointID_ to identify the datapoint that you want to remove from your favorites.
5. Click "_Try it out!_".
6. Inspect the response body and code.
{% endtab %}
{% endtabs %}

The answer confirms the deletion of the favorite and returns the deleted \(user, datapoint\) relation similar to the creation of a favorite.

```javascript
{
  "operation": "delete",
  "resource": {
    "datapoint": {
      "dataPointID": "datapoint_within_your_office",
      "hash_id": "Jw0EdgdG",
      "id": 121,
      "project_id": 1
    },
    "user": {
      "company_id": 1,
      "email": "john.doe@aedifion.com",
      "firstName": "John",
      "id": 1,
      "lastName": "Doe"
    }
  },
  "success": true
}
```

{% hint style="info" %}
Creating and deleting favorites is _idempotent_, i.e., calling `POST /v2/datapoint/favorite` multiple times has the same effect as calling it once \(same for `DELETE`\)
{% endhint %}

## Renaming



## Tags

Tags are a way to annotate your datapoints. Aedifion's tags are labels consisting of a key-value pair. For example, if you have a set of datapoints located inside your office, you can tag them with the key-value pair `('location', 'Office A113')`. Subsequently, the tagged datapoints can be easily accessed by the API. Tags are strictly project-scoped, i.e, they are shared within a project and can be assigned to multiple datapoints of that project but are never shared across different projects.

Aedifion also provides automatically created tags, e.g., datapoint properties read from BACnet or machine learning results.

We will cover all relevant API endpoints by the example of annotating datapoints with the office location.

### Types of tags

Tags are inherently only a key-value pair with an additional unique numeric id for easy access.  The following tag represents a location information.

```javascript
{
  "id": 42,
  "key": "location",
  "value": "Office A113"
}
```

A tag can be assigned to multiple datapoints. All tag assignments have a sources**.** Currently, sources are one of the following but may be extended in future: 

* _user:_ The tag has been assigned by a user. Any tag assigned through the API is designated a user generated tag assignment.
* _ai:_ The tag has been assigned by an artificial intelligence \(ai\).
* _bacnet:_ The tag assignment has been derived from BACnet meta data.

In the following, we will often use the terms tag and tag assignment synonymously since the difference rather lies in the implementation than in the concept.

{% tabs %}
{% tab title="User generated tag" %}
The following example shows a tag assigned to a datapoint by a user \(`"source"="user"`\). Besides the known properties _id, key,_ and _value_ additional properties are present. The property _confirmed_ is not shown since the assignment has neither been confirmed or rejected. The property _protected_ indicates, that this assigned tag can be changed.

```javascript
{
  "id": 42,
  "key": "location",
  "protected": false,
  "source": "user",
  "value": "Office A113"
}
```
{% endtab %}

{% tab title="BACnet tag" %}
The following example shows a tag that has been automatically created and assigned to a datapoint by a BACnet logger \(`"source"="bacnet"`\). Besides the known properties _id, key,_ and _value_ additional properties are present. The property _confirmed_ indicates that the assigned tag has been verified by a user. The property _protected_ notes, that this assigned tag cannot be changed \(since it is information read from BACnet\).

```javascript
{
  "confirmed": true,
  "id": 42,
  "key": "description",
  "protected": true,
  "source": "bacnet",
  "value": "Temperature sensor in Room 1.113"
 }
```
{% endtab %}

{% tab title="Classification tag" %}
The following example excerpt shows a tag assigned to a datapoint by aedifion's machine learning service \(`"source"="ai"`\). Besides the known properties _id_, _key_, and _value_ additional properties are present. The property _confirmed_ indicates that the assigned tag has been rejected by a user. The property _protected_ notes, that this assigned tag cannot be changed \(since it is provided by the artificial intelligence\). Furthermore, a _probability_ is provided from the machine learning system which indicates the confidence of the resulting class.

```javascript
{
  "confirmed": false,
  "id": 42,
  "key": "class",
  "probability": 0.9,
  "protected": true,
  "source": "ai",
  "value": "Temperature"
}
```
{% endtab %}
{% endtabs %}

### Adding tags

The API allows to create tags as key-value pairs. Subsequently, you can assign this tag to a datapoint. We want to create the tag `('location', 'Office A113')` to some of our datapoints. To this end, we use the `POST /v2/project/{project_id}/tag` which takes the following parameters:

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
      <td style="text-align:center">integer</td>
      <td style="text-align:center">path</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The numeric id of the project for which to create the tag.</td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>key</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">body (JSON)</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The key of the tag.</td>
      <td style="text-align:left">location</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>value</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">no</td>
      <td style="text-align:left">The value of the tag.</td>
      <td style="text-align:left">Office A113</td>
    </tr>
  </tbody>
</table>_project\_id_ is a path parameter, i.e., it is entered directly in the requests path while the parameters _key_ and _value_ must be encoded as a valid [JSON](https://www.json.org/) and sent in the body of the request.

{% tabs %}
{% tab title="Python" %}
```python
import requests
api_url = "https://api.aedifion.io"
john = ("john.doe@aedifion.com", "mys3cr3tp4ss0wrd")
project_id = 1
newtag = {'key': 'location', 'value': 'Office A113'}
r = requests.post(api_url + f"/v2/project/{project_id}/tag", 
                  auth=john,
                  json=newtag)
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="Curl" %}
1. Copy-paste the JSON into a file, e.g., named new_tag.json._

   {% code-tabs %}
   {% code-tabs-item title="newtag.json" %}
   ```javascript
   {
       "key": "location",
       "value": "Office A113"
   }
   ```
   {% endcode-tabs-item %}
   {% endcode-tabs %}

2. Open a commandline.
3. Execute the following command.

   ```bash
   curl https://api.aedifion.io/v2/project/1/tag
     -X POST
     -u john.doe@aedifion.com:mys3cr3tp4ss0wrd
     -d @newtag.json 
     -H "Content-Type: application/json" 
   ```
{% endtab %}

{% tab title="Swagger UI" %}
1. Point your browser to [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/).
2. Click _Authorize_ on the upper right and provide your login.
3. From the main tags \(Meta, Company, ...\) select the _Project_ tag ,then the `POST /v2/project/{project_id}/tag` endpoint \(green\).
4. Copy-paste the above JSON into the value of the _tag_ parameter and fill out the _project\_id_.
5. Click "_Try it out!_".
6. Inspect the response body and code.
{% endtab %}
{% endtabs %}

We get the following JSON-formatted response from the API which states that the tag has been successfully created as indicated by the return code: 201 \(Created\).

```javascript
{
  "success": true,
  "operation": "create",
  "resource": {
    "id": 42,
    "key": "location",
    "value": "Office A113"
  }
}
```

Go ahead and create the same tag, i.e., same key and value, a second time. Note how this will not produce an error but instead return the already existing tag and change return code to 200 \(OK\).

{% hint style="info" %}
You can only create a tag with the **same** _key_ **and** _value pair once. But you can use the tag multiple times._
{% endhint %}

For now, our newly created tag is not assigned to any datapoint. Let's now assign this tag to an existing datapoint. For this we have to use the _POST /v2/datapoint/tag_ endpoint which requires the following parameters:

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
      <td style="text-align:center">integer</td>
      <td style="text-align:center">query</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The numeric id of the project from which to assign a tag.</td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>dataPointID</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">query</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The alphanumeric id of the datapoint which to assign a tag to.</td>
      <td
      style="text-align:left">
        <p>datapoint_</p>
        <p>within_</p>
        <p>your_office</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>tag_id</b>
      </td>
      <td style="text-align:center">integer</td>
      <td style="text-align:center">query</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The numeric id of the tag to assign.</td>
      <td style="text-align:left">42</td>
    </tr>
  </tbody>
</table>Note that all parameters are _query parameters_, i.e., they're added in [url-encoded form](https://de.wikipedia.org/wiki/URL-Encoding) to the query part of the requested URL \(the part following the domain and path separated by a question mark\).

{% tabs %}
{% tab title="Python" %}
```python
import requests
project_id = 1
dataPointID = 'datapoint_within_your_office'
tag_id = 42
r = requests.post(api_url + "/v2/datapoint/tag", 
         auth=john,
         params={'project_id':project_id,
                  'dataPointID': dataPointID,
                  'tag_id':tag_id})
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="Curl" %}
1. Open a commandline.
2. Execute the following command.

   ```bash
   curl 'https://api.aedifion.io/v2/project/1/tag?project_id=1&\
     dataPointID=datapoint_within_your_office&tag_id=42'
     -X POST
     -u john.doe@aedifion.com:mys3cr3tp4ss0wrd
     -H "Content-Type: application/json"
   ```
{% endtab %}

{% tab title="Swagger UI" %}
1. Point your browser to [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/).
2. Click _Authorize_ on the upper right and provide your login.
3. From the main tags \(Meta, Company, ...\) select the _Datapoint_ tag ,then the `POST /v2/datapoint/tag` endpoint \(green\).
4. Fill out all the neccessary parameters.
5. Click "_Try it out!_".
6. Inspect the response body and code.
{% endtab %}
{% endtabs %}

We get the following response from the API which indicates a successful assignment of the tag to the datapoint. Since an assignment is a relation between two entities, it returns both entities, the datapoint and the assigned tag, in the resource field. Note that `"source": "user"` was added automatically.

```javascript
{
  "operation": "create",
  "resource": {
    "datapoint": {
      "dataPointID": "datapoint_within_your_office",
      "hash_id": "ABCD1234",
      "project_id": 1
    },
    "tag": {
      "id": 42,
      "key": "location",
      "protected": false,
      "source": "user",
      "value": "Office A113"
    }
  },
  "success": true
}
```

### Listing tags

Since we just created the tag we knew the required _tag\_id_ to assign it to a datapoint. If you want to assign a previously created tag but you do not know the tag\_id, you can get an overview of all possible key-value pairs in combination with their IDs \(tag\_id\) by using the `GET /v2/project/{project_id}/tags` endpoint with the following parameters:

| Parameter | Datatype | Type | Required | Description | Example |
| :--- | :---: | :---: | :---: | :--- | :--- |
| **project\_id** | integer | path | yes | The numeric id of the project from which to assign a tag. | 1 |
| **key** | string | query | no | Optional key to filter the returned tags by their key. | location |

For this method we need a GET request. Therefore, we use `requests.get`.

{% tabs %}
{% tab title="Python" %}
```python
import requests
project_id = 1

r = requests.get(apif"/v2/project/{project_id}/tags", 
         auth=("email", "password"),
         params={'key': 'location'})
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="Curl" %}
1. Open a commandline.
2. Execute the following command.

   ```bash
   curl 'https://api.aedifion.io/v2/project/1/tags?key=location'
     -X GET
     -u john.doe@aedifion.com:mys3cr3tp4ss0wrd
     -H "Content-Type: application/json"
   ```
{% endtab %}

{% tab title="Swagger UI" %}
1. Point your browser to [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/).
2. Click _Authorize_ on the upper right and provide your login.
3. From the main tags \(Meta, Company, ...\) select the _Project_ tag ,then the `GET /v2/datapoint/tag` endpoint \(blue\).
4. Fill out all the neccessary parameters.
5. Click "_Try it out!_".
6. Inspect the response body and code.
{% endtab %}
{% endtabs %}

Since we provided the optional key parameter only the tags with the key "location" are returned:

```javascript
[
  {
    "id": 42,
    "key": "location",
    "value": "Office A113"
  },
  {
    "id": 43,
    "key": "location",
    "value": "Office A114"
  }
]
```

### Modifying tags

With the Aedifion API you can either modify the **tag** directly \(i.e. the _key_ or the _value_\) or the **tag assignment** \(i.e. the confirmed status\). The first method affects **all assignments** the user has made. This method is conceived e.g. for fixing typos in the tags. The second method is to **verify** the tag assignment \(set the confirmed status\).

For example if your company decides to rename the offices \(e.g. from A113 to B113\), you do not need to recreate all the tags; you can just modify them. To modify the _key_ and/or the _value_ of the tag use the  API endpoint `PUT /v2/project/{project_id}/tag/{tag_id}`. It requires the following parameters:

| Parameter | Datatype | Type | Required | Description | Example |
| :--- | :---: | :---: | :---: | :--- | :--- |
| **project\_id** | integer | path | yes | The numeric id of the project from which to assign a tag. | 1 |
| **tag\_id** | integer | path | yes | The numeric id of the tag. | 42 |
| **key** | string | body \(JSON\) | no | The new key of the tag. | Location |
| **value** | string | body \(JSON\) | no | The new value of the tag. | Office B113 |

Let us now rename the created tag to the new Office name. We have to provide the project\_id, tag\_id, and the new value.

{% tabs %}
{% tab title="Python" %}
```python
import requests
project_id = 1
tag_id = 42
r = requests.put(api_url + f"/v2/project/{project_id}/tag/{tag_id}",
         auth=john,
         json={"value": "Office B113"})
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="Curl" %}
1. Copy-paste the JSON into a file, e.g., named update_tag.json._

   {% code-tabs %}
   {% code-tabs-item title="updatetag.json" %}
   ```javascript
   {
       "key": "location",
       "value": "Office B113"
   }
   ```
   {% endcode-tabs-item %}
   {% endcode-tabs %}

2. Open a commandline.
3. Execute the following command.

   ```bash
   curl https://api.aedifion.io/v2/project/1/tag/42
     -X PUT
     -u john.doe@aedifion.com:mys3cr3tp4ss0wrd
     -d @updatetag.json 
     -H "Content-Type: application/json" 
   ```
{% endtab %}

{% tab title="Swagger UI" %}
1. Point your browser to [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/).
2. Click _Authorize_ on the upper right and provide your login.
3. From the main tags \(Meta, Company, ...\) select the _Project_ tag ,then the `PUT /v2/project/{project_id}/tag/{tag_id}` endpoint \(yellow\).
4. Copy-paste the above JSON into the value of the _tag_ parameter and fill out the the neccessary parameters.
5. Click "_Try it out!_".
6. Inspect the response body and code.
{% endtab %}
{% endtabs %}

As a response we either get a 200 or 201 status code. This depends on whether the API edited the tag in-place or had to create a new tag. The latter case might have happened if for example the BACnet logger has assigned the same tag to another datapoint. In this case all user-made assignments are updated to use the new tag\_id.

{% hint style="warning" %}
If there are tags assigned by another source or protected tag assignments, a **new tag** \(with a new **tag\_id**\) is created and a 201 HTTP response is returned.
{% endhint %}

{% tabs %}
{% tab title="200 \(in place\)" %}
```javascript
{
  "operation": "update",
  "resource": {
    "id": 42,
    "key": "location",
    "value": "Office B113"
  },
  "success": true
}
```
{% endtab %}

{% tab title="201 \(new tag\)" %}
```javascript
{
  "operation": "update",
  "resource": {
    "id": 43,
    "key": "location",
    "value": "Office B113"
  },
  "success": true
}
```
{% endtab %}
{% endtabs %}

### Modifying tag assignments

You can also modify the tag assignment to a datapoint \(here: confirm or reject the assignment\). This is especially useful for **automatically generated tags** \(e.g. from BACnet or from machine learning\). Modifying these will help find faulty datapoints and improve our machine learning systems. For this we use the endpoint `put /v2/datapoint/tag/{tag_id}` with the following parameters:

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
      <td style="text-align:center">integer</td>
      <td style="text-align:center">query</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The numeric id of the project from which to assign a tag.</td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>tag_id</b>
      </td>
      <td style="text-align:center">integer</td>
      <td style="text-align:center">path</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The numeric id of the tag.</td>
      <td style="text-align:left">42</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>dataPointID</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">query</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The alphanumeric id of the datapoint which to assign a tag to.</td>
      <td
      style="text-align:left">
        <p>datapoint_</p>
        <p>within_</p>
        <p>your_office</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>confirmed</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">body (JSON)</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The confirmed status of the assignment. Either "true" (correct assignment),
        "false" (incorrect assignment) or "unconfirmed" (unknown assignment).</td>
      <td
      style="text-align:left">"true"</td>
    </tr>
  </tbody>
</table>With this method we can now confirm our created tag as an example.

{% tabs %}
{% tab title="Python" %}
```python
import requests
project_id = 1
tag_id = 42
r = requests.put(api_url + f"/v2/datapoint/tag/{tag_id}", 
         auth=john
         params = {
                  "dataPointID": "datapoint_within_your_office",
                  "project_id": project_id,
         },
         json={'confirmed': 'true'})
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="CURL" %}
1. Copy-paste the JSON into a file, e.g., named _confirmedtag.json._

   {% code-tabs %}
   {% code-tabs-item title="confirmedtag.json" %}
   ```javascript
   {
       "confirmed": "true"
   }
   ```
   {% endcode-tabs-item %}
   {% endcode-tabs %}

2. Open a commandline.
3. Execute the following command.

   ```bash
   curl 'https://api.aedifion.io/v2/datapoint/tag/42?
     dataPointID=datapoint_within_your_office&project_id=1'
     -X PUT
     -u john.doe@aedifion.com:mys3cr3tp4ss0wrd
     -d @confirmedtag.json 
     -H "Content-Type: application/json" 
   ```
{% endtab %}

{% tab title="Swagger UI" %}
1. Point your browser to [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/).
2. Click _Authorize_ on the upper right and provide your login.
3. From the main tags \(Meta, Company, ...\) select the _Datapoint_ tag ,then the `PUT /v2/project/{project_id}/tag/{tag_id}` endpoint \(yellow\).
4. Copy-paste the above JSON into the value of the _tag_ parameter and fill out the the neccessary parameters.
5. Click "_Try it out!_".
6. Inspect the response body and code.
{% endtab %}
{% endtabs %}

The response for this API method returns the modified tag associated with this datapoint. If a tag is assigned by **multiple sources** \(e.g. "user" and "bacnet"\) to this datapoint, **all** of these assignments are modified. Note, that the "confirmed" parameter is now set to `true`.

```javascript
{
  "operation": "update",
  "resource": [
    {
      "confirmed": true,
      "id": 42,
      "key": "location",
      "protected": false,
      "source": "user",
      "value": "B113"
    }
  ],
  "success": true
}
```

### Filtering datapoints by tag

You can retrieve all datapoints corresponding to different tags. You **must** provide a tag _key_. Optional parameters are the tag _value_, the assigning _source_, or the _confirmed_ status. The API call returns a list of all datapoints conforming with all those filters combined with the relevant tags.

This method is intended for fast retrieval of datapoints with _tags assigned_ to it. For example one could get the list of all datapoints with a unit assigned to it \(key='unit'\) which are unconfirmed \(confirmed='unconfirmed'\). The returned tag assignments can now be easily confirmed or rejected to improve finding faulty datapoints or the machine learning system.

The relevant API endpoint is `GET /v2/project/{project_id}/datapoints/byTag` and the parameters are as follows:

| Parameter | Datatype | Type | Required | Description | Example |
| :--- | :---: | :---: | :---: | :--- | :--- |
| **project\_id** | integer | query | yes | The numeric id of the project from which to assign a tag. | 1 |
| **key** | string | query | yes | The key of the tags we filter for. | location |
| **value** | string | query | no | The value of the tags we filter for. | Office B113 |
| **source** | string | query | no | The source of the tag assignment we filter for. | user |
| **confirmed** | string | query | no | The confirmed status of the assignment. Either "true" \(correct assignment\), "false" \(incorrect assignment\) or "unconfirmed" \(unknown assignment\). | "true" |

Let us now query all datapoints we assigned a location tag to which are confirmed. Since we just confirmed the assignment before, we should get returned the just assigned tag to the datapoint.

{% tabs %}
{% tab title="Python" %}
```python
import requests
project_id = 1
r = requests.get(api_url + f"/v2/project/{project_id}/datapoints/byTag", 
         auth=john,
         params={
                  'key': 'location',
                  'confirmed': 'unconfirmed',
         })
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="CURL" %}
1. Open a commandline.
2. Execute the following command.

   ```bash
   curl 'https://api.aedifion.io/v2/project/1/datapoints/byTag?key=location'
     -X GET
     -u john.doe@aedifion.com:mys3cr3tp4ss0wrd
     -H "Content-Type: application/json"
   ```
{% endtab %}

{% tab title="Swagger UI" %}
1. Point your browser to [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/).
2. Click _Authorize_ on the upper right and provide your login.
3. From the main tags \(Meta, Company, ...\) select the _Datapoint_ tag ,then the `GET /v2/project/{project_id}/datapoints/byTag` endpoint \(blue\).
4. Fill out the the neccessary parameters.
5. Click "_Try it out!_".
6. Inspect the response body and code.
{% endtab %}
{% endtabs %}

We get the following list as result:

```javascript
[
  {
    "dataPointID": "datapoint_within_your_office",
    "tags": [
      {
        "confirmed": true
        "id": 42,
        "key": "location",
        "protected": false,
        "source": "user",
        "value": "Office B113"
      }
    ]
  }
]
```

### Deleting tag assignments

The Aedifion API allows you to delete tag assignments \(remove tag from a datapoint\) or to delete a tag entirely. You cannot delete a tag assignment which is **protected**. Also you cannot delete a tag completely from a project which has protected assignments or is assigned by another source as _user_ \(e.g. BAC_net_\).

{% hint style="danger" %}
Deleting a tag or a tag assignment cannot be undone.
{% endhint %}

Let us now delete the assignment of the created location tag from the datapoint. For this we need the API endpoint `DELETE /v2/datapoint/tag/{tag_id}`. The parameters are as follows:

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
      <td style="text-align:center">integer</td>
      <td style="text-align:center">query</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The numeric id of the project where the tag belongs to.</td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>tag_id</b>
      </td>
      <td style="text-align:center">integer</td>
      <td style="text-align:center">path</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The id of the tag we want to delete from the datapoint.</td>
      <td style="text-align:left">42</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>dataPointID</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">query</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The alphanumeric id of the datapoint from which we want to delete the
        tag.</td>
      <td style="text-align:left">
        <p>datapoint_</p>
        <p>within_</p>
        <p>your_office</p>
      </td>
    </tr>
  </tbody>
</table>{% tabs %}
{% tab title="Python" %}
```python
import requests
project_id = 1
tag_id = 42
r = requests.delete(api_url + f"/v2/datapoint/tag/{tag_id}", 
         auth=john,
         params = {
                  'dataPointID': 'datapoint_within_your_office',
                  'project_id': project_id,
         })
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="CURL" %}
1. Open a commandline.
2. Execute the following command.

   ```bash
   curl 'https://api.aedifion.io/v2/datapoint/tag/42?
     dataPointID=datapoint_within_your_office&project_id=1'
     -X DELETE
     -u john.doe@aedifion.com:mys3cr3tp4ss0wrd
     -H "Content-Type: application/json"
   ```
{% endtab %}

{% tab title="Swagger UI" %}
1. Point your browser to [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/).
2. Click _Authorize_ on the upper right and provide your login.
3. From the main tags \(Meta, Company, ...\) select the _Datapoint_ tag ,then the `DELETE /v2/datapoint/tag/{tag_id}` endpoint \(red\).
4. Fill out the the neccessary parameters.
5. Click "_Try it out!_".
6. Inspect the response body and code.
{% endtab %}
{% endtabs %}

The result returns the tag and the datapoint from which the assignment was deleted.

```javascript
{
  "operation": "delete",
  "resource": {
    "datapoint": {
      "dataPointID": "datapoint_within_your_office",
      "project_id": 1
    },
    "tag": {
      "id": 42,
      "key": "location",
      "value": "Office B113"
    }
  },
  "success": true
}
```

The tag itself still exists in the system but it is no longer assigned to the datapoint.

### Deleting Tags

If you want to delete the tag completely including **all its assignments** then you can use the following API method `DELETE /v2/project/{project_id}/tag/{tag_id}`.

{% hint style="warning" %}
You cannot delete a tag if it is assigned by **another source** \(e.g. automatically assigned tags by BACnet\) or has protected assignments.
{% endhint %}

The parameters for the API endpoint are the following:

| Parameter | Datatype | Type | Required | Description | Example |
| :--- | :---: | :---: | :---: | :--- | :--- |
| **project\_id** | integer | path | yes | The numeric id of the project from which to delete a tag. | 1 |
| **tag\_id** | integer | path | yes | The id of the tag we want to delete. | 42 |

{% tabs %}
{% tab title="Python" %}
```python
import requests
project_id = 1
tag_id = 42
r = requests.delete(api_url + f"/v2/project/{project_id}/tag/{tag_id}", 
         auth=("email", "password"))
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="CURL" %}
1. Open a commandline.
2. Execute the following command.

   ```bash
   curl https://api.aedifion.io/v2/project/1/tag/42
     -X DELETE
     -u john.doe@aedifion.com:mys3cr3tp4ss0wrd
     -H "Content-Type: application/json"
   ```
{% endtab %}

{% tab title="Swagger UI" %}
1. Point your browser to [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/).
2. Click _Authorize_ on the upper right and provide your login.
3. From the main tags \(Meta, Company, ...\) select the _Project_ tag ,then the `DELETE /v2/project/{project_id}/tag/{tag_id}` endpoint \(red\).
4. Fill out the the neccessary parameters.
5. Click "_Try it out!_".
6. Inspect the response body and code.
{% endtab %}
{% endtabs %}

When the operation is successful the deleted tag is returned. **This operation cannot be undone**!

```javascript
{
  "operation": "delete",
  "resource": {
    "id": 42,
    "key": "location",
    "value": "Office B113"
  },
  "success": true
}
```



