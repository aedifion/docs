---
description: >-
  Tutorial for the administration of users, projects, and associated permissions
  via the HTTP API.
---

# Administration

## Overview

In this tutorial, we cover user and project management as well as the associated role-based access control \(RBAC\) system. You will learn how to add, modify, and delete users and projects as well as how to add new roles, manage permissions of a role, and assign roles to users -- all easily automatable through the [HTTP API](../developers/api-documentation.md).

If you haven't already, please skim through the definition of [special terminology](../glossary.md). We recapitulate the most important concepts and their relation with a focus on their administration in the following:

* _Companies_ are administration-wise the topmost entity on the aedifion.io platform_._ Companies are created and managed by aedifion. They can neither be created nor modified or deleted by users. Companies are created by aedifion as strictly separated administration domains that never share any resources.
* A company has arbitrary many _users_ and each user belongs to exactly one company. Users can \(with sufficient permissions\) create and add further users to their company, but can only modify or delete their own account and never that of another user.
* A company has arbitrary many _projects_ and each project belongs to exactly one company. Projects can be considered administrative subspaces within a company.
* _Roles_ define a set of permissions. A special _admin_ role is automatically created by default for each company and each project with all permissions in the respective scope. A role can be assigned to arbitrary many users in the company and a single user can have multiple roles.

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

| Parameter | Datatype | Type | Required | Description | Example |
| :--- | :---: | :---: | :---: | :--- | :--- |
| **name** | string | body \(JSON\) | yes | The name of the project. | simu\_01 |
| **description** | string | body \(JSON\) | no | An optional free text description of this project, e.g., something to help others understand what this project is about. | My first simulation project. |

