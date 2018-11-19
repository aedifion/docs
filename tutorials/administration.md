---
description: >-
  Guide and tutorial for the administration of users, projects, and associated
  permissions via the HTTP API.
---

# Administration

## Overview

In this tutorial, we cover user and project management as well as the associated role-based access control \(RBAC\) system. You will learn how to add, modify, and delete users and projects as well as how to add new roles, manage permissions of a role, and assign roles to users -- all easily automatable through the [HTTP API](../developers/api-documentation/).

If you haven't already, please shortly look over the definition of [special terminology](../glossary.md). We recapitulate the most important concepts and their relation with a focus on their administration in the following:

* _Companies_ are administration-wise the topmost entity on the aedifion.io platform_._ Companies are created and managed by aedifion. They can neither be created nor modified or deleted by their users. Companies are strictly separated administration domains that never share any resources.
* A company has arbitrary many _users_ and each user belongs to exactly one company. __Users can modify their personal details and \(with sufficient permissions\), create new users within their company, and delete their own accounts.
* A company has arbitrary many _projects_ and each project belongs to exactly one company. Projects can be considered administrative subspaces within a company.
* _Roles_ define permissions for resource access for a specific project or company-wide, i.e., spanning all of that company's projects. A special "admin" role is automatically created by default for each company and each project with all permissions. A role can be assigned to arbitrary many users in the company and a single user can have multiple roles. They thus connect users to projects.

### Prerequisites

To execute the examples provided in this tutorial, the following is needed:

