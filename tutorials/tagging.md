---
description: >-
  This article deals with creation, modification, assignment and deletion of
  annotations to datapoints.
---

# Tagging

## Overview

This article deals with creating annotations to datapoints \(tags\). We will cover creating tags, assigning them to datapoints, modifying tags and their association with a datapoint, and finally removing associations with a datapoint or removing tags completely. We will cover all relevant API endpoints by the example of annotating datapoints with the office location.

Tags are a way to annotate your datapoints. Aedifion's tags are labels consisting of a key-value pair. For example, if you have a set of datapoints located inside your office, you can tag them with the key-value pair `('location', 'Office A113')`. Subsequently, the tagged datapoints can be easily accessed by the API. Tags are strictly project-scoped, i.e, they are shared within a project and can be assigned to multiple datapoints of that project but are never shared across different projects.

Aedifion also provides automatically created tags, e.g., datapoint properties read from BACnet or machine learning results.

## Types of tags

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
* _AI:_ The tag has been assigned by an artificial intelligence \(AI\).
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

## API Examples

In this section, we provide concrete examples on how to use the aedifion [HTTP API](../developers/api-documentation.md) to create, inspect, modify and delete tags and tag assignments to datapoints. We explain all the necessary methods by the example of tagging all the datapoints within your office.

### Adding tags

The API allows to create tags as key-value pairs. Subsequently, you can assign this tag to a datapoint. We want to create the tag `('location', 'Office A113')` to some of our datapoints. To this end, we use the `POST /v2/project/{project_id}/tag` which takes the following parameters:



{% api-method method="post" host="https://api-dev.aedifion.io" path="/v2/project/{project\_id}/tag" %}
{% api-method-summary %}
Add a tag to a project
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project\_id" type="integer" required=true %}
The id of the project where to create the tag
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="tag" type="object" required=true %}
A dictionary with a _key_ and a _value_
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "operation": "create",
  "resource": {
    "id": 42,
    "key": "tag key",
    "value": "tag value"
  },
  "success": true
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% tabs %}
{% tab title="Python" %}
```python
import requests
project_id = 1
r = requests.post(f"https://api-dev.aedifion.io/v2/project/{project_id}/tag", 
         auth=("email", "password"),
         data={'tag': {'key': 'location', 'value': 'Office A113'}})
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

We get the following response from the API which states that the tag has been successfully created.

```javascript
{
  "operation": "create",
  "resource": {
    "id": 42,
    "key": "location",
    "value": "Office A113"
  },
  "success": true
}
```

{% hint style="info" %}
You can only create a tag with the **same** _key_ **and** _value pair once. But you can use the tag multiple times._
{% endhint %}

The now created tag is not assigned to any datapoint. Let us now assign this tag to an existing datapoint. For this we have to use the following method \[LINK\].

{% api-method method="post" host="https://api-dev.aedifion.io" path="/v2/datapoint/tag " %}
{% api-method-summary %}
Assign tag to datapoint
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="dataPointID" type="string" required=true %}
The alphanumeric dataPointID of the datapoint to assign the tag to
{% endapi-method-parameter %}

{% api-method-parameter name="project\_id" type="integer" required=true %}
The numeric ID of the project
{% endapi-method-parameter %}

{% api-method-parameter name="tag\_id" type="integer" required=true %}
The numeric ID of the tag, which to assign to the dataPoint
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "operation": "create",
  "resource": {
    "datapoint": {
      "dataPointID": "the_datapointID_with_the_new_tag",
      "hash_id": "ABCD1234",
      "id": 99,
      "project_id": 4
    },
    "tag": {
      "id": 42,
      "key": "tag key",
      "protected": false,
      "source": "user",
      "value": "tag value"
    }
  },
  "success": true
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% tabs %}
{% tab title="Python" %}
```python
import requests
project_id = 1
r = requests.post(f"https://api-dev.aedifion.io/v2/datapoint/tag", 
         auth=("email", "password"),
         params={
                  'dataPointID': 'datapoint_within_your_office',
                  'tag_id': 42,
                  'project_id': 1,
         })
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

We get the following response from the API which indicates a successful assignment of the tag to the datapoint. It also returns the **datapoint** and the **assigned tag** as additional resource.

```javascript
{
  "operation": "create",
  "resource": {
    "datapoint": {
      "dataPointID": "datapoint_within_your_office",
      "hash_id": "ABCD1234",
      "project_id": 4
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

Since we just created the tag we knew the required **tag\_id** to assign it to a datapoint. If you want to assign a previously created tag but you do not know the tag\_id, you can get an overview of all possible key-value pairs in combination with their IDs \(tag\_id\) by using the following method of Aedifion's API.

{% api-method method="get" host="https://api-dev.aedifion.io" path="/v2/project/{project\_id}/tags " %}
{% api-method-summary %}
List all tags
{% endapi-method-summary %}

{% api-method-description %}
This methods lists all tags of a project.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project\_id" type="integer" required=true %}
The numeric ID of a project
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-query-parameters %}
{% api-method-parameter name="key" type="string" required=false %}
A string used to filter the tag key
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
[
  {
    "id": 42,
    "key": "first tag",
    "value": "first value"
  },
  {
    "id": 43,
    "key": "second tag",
    "value": "second value"
  }
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% tabs %}
{% tab title="Python" %}
```python
import requests
project_id = 1

