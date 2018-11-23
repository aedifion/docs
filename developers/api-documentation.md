---
description: Reference for the aedifion HTTP API.
---

# HTTP API

## Overview

The aedifion.io HTTP API is completely specified in [OpenAPI Specificiation Version 2 format](https://swagger.io/docs/specification/2-0/what-is-swagger/) from which we generate an interactive API documentation that allows you to explore and try out our APIs.

We currently run two APIs:

* _Production API:_ Stable API that receives updates and new features only after an intensive testing phase in our development environment.
  * Base URL: [https://api.aedifion.io](https://api.aedifion.io)
  * Interactive documentation and user interface: [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/)
* _Development API:_ A semi-stable __API that receives updates and new features after a short internal testing phase.
  * Base URL: [https://api-dev.aedifion.io](https://api-dev.aedifion.io)
  * Interactive documentation and user interface: [https://api-dev.aedifion.io/ui/](https://api-dev.aedifion.io/ui/)

## Accessing the HTTP API

### Bare HTTP

{% tabs %}
{% tab title="Python" %}
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

### Auto-generated API clients

From our API specification you can automatically generate clients for the aedifion.io API that will relieve you of writing boiler plate code. Currently, you may choose among 40+ programming languages, e.g., Python, Rust, C++, Go, Javascript, and many more.

{% tabs %}
{% tab title="Python" %}
#### Generating a Python API client

1. Go to [https://editor.swagger.io/](https://editor.swagger.io/)
2. Copy-paste the JSON from [https://api.aedifion.io/swagger.json](https://api.aedifion.io/swagger.json) into the left-side editor field \(convert to YAML if you want to\).
3. On the top bar, click _Generate Client_ and choose _Python._
4. Download the ZIP Archive and unzip it.
5. Open the file `swagger_client/configuration.py` for editing and set the following lines.

```python
# Default Base url
self.host = "https://api.aedifion.io"

#...

# Username for HTTP basic authentication
self.username = "john.doe@aedifion.com"
# Password for HTTP basic authentication
self.password = "mys3cr3tp4ssw0rd"
```

     6.  From the base directory of the unzipped archive, open a Python shell.

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
The process for generating API clients in other languages is similar. But make sure that you set the APIs base URL as well as your login credentials.
{% endhint %}
{% endtab %}
{% endtabs %}