* A valid login \(username and password\) to the aedifion.io platform. If you do not have a login yet, please [contact us](../contact.md) regarding a demo login. The login used in the example will not work!
* Optionally, a working installation of [Python](https://www.python.org/) or [Curl](https://curl.haxx.se/).

## Managing the company

We currently do not save any meta-data about companies except for a name and a short description. These attributes are deliberately read-only and can only be changed by aedifion. Please [contact](../contact.md) us in such matters.

## Managing projects

In this section, we first add a demo project, then modify its details, and finally delete it.

### Adding projects

Projects are created through the `POST /v2/project` API endpoint. You need to supply the details of the new project in [JSON-format](https://www.json.org/) in the body of this request. The JSON should contain the following name/value-pairs:

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
      <td style="text-align:left"><b>company_id</b>
      </td>
      <td style="text-align:center">integer</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The numeric id of your company (can be obtained through the <code>GET /v2/company</code> endpoint
        if unknown).</td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>name</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">body (JSON)</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The name of the project.</td>
      <td style="text-align:left">simu_01</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>description</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">body (JSON)</td>
      <td style="text-align:center">no</td>
      <td style="text-align:left">An optional free text description of this project, e.g., something to
        help others understand what this project is about.</td>
      <td style="text-align:left">My first simulation project.</td>
    </tr>
  </tbody>
</table>This information must be encoded as a [valid JSON](https://jsonlint.com/):

```javascript
{
	"company_id": 1,
	"description": "My first simulation project.",
	"name": "simu_01"
}
```

You can write this JSON by hand, use an [editor](https://jsoneditoronline.org/), or construct it using the programming language of your choice \(see Python example below\). Finally, the JSON is posted in the request's body to the `POST /v2/project` API endpoint.

{% tabs %}
{% tab title="Python" %}
1. Open an interactive python shell.
2. Paste the following code line by line.

```python
import requests
auth = ("john.doe@newco.com", "s3cr3tp4ssw0rd")
new_project =  {
	"company_id": 1,
	"description": "My first simulation project.",
	"name": "simu_01"
}
r = requests.post("https://api.aedifion.io/v2/project", auth=auth, json=new_project)
```

3. Inspect the response code and body.

```python
print(r.status_code)  # prints the HTTP status code
print(r.text)         # prints the raw answer
print(r.json())       # parses the answer into a JSON
```
{% endtab %}

{% tab title="Curl" %}
1. Copy-paste the JSON into a file, e.g., named _new\_project.json._
2. Open a commandline.
3. Execute the following command.

```bash
curl http://localhost:5001/v2/project
  -X POST
  -u john.doe@newco.com:s3cr3tp4ssw0rd
  -d @new_project.json 
  -H "Content-Type: application/json"  
```
{% endtab %}

{% tab title="Swagger UI" %}
1. Point your browser to [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/).
2. Click _Authorize_ on the upper right and provide your login.
3. From the main tags \(Meta, Company, ...\) select the _Project_ tag ,then the `POST /v2/project` endpoint \(green\).
4. Copy-paste the above JSON into the value of the _project_ parameter.
5. Click "_Try it out!_".
6. Inspect the response body and code.
{% endtab %}
{% endtabs %}

The answer is a JSON-parsable object containing similar to the following:

```javascript
{
    'success': True,
    'operation': 'create', 
    'resource': {
        'project': {
            'company_id': 1, 
            'description': 'My first simulation project.', 
            'id': 20, 
            'name': 'simu_02'
        }, 
        'role': {
            'id': 35,
            'name': 'admin',
            'project_id': 20, 
            'description': 'Admin role for project simu_02',                                  
            'rights_level': 0            
            'authed_endpoints': [
                1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74
                ], 
            'authed_tags': [
                {'id': 1, 'key': 'name', 'read': True, 'value': '*', 'write': True}
                ],  
        }
    }
}
```

Let's inspect the answer piece-by-piece.

* `'success': True` is the confirmation that the request was successful.
* `'operation': 'create'` tells us that a new resource was created.
* The `resource` field contains two further objects, i.e., the resources that were created
  * The first resource, `'project':{...}` , is the project that we set out to create. Note how the new project has been assigned a unique id \(20\) in addition to the information that we provided in the `POST` request.
  * The second resource, `'role:{...}'`, has been created automatically. It is the default _admin_ role for that project and has been automatically assigned to you, i.e., the user that who the project. Do not worry about the other details of the newly created role. We will cover them later in the section on [managing permissions](administration.md#managing-permissions).

Log in to the [frontend](https://www.aedifion.io) or call the `GET /v2/user/projects` endpoint to see the newly created simulation project listed in your projects. 

{% hint style="warning" %}
The created project will not have any datapoints. It is a mere placeholder so far. Either the project must be configured to receive data from a logger \(by the aedifion staff\) or data must be [imported by hand](data-import.md).
{% endhint %}

### Modifying projects

Let's imagine there was an error in the project name and description and we wanted to fix it. The `PUT /v2/project/{project_id}` endpoint is our friend. Note that `{project_id}` is a [_path parameter_](https://swagger.io/docs/specification/describing-parameters/#path-parameters), i.e., a parameter that is conveyed directly within the path of the requested endpoint. Altogether, the following parameters can be provided. Note that none of the body parameters \(those that are conveyed in a JSON in the _body_ of the request\) are required -- even an empty update would be accepted.

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
      <td style="text-align:left">The numeric id of the project.</td>
      <td style="text-align:left">20</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>name</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">no</td>
      <td style="text-align:left">The new name of the project.</td>
      <td style="text-align:left">SIMU02</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>description</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">no</td>
      <td style="text-align:left">The new description of the project.</td>
      <td style="text-align:left">My second simulation project.</td>
    </tr>
  </tbody>
</table>As in [the previous example](administration.md#adding-projects), we encode the body parameters in a single [JSON-formatted](https://www.json.org/) object.

```javascript
{ 
    "name": "SIMU02",
    "description": "My second simulation project."
}
```

 And POST it to the API using the method of choice \(c.f. Python, Curl, and Swagger UI examples above\). Of course, we need to substitute the `project_id` path parameter by the actual id of the project, i.e., 20 in our running example.

{% tabs %}
{% tab title="Python" %}
```python
import requests
auth = ("john.doe@newco.com", "s3cr3tp4ssw0rd")
update =  {
	"name": "SIMU02",
	"description": "My second simulation project."
}
r = requests.put("https://api.aedifion.io/v2/project/20", auth=auth, json=update)
```
{% endtab %}

{% tab title="Curl" %}
```bash
curl https://api.aedifion.io/v2/project/19
  -X PUT 
  -H 'Content-Type: application/json'
  -u john.doe@newco.com:s3cr3tp4ssw0rd
  -d '{
    "description": "another description",
    "name": "SIMU04"
   }' 
```
{% endtab %}

{% tab title="Swagger UI" %}
1. Point your browser to [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/).
2. Click _Authorize_ on the upper right and provide your login.
3. From the main tags \(Meta, Company, ...\) select the _Project_ tag ,then the `PUT /v2/project/{project_id}` endpoint \(orange\).
4. Enter the numeric id of the previously created project.
5. Copy-paste the above JSON into the value of the _project_ parameter.
6. Click "_Try it out!_".
7. Inspect the response body and code.
{% endtab %}
{% endtabs %}

The answer comes in JSON-format and confirms our changes.

```javascript
{
  "success": true,
  "operation": "update",
  "resource": {
    "id": 20,
    "company_id": 1,
    "name": "SIMU02",
    "description": "another description",    
  }
}
```

### Deleting projects

Let's leave our project space tidy and clean up our dummy simulation project. Projects are deleted using the `DELETE /v2/project/{project_id}` endpoint. It only requires the numeric project id as path parameter and nothing else.

| Parameter | Datatype | Type | Required | Description | Example |
| :--- | :---: | :---: | :---: | :--- | :--- |
| **project\_id** | integer | path | yes | The numeric id of the project. | 20 |

Calling this endpoint is even more simple than before.

{% tabs %}
{% tab title="Python" %}
```python
import requests
auth = ("john.doe@newco.com", "s3cr3tp4ssw0rd")
update =  {
	"name": "SIMU02",
	"description": "My second simulation project."
}
r = requests.put("https://api.aedifion.io/v2/project/20", auth=auth, json=update)
```
{% endtab %}

{% tab title="Curl" %}
```bash
curl https://api.aedifion.io/v2/project/20
  -X DELETE
  -u john.doe@newco.com:s3cr3tp4ssw0rd
```
{% endtab %}

{% tab title="Swagger UI" %}
1. Point your browser to [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/).
2. Click _Authorize_ on the upper right and provide your login.
3. From the main tags \(Meta, Company, ...\) select the _Project_ tag ,then the `DELETE /v2/project/{project_id}` endpoint \(red\).
4. Enter the numeric id of the previously created project.
5. Copy-paste the above JSON into the value of the _project_ parameter.
6. Click "_Try it out!_".
7. Inspect the response body and code.
{% endtab %}
{% endtabs %}

As response, the deleted project is returned. 

```javascript
{
  "success": true,
  "operation": "delete",
  "resource": {
    "id": 20,
    "name": "SIMU02", 
    "company_id": 1,
    "description": "My second simulation project."
  }
}
```

{% hint style="danger" %}
Deleting a project deletes the project together with _all project-specific meta data_, including all roles, datapointkeys, tags, and so forth. Handle this endpoint with great care and restrict access to it.
{% endhint %}

## Managing users

The flow for managing users is mostly the same as for [managing projects](administration.md#managing-projects). There is, however, one crucial difference: _Users can add other users but cannot modify or delete them_. More figuratively, once a user is born into the company, he/she immediately enters adulthood.

### Adding users

New users are created through the `POST /v2/user` API endpoint. As usual, you need to supply the details of the new user in [JSON-format](https://www.json.org/) in the body of this request. The JSON should contain the following name/value-pairs:

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
      <td style="text-align:left"><b>company_id</b>
      </td>
      <td style="text-align:center">integer</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The numeric id of your company (can be obtained through the <code>GET /v2/company</code> endpoint
        if unknown).</td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>firstName</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">body (JSON)</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The first name of the new user.</td>
      <td style="text-align:left">Jane</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>lastName</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">body (JSON)</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The last name of the new user.</td>
      <td style="text-align:left">Doe</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>email</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The email address of the new user (must not already exist).</td>
      <td style="text-align:left">jane.doe@newco.com</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>password</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The new users's initial password.</td>
      <td style="text-align:left">ch4ng3m3s00n</td>
    </tr>
  </tbody>
</table>A [valid JSON](https://jsonlint.com/) encoding looks like this:

```javascript
{
  "company_id": 1,
  "email": "jane.dow@newco.com",
  "firstName": "Jane",
  "lastName": "Doe",
  "password": "ch4ng3m3s00n"
}
```

We post this request with the login of our _current user_ since Jane Doe cannot create herself, evidently. Refer to the [previous section](administration.md#managing-projects) for examples on how to issue this request.

The response has the usual format. Note that just as for the new project before, the new user has received a unique numeric id \(which we will need later to assign roles to this user\).

```javascript
{
  "success": true,
  "operation": "create",
  "resource": {
    "id": 84,
    "company_id": 1,    
    "firstName": "Jane",
    "lastName": "Doe"   
    "email": "xyz@newco.com",
  }
}
```

{% hint style="info" %}
For security reasons the user's password is not returned. Indeed, passwords are only saved in [hashed form](https://en.wikipedia.org/wiki/Key_derivation_function#Uses_of_KDFs) on the aedifion servers and thus cannot be retrieved in clear text form.
{% endhint %}

### Modifying users

The new user, Jane Doe, should change her password after she receives her account information from John Doe who initially set up her account. She can also change other personal details:

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
      <td style="text-align:left"><b>firstName</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">body (JSON)</td>
      <td style="text-align:center">no</td>
      <td style="text-align:left">The new first name of the user.</td>
      <td style="text-align:left">Jane Marie</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>lastName</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">body (JSON)</td>
      <td style="text-align:center">no</td>
      <td style="text-align:left">The new last name of the user.</td>
      <td style="text-align:left">Doe</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>email</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The new email address of the user (must not already exist).</td>
      <td style="text-align:left">jm.doe@newco.com</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>password</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The new password of the user.</td>
      <td style="text-align:left">my0wnsup3rs3cr3tpw</td>
    </tr>
  </tbody>
</table>The new user, Jane Doe, must put this request herself as only she is allowed to change her personal details. Regarding the previous examples, we thus have to change the authorization. Note that we can also simply leave out the optional _lastName_ in our request, since we do not intend to change it.

{% tabs %}
{% tab title="Python" %}
```python
import requests
auth = ("jane.doe@newco.com", "ch4ng3m3s00n")
update =  {
	"firstName": "Jane Marie",
	"email": "jm.doe@newco.com",
	"password": "my0wnsup3rs3cr3tpw"
}
r = requests.put("https://api.aedifion.io/v2/project/20", auth=auth, json=update)
```
{% endtab %}

{% tab title="Curl" %}
```bash
curl https://api.aedifion.io/v2/user
  -X PUT
  -H 'Content-Type: application/json'
  -u jane.doe@newco.com:ch4ng3m3s00n
  -d '{
    "firstName": "Jane Marie",
    "email": "jm.doe@newco.com",
    "password": "my0wnsup3rs3cr3tpw"
   }' 
```
{% endtab %}

{% tab title="Swagger UI" %}
1. Point your browser to [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/).
2. Click _Authorize_ on the upper right and provide the login of the new user.
3. From the main tags \(Meta, Company, ...\) select the _User_ tag ,then the `PUT /v2/user` endpoint \(orange\).
4. Copy-paste the above JSON into the value of the _user_ parameter.
5. Click "_Try it out!_".
6. Inspect the response body and code.
{% endtab %}
{% endtabs %}

As usual, we receive a confirmation and the touched resource in the response:

```javascript
{
  "success": true,
  "operation": "update",
  "resource": {
    "id": 84,
    "company_id": 1,
    "email": "jm.doe@newco.com",
    "firstName": "Jane Marie",
    "lastName": "Doe"
  }
}
```

{% hint style="info" %}
You may have noticed that this call did not require the user's id, unlike the `PUT /v2/project/{project_id}` endpoint which required the project's id as a path parameter. The reason is that users can only modify themselves. Which user to modify is thus already implied by the identity of the user that logs in.
{% endhint %}

### Deleting users

To delete a user, just call the `DELETE /v2/user` endpoint with the login of the user that should be deleted. Again, this implied that a user can only delete him/herself. The call is thus even simpler than the analog call for [deleting a project](administration.md#deleting-projects).

{% hint style="danger" %}
Deleting a user deletes the user together with _all user-specific meta data_, including favorites, plotviews, and so forth. Handle this endpoint with great care.
{% endhint %}

## Managing permissions

### Conceptual overview

### Adding roles

### Modifying roles

### Assigning roles to users

### Deleting roles