This information must be encoded as a [valid JSON](https://jsonlint.com/):

```javascript
{
	"name": "simu_01",
	"description": "My first simulation project."
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
	"name": "simu_01",
	"description": "My first simulation project."
}
r = requests.post("https://api.aedifion.io/v2/project", 
	              auth=auth, json=new_project)
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
            'authed_endpoints': [
                1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74
            ], 
            'authed_tags': [
                {'id': 1, 'key': 'name', 'read': True, 'value': '*', 'write': True}
            ]  
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
r = requests.put("https://api.aedifion.io/v2/project/20", 
				 auth=auth, json=update)
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
r = requests.put("https://api.aedifion.io/v2/project/20", 
				 auth=auth, json=update)
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
  "email": "jane.dow@newco.com",
  "firstName": "Jane",
  "lastName": "Doe",
  "password": "ch4ng3m3s00n"
}
```

We post this request with the login of our _current user_ since Jane Doe cannot create herself, evidently. Refer to the [previous section](administration.md#managing-projects) for examples on how to issue this request.

The response has the usual format. Note that just as for the new project before, the new user has received a unique numeric id \(which we will need later to assign roles to this user\) and has been added to the company of the creating user.

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
r = requests.put("https://api.aedifion.io/v2/project/20", 
 				 auth=auth, json=update)
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

Permissions to access resources on the aedifion.io platform can be controlled in a very fine granular manner using a role-based access control \(RBAC\) system. In this section, we will review the basic concepts behind the permission management then go through the process of defining a new role, modifying its permissions, assigning it to a user, and finally deleting the role.

### Conceptual overview

We first have to define what we mean by _permission to access a resource:_

* A _resource_ is any kind of collection or individual piece of \(meta-\)data on the aedifion.io platform, e.g., a project, datapointkey, tag, datapoint, time series, setpoint, alert, role, and so forth. 
* These resources are _accessed_ through different [HTTP API endpoints](../developers/api-documentation.md). An API endpoint may touch on more than one resource.
* _Permissions_ grant the right to call an API endpoint on a specific resource, e.g., modify \(the endpoint\) project _A_ \(the resource\). Permissions either have project or company scope. _Company-scoped_ permissions apply to all of that company's resources including all projects while _project-scoped_ permissions are limited to the resources of a single project.
* An important resource in projects are datapoints as they are usually associated with real time series data, e.g., measured from a real building. Due to their special importance, permissions can be further limited to specific datapoints using TagAuths_. TagAuths_ allow filtering over the names or [tags](tagging.md) of datapoints to limit defined permissions, e.g., to _"all datapoints with unit=CO2"  or "all datapoints of component x"_. Write access to a datapoint, e.g., posting a room temperature setpoint or switching off the heating, is a more critical action than just reading the current or paste state of a datapoint. Thus, TagAuths additionally allow limiting access to read or write \(or both\).

Permissions and TagAuths are bundled in _two types of roles_:

* _Company roles_ define resource access within the whole company, i.e., all resource access permissions granted through a company role are company-scoped. E.g., providing permission to endpoint `GET /v2/project/{project_id}` allows retrieving the meta-data of all projects of that company. Hence company roles are low maintenance as they provide broad and automatic access even to new projects that are added to the company's portfolio. Company roles are best used as high-level administrative roles and should thus be assigned sparingly and carefully.
* _Project roles_ define permissions for single projects, i.e., all permissions granted through a project role are project-scoped. E.g., granting permission to `GET /v2/project/{project_id}` within a project role allows retrieving the meta-data of only the project associated with that role. Project roles are thus the means of choice to grant fine granular access to \(meta-\)data in the majority of use cases. They can be assigned more generously and freely than company roles. It should also be noted that a project role cannot grant permission on company-related resources, i.e., on the API endpoints with _Company_ tag such as `POST /v2/company/user` __or `PUT /v2/company/role/{role_id}`.

Putting it all together, a user's _permission set_ is defined as the union over the permission sets of all his/her project and company roles. A user is then granted access to call a certain API endpoint on a certain set of resources if the corresponding permission is in his/her permission set. 

{% hint style="info" %}
Intuitively, we can think of _company roles_ as being one hierarchy above _project roles,_ i.e., a company role can grant all permissions that a project role can grant - and more. In particular, the _permission set_ of a company role that grants access to a set of endpoints and datapoints is always a superset of the permission set of a project role for the same set of endpoints and datapoints.
{% endhint %}

Finally, it is important to note that the permission system is a strict _whitelisting_ approach with the following important properties:

* Per default, access to all resources is forbidden except for the user's own meta-data.
* Access to a resource must be explicitly granted through a role.
* If a user is granted the permission to access resource _R_ through role _A_ then this access is not revoked by any other assigned role _B_ that does not grant access to _R._
* A user can only grant resource access permissions to other users/roles that are in his/her own permission set \(to [prevent privilege escalation](administration.md#preventing-escalation-of-privileges)\).

{% hint style="info" %}
**Example:**

Imagine the following setup:

* Company _NewCo_ has two company roles:
  * Role _admin_ with permissions to read and write all resources.
  * Role _reader_ with permissions to read all resources.
* _NewCo_  has two projects:
  * Project _Headquarters_ with roles _reader_ and _writer._
  * Project _FactoryFloor_ with roles _reader_ and _writer._
* User Alice is _NewCo's_ system administrator and has roles {_NewCo/admin_},
* User Bob is a worker and has roles {_NewCo/reader_, _Newco/FactoryFloor/writer_}.
* _NewCo_ uses their headquarters as a demo building and grants role {_NewCo/Headquarters/reader_} to any guest.

Now, what permissions do Alice, Bob, and the guests have?

* Alice has access to all of _NewCo's_ resources and projects __since she is assigned the root _admin_ company role. She does not need any specific project role to edit projects' meta data as her company role extends to all projects.
* Bob's company role _NewCo/reader_ allows him to read all of _NewCo's_ resources but not to change them \(e.g., granting access only to `GET` methods and not to `POST`, `PUT`, or `DELETE`\). Since Bob is working in the factory, he is also granted permissions to change datapoints from the factory floor, e.g., write a setpoint for the room temperature, through the project role _NewCo/FactoryFloor/writer._
* Guests only have rights to read resources \(including datapoints\) of the project _Headquarters,_ but cannot access any other project or company resources. 
{% endhint %}

Now, enough for the boring theory, let's dive into practice.

### Viewing roles

We first examine the roles that we already got. To this end, we use the `GET /v2/user` endpoint which will return a comprehensive summary of the logged-in user's resources. We use the following script to parse the output to a more readable form:

{% tabs %}
{% tab title="Python" %}
```python
import requests

# Short print for long lists
def list2string(l):
    if len(l) <= 6:
        return ", ".join(map(str, l))
    else:
        return ", ".join(map(str, l[:3])) + ", ..., " + ", ".join(map(str, l[-3:]))

# Print a user's roles
def printUserRoles(auth):
    r = get(api_url + "/v2/user", auth=auth)
    if r.status_code != 200:
        print(r.text)
    else:
        j = r.json()
        print("User '{} {}'".format(j['user']['firstName'], j['user']['lastName']))
        print("- Project roles:")
        for r in j['roles']:
            print(" -> {} ({})".format(r['name'], r['description']))
            print("  * authorized endpoints: {}".format(list2string(r['authed_endpoints'])))
            print("  * authorized tags:")
            for t in r['authed_tags']:
                print("   + {}".format(t))
        print("- CompanyRoles")
        for r in j['companyroles']:
            print(" -> {} ({})".format(r['name'], r['description']))
            print("  * authorized endpoints: {}".format(list2string(r['authed_endpoints'])))

# credentials
john = ("john.doe@aedifion.com", "s3cr3tp4assw0rd")

# make the GET request and parse the answer
printUserRoles(john)
```
{% endtab %}
{% endtabs %}

The response is similar to the following shortened output:

```text
User 'John Doe'
- Project roles:
 -> admin (Admin project role for MainBuilding)
  * authorized endpoints: 1, 2, 3, ..., 72, 73, 74
  * authorized tags:
   + {'id': 1, 'key': 'name', 'read': True, 'value': '*', 'write': False}
   + {'id': 3, 'key': 'name', 'read': True, 'value': 'bacnet512-4120L01_DASBM06_Abluftventilator', 'write': True}
   + {'id': 4, 'key': 'name', 'read': True, 'value': 'bacnet512-4120L01_VEGYSW__Abluft-Druck', 'write': True}
   + {'id': 5, 'key': 'name', 'read': True, 'value': 'bacnet510-4120L04_VEGYSW__Druck-Abluft', 'write': True}
   + {'id': 6, 'key': 'name', 'read': True, 'value': 'bacnet510-4120L04_VEGYSW__Druck-Zuluft', 'write': True}
   + {'id': 7, 'key': 'name', 'read': True, 'value': 'bacnet512-4120L022VEGSHSB_Anlage-L22', 'write': True}
- CompanyRoles
 -> reader (Reader company role for aedifion GmbH): 
  * authorized endpoints: 1, 2, 3, ..., 72, 73, 74
  * authorized tags:
   + {'id': 1, 'key': 'name', 'read': True, 'value': '*', 'write': False}  
```

The output contains the following information:

* User John Doe has one project role, _admin_ on the _MainBuilding_ project. 
  * The admin project role grants access to endpoints 1, 2, 3, ..., 74. These ids correspond to the output of `GET /v2/meta/endpoints`and determine the endpoints to which I have access - all endpoints, in this case.
  * The admin project role grants read access to all datapoints \(the tag `'key':'name', value': '*'` filters by name and allows all values\) and write access to five explicitly specified datapoints, e.g., `bacnet512-4120L01_DASBM06_Abluftventilator.`
* The user has one company role, _reader_, which grants read access to all endpoints.

### Adding roles

#### Automatic role creation

Let's [add another project](administration.md#adding-projects) then re-run the above script and see what happens.

```python
newproject = {
    "company_id":1, 
    "name":"TestProject01", 
    "description":"A first test project."
    }
requests.post(api_url + "/v2/project", auth=john, json=newproject)
```

```text
- Project roles:
...
 -> admin (Admin role for project TestProject01)
  * authorized endpoints: 1, 2, 3, ..., 72, 73, 74
  * authorized tags:
   + {'id': 1, 'key': 'name', 'read': True, 'value': '*', 'write': True}
...
```

Note that one new project role has appeared. This _admin_ role was automatically created when we created the new project and assigned to the creating user.

{% hint style="info" %}
Automatic creation and assignment of the _admin_ role on new projects ensures that there is at least one user who has access to the project and can grant access to other users of his/her company.
{% endhint %}

#### Adding project roles

In many use cases, we would, however, like to restrict access to our new _TestProject01._ To this end, we have to define a new role which is done through the `POST /v2/project/{project_id}/role` endpoint. 

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
      <td style="text-align:left">The numeric id of the project for which to create the role.</td>
      <td style="text-align:left">21</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>name</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">body (JSON)</td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The name of the new role.</td>
      <td style="text-align:left">Maintainer</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>description</b>
      </td>
      <td style="text-align:center">string</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">A short free text description of the new role that should shortly describe
        the permissions granted by this role.</td>
      <td style="text-align:left">Limited access for maintainers of TestProject01.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>authed_<br />endpoints</b>
      </td>
      <td style="text-align:center">list of integer</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">yes</td>
      <td style="text-align:left">The list of numeric ids of endpoints that this role grants permissions
        for (ids can be obtained through <code>GET /v2/meta/endpoints</code>). <code>id=0</code> is
        a special short-hand for all endpoints.</td>
      <td style="text-align:left">[22, 52]</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>authed_<br />tags</b>
      </td>
      <td style="text-align:center">list of object</td>
      <td style="text-align:center">
        <p>body</p>
        <p>(JSON)</p>
      </td>
      <td style="text-align:center">no</td>
      <td style="text-align:left">The list of tags that this role grants permissions for. A tag has a <em>key</em> (string)
        and <em>value</em> (string) as well as <em>read</em> (boolean) and <em>write</em> (boolean)
        flags.</td>
      <td style="text-align:left">[{"key":"name", "value": "*", "read": True, "write": False}]</td>
    </tr>
  </tbody>
</table>We do not care about _authed\_tags_ at this point since the specified endpoints do not touch datapoint resources. The request is then built as follows:

{% tabs %}
{% tab title="Python" %}
```python
# Helper function to retrieve the numeric id of an endpoint         
def getEndpointId(method, path):
    j = get(api_url + "/v2/meta/endpoints").json()
    for endpoint in j:
        if endpoint['path'] == path and endpoint['request_method'] == method:
            return endpoint['id']
    return None

new_project_id = 21
id_get_project = getEndpointId("GET", "/v2/project/{project_id}")
id_put_project = getEndpointId("PUT", "/v2/project/{project_id}")
newrole = {
    "name": "Maintainer",
    "description": "Limited access for maintainers of TestProject01",
    "authed_endpoints": [id_get_project, id_put_project]
}
r = post(api_url + "/v2/project/{}/role".format(new_project_id), 
         auth=john, json=newrole)
print(r.text)
```
{% endtab %}

{% tab title="Swagger UI" %}
1. Navigate to the _Meta_ tag and drop down the `GET /v2/meta/endpoints` endpoint.
2. Click _Try it out!_ and note down the numeric ids of the desired endpoints.
3. Navigate to the _Project_ tag and drop down the `POST /v2/project/{project_id}/role` endpoint.
4. Enter the _project\_id._ Copy paste the example value into the _role\_definition_ and edit _name, description,_ and _authed\_endpoints_. Delete the dummy _authed\_tags_ object.
5. Click _Try it out!_ and inspect the response.
{% endtab %}
{% endtabs %}

As usual, the response confirms success and returns the created role.

```javascript
{
    "operation": "create",
    "success": true
    "resource": {
        "id": 41,
        "name": "Maintainer",
        "description": "Limited access for maintainers of TestProject01",        
        "project_id": 21,
        "authed_endpoints": [22,52],
        "authed_tags": [],
    }
}
```

#### Adding company roles

If we have added many projects, a company-wide maintainer role is probably required that defines access on all projects and automatically extends also to future projects that have not been created, yet. [As explained above](administration.md#conceptual-overview), company roles address this objective.

As en example, we create a _company role_ through the _POST /v2/company/role_ endpoint that allows creation, modification, and deletion of projects. The process is the same as for adding a project role.

```python
# Define and add the role
id_post_project   = getEndpointId("POST", "/v2/project")
id_put_project    = getEndpointId("PUT", "/v2/project/{project_id}")
id_delete_project = getEndpointId("DELETE", "/v2/project/{project_id}")
newrole = {
    "name": "Project Admin",
    "description": "Role for maintaining projects company-wide",
    "authed_endpoints": [id_post_project, id_put_project, id_delete_project]
}
r = post(api_url + "/v2/company/role", auth=john, json=newrole)
print(r.text)
```

Response from `POST /v2/company/role`:

```javascript
{
    "success":true,
    "operation": "create",
    "resource": {
        "id": 32,
        "name": "Project Admin",
        "description": "Role for maintaining projects company-wide",
        "company_id": 1,
        "authed_endpoints": [7, 40, 52]
    }
}
```

### Assigning roles to users

We can now assign the company and project roles created in the previous section to other users to grant them \(limited\) access to our company and new project. In the following, we [create a test user](administration.md#adding-users) then assign this user the previously created _Maintainer_ project role as well as the _Project Admin_ company role.

```python
# Create a new user
newuser = {
    "firstName":"Jane", 
    "lastName":"Doe", 
    "email":"jane.doe@aedifion.com", 
    "password":"ch4ng3m3"
}
r = post(api_url + "/v2/user", auth=auth, json=newuser)
new_user_id = r.json()['resource']['id']

# Assign the project role to the new user
project_role_id = 41
r = post(api_url + "/v2/project/role/{}/user/{}".format(project_role_id, new_user_id),
         auth=john)
print(r.text)

# Assing the company role tot the new user
company_role_id = 32
r = post(api_url + "/v2/company/role/{}/user/{}".format(company_role_id, new_user_id),
         auth=john)
print(r.text)
```

In the response, the role assignment is confirmed. Since an assignment from a role to a user is a relation \(role, user\), the resources field returns this relation.

The response to the assignment of the project role:

```javascript
{
    "success": true,
    "operation": "create",
    "resource":{
        "role":{
            "id": 41,
            "name": "Maintainer",
            "project_id": 21,
            "authed_endpoints": [22, 52],
            "authed_tags": [],
            "description": "Limited access for maintainers of TestProject01"
        },
        "user":{
            "id": 102,
            "firstName":"Jane",
            "lastName":"Doe",
            "email":"jane.doe@aedifion.com",            
            "company_id": 1
        }
    }
}
```

Similarly, the response to the assignment of the company role:

```javascript
{
    "success": true,
    "operation": "create",
    "resource": {
        "role": {
            "id": 32,
            "name": "Project Admin",
            "description": "Role for maintaining projects company-wide",
            "company_id": 1,
            "authed_endpoints": [7, 40, 52]
        },
        "user": {
            "id": 102,        
            "firstName": "Jane",
            "lastName":"Doe",
            "email": "jane.doe@aedifion.com",
            "company_id": 1
        }
    }
}
```

Querying the user confirms that both roles have been created:

```python
jane = ("jane.doe@aedifion.com", "ch4ngem3")
printUserRoles(jane)
```

```text
User 'Jane Doe'
- Project roles:
 -> Maintainer (Limited access for maintainers of TestProject01)
  * authorized endpoints: 22, 52
  * authorized tags:
- CompanyRoles
 -> Project Admin (Role for maintaining projects company-wide)
  * authorized endpoints: 7, 40, 52
```

### Modifying roles

Modifying roles is done through the `PUT /v2/project/{project_id}/role/{role_id}` endpoint. The process is the same as [modifying projects](administration.md#modifying-projects) or [users](administration.md#modifying-users). It should however be noted that _authed\_endpoints_ and _authed\_tags_ are completely replaced by the update \(and not appended or preserved otherwise\).

For the sake of completeness, we continue our example and edit all attributes of the previously created _Maintainer_ role \(editing a company role in an analogous manner\).

```python
new_project_id = 21
new_role_id    = 41
id_get_project = getEndpointId("GET", "/v2/project/{project_id}")
id_put_project = getEndpointId("PUT", "/v2/project/{project_id}")
id_get_alerts  = getEndpointId("GET", "/v2/project/{project_id}/alerts")
id_get_dpks    = getEndpointId("GET", "/v2/project/{project_id}/datapointkeys")
role_update = {
    "name":"Extended-Maintainer",
    "description": "Extended access for maintainers of TestProject01",
    "authed_endpoints": [
        id_get_project, 
        id_put_project, 
        id_get_dpks, 
        id_get_alerts
    ],
    "authed_tags": [
        {"key":"name", "value":"*", "read":True, "write":False}
    ],
}
r = put(api_url + "/v2/project/{}/role/{}".format(new_project_id, new_role_id),
        auth=john, json=role_update)
print(r.text)
```

```javascript
{
    "success":true,
    "operation": "update",
    "resource": {
        "id": 41,
        "name": "Extended-Maintainer",
        "project_id": 21,
        "authed_endpoints": [15,22,23,52],
        "description":"Extended access for maintainers of TestProject01"
        "authed_tags": [
            {"id":2,"key":"name","read":true,"value":"*","write":false}
        ]
    }
}
```

### Deleting roles and role assignments

Deleting a role assignment without deleting the role itself is done through the `DELETE /v2/project/role/{role_id}/user/{user_id}` and `DELETE /v2/company/role/{role_id}/user/{user_id}` endpoints for project and company roles, respectively. In contrast, deleting a role including all assignments of that role users is done through the `DELETE /v2/project/{project_id}/role/{role_id}` and `DELETE /v2/company/roles/{role_id}` endpoints

Continuing our running example, we delete the _Project Admin_ company role and the assignment of the _Maintainer_ project role to Jane Doe.

```python
maintainer_project_role_id    = 41
projectadmin_company_role_id  = 32
jane_id = 102

# delete and unassign roles
r = delete(api_url + "/v2/company/role/{}".format(projectadmin_company_role_id), 
           auth=john)
r = delete(api_url + "/v2/project/role/{}/user/{}".format(maintainer_project_role_id, jane_id), 
           auth=john)

printUserRoles(jane)
```

Jane Doe now holds no more roles in our company:

```text
User 'Jane Doe'
- Project roles:
- CompanyRoles
```

However, the _Maintainer_ project role on project _TestProject01_ \(id=21\) still exists, as we can verify by calling the `GET /v2/project/{project_id}/roles` endpoint:

```python
project_id = 21
r = get(api_url + "/v2/project/{}/roles".format(project_id), auth=john)
print("Roles on project: {}".format(project_id))
for role in r.json():
    print("-> {}: {}".format(role['name'], role['description']))
```

```text
Roles on project: 21
-> admin: Admin role for project TestProject01
-> Extended-Maintainer: Extended access for maintainers of TestProject01
```

## Preventing escalation of privileges

Since this is not a crime novel, let's start with a spoiler: 

{% hint style="info" %}
You do not have to do anything to prevent privilege escalation - the RBAC system rigorously enforces that no user can create or assign to him/herself or other users permissions that he/she does not hold him/herself already.
{% endhint %}

Still, a rough understanding of privilege escalation does not hurt, so please read on. Wikipedia defines [privileges escalation](https://en.wikipedia.org/wiki/Privilege_escalation) as 

> the act of exploiting a bug, design flaw or configuration oversight \[...\] to gain elevated access to resources that are normally protected \[...\] The result is that an application \[...\] can perform unauthorized actions.

What would be privilege escalation in the context of the RBAC system presented in this article? Let's assume the following example:

* Eve has a role _Maintainer_ that allows her to create, edit, and delete different project-related resources.
* In particular, Eve has permissions to create and edit roles.
* Eve tries to misuse her permission on `POST /v2/project/role/{role_id}/user/{user_id}` to assign to herself the default admin role for a sensitive project to gain access to that project's raw data.

Clearly, this case \(or more elaborate ways of privilege escalation\) must be prevented. The following simple measure ensure this in aedifion.io's RBAC system: When creating a new role or assigning an existing role,

1. the permission set $$P_R$$ of that role is built,
2. the permission set $$P_U$$of the creating/assigning user is built, and
3. the request is granted if and only if $$P_R \subseteq P_U$$\(and the user has permission to create/assign roles\).

This way a user can never assign or create roles with more permissions than his/her own.