r = requests.get(f"https://api-dev.aedifion.io/v2/project/{project_id}/tags", 
         auth=("email", "password"),
         params={'key': 'location')
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="Second Tab" %}

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

### 

### Modifying tags

With the Aedifion API you can either modify the **tag** directly \(i.e. the _key_ or the _value_\) or the **tag assignment** \(i.e. the confirmed status\). The first method affects all assignments the user has made. This method is conceived e.g. for fixing typos in the tags. The second method is to verify the tag assignment.

For example if your company decides to rename the offices \(e.g. from A113 to B113\), you do not need to recreate all the tags; you can just modify them. To modify the _key_ and/or the _value_ of the tag use the following API method \[LINK\].

{% api-method method="put" host="https://api-dev.aedifion.io" path="/v2/project/{project\_id}/tag/{tag\_id}" %}
{% api-method-summary %}
Modify tag
{% endapi-method-summary %}

{% api-method-description %}
This method modifies a tag within a project. With this you can change the key and/or value assigned to multiple datapoints.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project\_id" type="integer" required=true %}
The numeric ID of the project
{% endapi-method-parameter %}

{% api-method-parameter name="tag\_id" type="integer" required=true %}
The numeric ID of the tag
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="tag" type="object" required=true %}
A dictionary with key and/or value.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "operation": "update",
  "resource": {
    "id": 42,
    "key": "new tag key",
    "value": "new tag value"
  },
  "success": true
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=201 %}
{% api-method-response-example-description %}
A new tag is generated, since the old tag could not be changed directly \(e.g. because bacnet has assigned the tag to new datapoints\). The relevant associations are updated accordingly.
{% endapi-method-response-example-description %}

```javascript
{
  "operation": "update",
  "resource": {
    "id": 43,
    "key": "new tag key",
    "value": "new tag value"
  },
  "success": true
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% hint style="warning" %}
If there are tags assigned by another source or protected tag assignments, a **new tag** \(with a new **tag\_id**\) is created and a 201 HTTP response is returned.
{% endhint %}

{% tabs %}
{% tab title="Python" %}
```python
import requests
project_id = 1
tag_id = 42
r = requests.put(f"https://api-dev.aedifion.io/v2/project/{project_id}/tag/{tag_id}", 
         auth=("email", "password"),
         data={"tag": {"value": "B113"}})
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

We get the following response which indicates a successful change of the tag including the properties of the changed tag.

```javascript
{
  "operation": "update",
  "resource": {
    "id": 42,
    "key": "location",
    "value": "B113"
  },
  "success": true
}
```



You can also modify the tag assignment to a datapoint \(here: confirm or reject the assignment\). This is especially useful for **automatically generated tags** \(e.g. from bacnet or from machine learning\). Modifying these will help find faulty datapoints and improve our machine learning systems.

{% api-method method="put" host="https://api-dev.aedifion.io" path="/v2/datapoint/tag/{tag\_id} " %}
{% api-method-summary %}
Modify tag assignment
{% endapi-method-summary %}

{% api-method-description %}
With this method you can modify the confirmed status of an assigned tag.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="" type="integer" required=true %}
The numeric tag id
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-query-parameters %}
{% api-method-parameter name="dataPointID" type="string" required=false %}
The alphanumeric dataPointID
{% endapi-method-parameter %}

{% api-method-parameter name="project\_id" type="integer" required=true %}
The numeric project ID
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="tag" type="object" required=true %}
A dictionary with the key confirmed.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "operation": "update",
  "resource": [
    {
      "confirmed": true,
      "id": 42,
      "key": "tag key",
      "protected": false,
      "source": "user",
      "value": "tag value"
    }
  ],
  "success": true
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% tabs %}
{% tab title="Python" %}
```python
import requests
project_id = 1
tag_id = 42
r = requests.put(f"https://api-dev.aedifion.io/v2/project/{project_id}/tag/{tag_id}", 
         auth=("email", "password"),
         params = {
                  "dataPointID": "datapoint_within_your_office",
                  "project_id": project_id,
         },
         data={'tag': {'confirmed': True}})
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

The response for this API method returns the modified tag associated with this datapoint. If a tag is assigned my **multiple sources** to this datapoint, **all** assignments are modified.

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

You can retrieve all datapoints corresponding to different tags. You **must** provide a tag **key**. Optional parameters are the tag **value**, the assigning **source**, or the **confirmed** status. The API call returns a list of all datapoints conforming with all those filters combined with the conforming tags.

This method is intended for fast retrieval of datapoints with tags assigned to it. For example one could get the list of all datapoints with a unit assigned to it \(key='unit'\) which are unconfirmed \(confirmed='unconfirmed'\). The returned tag assignments can now be easily confirmed or rejected to improve finding faulty datapoints or the machine learning system.

{% api-method method="get" host="https://api-dev.aedifion.io" path="/v2/project/{project\_id}/datapoints/byTag" %}
{% api-method-summary %}
Retrieve datapoints by tag
{% endapi-method-summary %}

{% api-method-description %}
This method retrieves all datapoints by different tag filters.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project\_id" type="integer" required=true %}
The numeric ID of the project
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-query-parameters %}
{% api-method-parameter name="key" type="string" required=true %}
The key of the tag
{% endapi-method-parameter %}

{% api-method-parameter name="value" type="string" required=false %}
The value of the tag
{% endapi-method-parameter %}

{% api-method-parameter name="source" type="string" required=false %}
The assigning source of the tag
{% endapi-method-parameter %}

{% api-method-parameter name="confirmed" type="string" required=false %}
One of 'true', 'false', or 'unconfirmed'
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
[
  {
    "dataPointID": "datapointID_with_tag",
    "tags": [
      {
        "id": 42,
        "key": "tag key",
        "protected": false,
        "source": "user",
        "value": "tag value"
      }
    ]
  }
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% tabs %}
{% tab title="Python" %}
```python
import requests
project_id = 1
r = requests.get(f"https://api-dev.aedifion.io/v2/project/{project_id}/datapoints/byTag", 
         auth=("email", "password"),
         params={'key': 'location'})
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

