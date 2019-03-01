---
description: Reference for the aedifion HTTP API.
---

# HTTP API

## Overview

The HTTP API allows accessing all services of the aedifion.io platform through a carefully [designed](./#design-principles) RESTful interface. It can be [accessed](./#accessing-the-http-api) using a wide range of programming languages, libraries, tools and clients.

## Design principles

The aedifion.io HTTP API is designed according to the following principles.

### JSON encoding

All our HTTP API endpoints use the [JSON](https://www.json.org/) format to encode content of request and response bodies. The JSON standard is widely used on the web and can be written and parsed in virtually any programming language as well as with a wide range of text and code editors.

### HTTP methods

We make consistent use of HTTP methods:

* `GET` methods are used exclusively to query existing resources and will never trigger any modification of resources. In many cases, you can query single resources by providing a unique identifier or a list of resources of the same type. On success, they directly return the queried resource\(s\).
* `POST` methods are used exclusively to create new resources that previously did no exist, such as new users, projects, or tags. They return a _Success_ object containing the created resource or an _Error_ object with a message that indicates why creating the resource failed.
* `PUT` methods are used exclusively to modify existing resources and thus always require a unique identifier of the resource to modify. They return a _Success_ object containing the modified resource or an _Error_ object with a message that indicates why modifying the reference resource failed.
* `DELETE` methods are used exclusively to delete existing resources and thus always require a unique identifier of the resource to delete. They return a _Success_ object containing the modified resource or an _Error_ object with a message that indicates why modifying the reference resource failed.

### HTTP status codes

The appropriate [HTTP status code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) is used where possible, e.g.,

* 200 for successful retrieval, modification, and deletion of resources
* 201 for successful creation of resources
* 400 for bad requests that the user can correct
* 401 for access to non-existant or unauthorized resources
* 403 for valid and authorized requests that are forbidden due to other reasons
* 404 for access to non-existant resources where this does not leak information
* 5xx for internal server errors that are not due to and/or fixable by the user

### **Meaningful error messages**

Upon error, an appropriate error message is returned that indicates what went wrong. The error message will be as informative as possible to help you fix the erroneous API call, but will deliberately spare out some details in order to protect privacy. E.g., error messages are formulated such that an unauthorized caller cannot distinguish a non-existant project from one that he/she does not have access to.

### Transactional changes

Any API call that touches resources through creation, modification, or deletion, will either succeed as a whole or fail without any modification done. If there occur any errors during a creation, modification, or deletion of an existing resource, all changes made up to that point are rolled back. Viewing an API call as an atomic transaction, the transaction is either made or not.

## Accessing the HTTP API

The API is accessed using HTTP which is one of the most widespread Internet standards today. Thus, you can use a wide range of tools and or programming languages to access it. The best choice depends on your use case and experience.

* [Command line tools](./#command-line-tools) are probably the quickest way to issue simple HTTP requests. Adequate tools are already built in all Unix-like systems and may be downloaded for Windows. Of course, these tools do not know anything about the form of request parameters and responses so you will have to do all parsing yourself which quickly becomes unhandy for more complex requests.
* [HTTP libraries](./#http-libraries) are available for all established programming languages. At different levels of abstraction, they let you build, send, and receive HTTP requests and responses yourself. If you want fine-grained control about what is happening or would like to integrate aedifion.io's HTTP API into your own code, HTTP libraries are the best choice.
* [Graphical REST clients](./#graphical-rest-clients) are GUIs for HTTP that let you conveniently build, view, send, and parse HTTP requests and responses. If you just want to explore and try out the APIs, this is the best way. 
* [API clients](./#auto-generated-api-clients) can be automatically generated from our API specification for 40+ different programming languages, e.g., Python, Rust, C++, Go, Javascript, and many more. These API clients are, basically, high-level wrappers around standard HTTP libraries and relieve you of much of the boiler plate code for building requests, handling errors, and parsing responses.

### Command line tools

Here's how to do a simple HTTP GET request including basic authentication using the two most popular command line tools for opening URLs.

{% tabs %}
{% tab title="curl" %}
Request:

```bash
curl https://api.aedifion.io/v2/user 
    -X GET
    -u john.doe@aedifion.com:mys3cr3tp4ssw0rd
```

The `-X GET` option is optional as GET requests are the standard. You will need it, however, for issuing `POST`, `PUT`, and `DELETE` requests.
{% endtab %}

{% tab title="wget" %}
Request:

```bash
wget http://api.aedifion.io/v2/user
    -O- 
    --http-user=john.doe@aedifion.com 
    --http-password=mys3cr3tp4ssw0rd 
```

The `-O-` option tells wget to print to stdout instead of saving the response in a file.
{% endtab %}
{% endtabs %}

### HTTP libraries

Here are examples in different programming languages for a simple HTTP GET request.

{% tabs %}
{% tab title="Python" %}
For Python, we greatly recommend the [requests](http://docs.python-requests.org/en/master/) library. Alternatives are Python's built-in [urllib](https://docs.python.org/3/library/urllib.html) or [httplib](https://docs.python.org/3.4/library/http.html) modules.

```python
import requests
john = ("john.doe@aedifion.com", "mys3cr3tp4ssw0rd")
r = requests.get("https://api.aedifion.io/v2/user", auth=john)
print(f"{r.status_code} - {r.text}")
```
{% endtab %}

{% tab title="Other languages" %}
Coming soon 🐒
{% endtab %}
{% endtabs %}

### Graphical REST clients

#### Swagger UI

The Swagger User Interface \(UI\) is a Javascript page that is auto-generated from our API specifications and allows you to interactively explore our APIs from within your browser without any additional software. We host it for the two APIs at the following locations:

* Stable API: [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/)
* Development API: [https://api-dev.aedifion.io/ui/](https://api-dev.aedifion.io/ui/)

#### Postman

1. Download the Postman app from [https://www.getpostman.com/downloads/](https://www.getpostman.com/downloads/)
2. Start the app. You may skip the login.
3. Build your own story book of HTTP requests
   * Hint: Add a new environment in which you configure the variables `email` and `password` that safely store your credentials. This allows you to save your requests in collections and share these with others without leaking your credentials.

### Auto-generated API clients

Using the online [Swagger Editor](https://editor.swagger.io/) \(or the [Swagger Codegen](https://swagger.io/tools/swagger-codegen/) desktop application\), you can auto-generate API clients for 40+ different programming languages in a few steps:

1.  Go to [https://editor.swagger.io/](https://editor.swagger.io/)
2. Copy-paste the JSON from [https://api.aedifion.io/swagger.json](https://api.aedifion.io/swagger.json) into the left-side editor field \(convert to YAML if you want to\).
3. On the top bar, click _Generate Client_ and choose the desired language.
4. Download the ZIP Archive and unzip it.
5. Depending on the programming language, further customization steps might be necessary which we detail below.

{% tabs %}
{% tab title="Python" %}
#### Generating a Python API client

1. Open the file `swagger_client/configuration.py` for editing and configure the API url as well as your login credentials.

```python
# Default Base url
self.host = "https://api.aedifion.io"

#...

# Username for HTTP basic authentication
self.username = "john.doe@aedifion.com"
# Password for HTTP basic authentication
self.password = "mys3cr3tp4ssw0rd"
```

     2.  From the base directory of the unzipped archive, open a Python shell.

```python
$: python3
>>> import swagger_client
>>> user_api = swagger_client.UserApi()
>>> r = user_api.get_user()
>>> print(r.user)
{'company_id': 1,
 'email': 'john.doe@aedifion.com',
 'first_name': 'John',
 'id': 1,
 'last_name': 'Doe'}
>>>
```
{% endtab %}

{% tab title="Other languages" %}
Coming soon 🐒

{% hint style="info" %}
The process for generating API clients in other languages is similar. But make sure that you set the APIs base URL as well as your login credentials correctly.
{% endhint %}
{% endtab %}
{% endtabs %}

## Further resources

* Explore our [tutorials](guides-and-tutorials/) that show you how to, e.g., manage user, projects, and permission, or setup alarms, using the HTTP API.









