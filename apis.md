---
description: Overview of the APIs of the aedifion.io cloud platform.
---

# APIs

{% hint style="danger" %}
Under construction
{% endhint %}

## HTTP API

### Design principles

#### Idempotency

#### JSON

All our HTTP API endpoints use [JSON](https://www.json.org/) to encode \(body\) parameters and responses. JSON is a de-facto standard on the web and can be written and parsed in virtually any programming language as well as with a wide range of text and code editors.

#### Consistent use of HTTP Methods

We make consistent use of HTTP methods:

* `GET` methods are used exclusively to query existing resources and will never trigger any modification of resources.
* `POST` methods are used exclusively to create new resources that previously did no exist, yet, such as new users, projects, or tags.
* `PUT` methods are used exclusively to modify existing resources and thus always require the `id` of the resource to modify.
* `DELETE` methods are used exclusively to delete existing resources and thus always require the `id` of the resource to delete.

#### **Meaningful error messages but privacy first**

\*\*\*\*

## Websocket API

## MQTT API