The result returns a list of all datapoints where a location-tag is assigned to:

```javascript
[
  {
    "dataPointID": "datapoint_within_your_office",
    "tags": [
      {
        "confirmed": true,
        "id": 42,
        "key": "location",
        "protected": false,
        "source": "user",
        "value": "B113"
      }
    ]
  }
]
```

### Deleting tags and assignments

The Aedifion API allows you to delete tag assignments \(remove tag from a datapoint\) or to delete a tag entirely. You cannot delete a tag assignment which is **protected**. Also you cannot delete a tag from a project which has protected assignments or is assigned by another source as _user_ \(e.g. _bacnet_\).

{% hint style="danger" %}
Deleting a tag or a tag assignment cannot be undone.
{% endhint %}

{% api-method method="delete" host="https://api-dev.aedifion.io" path="/v2/datapoint/tag/{tag\_id} " %}
{% api-method-summary %}
Delete tag assignments
{% endapi-method-summary %}

{% api-method-description %}
Calling this method removes an assignment of a tag from a datapoint.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="tag\_id" type="integer" required=true %}
The numeric ID of the tag
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-query-parameters %}
{% api-method-parameter name="dataPointID" type="string" required=true %}
The alphanumeric dataPointID
{% endapi-method-parameter %}

{% api-method-parameter name="project\_id" type="integer" required=true %}
The numeric ID of the project
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "operation": "delete",
  "resource": {
    "datapoint": {
      "dataPointID": "dataPointID_to_remove_from",
      "hash_id": "ABCD1234",
      "id": 1,
      "project_id": 1
    },
    "tag": {
      "id": 42,
      "key": "tag key",
      "value": "tag value"
    }
  },
  "success": true
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

If a tag has been erroneously assigned to a datapoint, you can remove this assignment with the following API method \[LINK\].

{% tabs %}
{% tab title="Python" %}
```python
import requests
project_id = 1
tag_id = 42
r = requests.delete(f"https://api-dev.aedifion.io/v2/datapoint/tag/{tag_id}", 
         auth=("email", "password"),
         params = {
                  'dataPointID': 'wrong_dataPointID',
                  'project_id': project_id,
         })
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

The result returns the tag and the datapoint from which the assignment was deleted.

```javascript
{
  "operation": "delete",
  "resource": {
    "datapoint": {
      "dataPointID": "wrong_dataPointID",
      "hash_id": "WXYZ9876",
      "id": 77,
      "project_id": 1
    },
    "tag": {
      "id": 42,
      "key": "location",
      "value": "B113"
    }
  },
  "success": true
}
```

If you want to delete the tag completely including **all its assignments** then you can use the following API method \[LINK\].

{% hint style="warning" %}
You cannot delete a tag if it is assigned by another source \(e.g. Bacnet\) or has protected assignments.
{% endhint %}

{% api-method method="delete" host="https://api-dev.aedifion.io" path="/v2/project/{project\_id}/tag/{tag\_id} " %}
{% api-method-summary %}
Delete tag
{% endapi-method-summary %}

{% api-method-description %}
Deletes a tag from the project
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project\_id" type="integer" required=false %}
The numeric ID of the project
{% endapi-method-parameter %}

{% api-method-parameter name="tag\_id" type="integer" required=false %}
The numeric ID of the tag
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "operation": "delete",
  "resource": {
    "id": 42,
    "key": "tag key",
    "value": "tag value"
  },
  "success": true
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% tabs %}
{% tab title="Python" %}
```python
import requests
project_id = 1
tag_id = 42
r = requests.delete(f"https://api-dev.aedifion.io/v2/project/{project_id}/tag/{tag_id}", 
         auth=("email", "password"))
print(r.status_code, r.json())
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

When the operation is successful the deleted tag is returned. **This operation cannot be undone**!

```javascript
{
  "operation": "delete",
  "resource": {
    "id": 42,
    "key": "location",
    "value": "B113"
  },
  "success": true
}
```

