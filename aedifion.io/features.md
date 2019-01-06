---
description: This page introduces important key features of aedifion.io
---

# Features

## Advanced data ingress

Aedifion.io offers plant-, building- and district-wide data acquisition compatible to many bus communication standards, partially with auto discovery of [datapoints ](https://docs.aedifion.io/docs/glossary#datapoint)and [devices](https://docs.aedifion.io/docs/glossary#device). Moreover, it integrates all IP-available data sources, replicates local data bases and connects to existing data servers. In addition, users or devices can stream their data directly into aedifion.io. Of course, historic data can be uploaded via a CSV API.

| Type | Example |
| :--- | :--- |
| Bus communication | BACnet, Modbus, KNX, M-Bus,... |
| Data servers | OPC DA, OPC UA |
| Local data bases | MS SQL, mySQL, pgSQL, ... |
| Rest APIs | MS Exchange, weather data, weather forecasts,... |
| User | MQTT stream, CSV upload |

{% hint style="info" %}
We aspire to integrate &lt;&lt;everything that is IP-based available&gt;&gt;.
{% endhint %}

{% hint style="success" %}
aedifion.io works fully plug-and-play for BACnet system. All it needs is a gateway that operating personnell can easily connect to local networks. Once plugged in, the gateway connects automatically to our servers and we start data integrations.
{% endhint %}

## Metadata acquisition

For each integrated [datapoint ](https://docs.aedifion.io/docs/glossary#datapoint)and for each integrated[ device](https://docs.aedifion.io/docs/glossary#device), aedifion.io is able to acquire comprehensive metadata which originates either from standards like BACnet, from Modbus plat descriptions, from local databases or data servers as OPC. This metadata is automatically processed into aedifion.io's data structuring.  Metadata is added to each datapoint via [tags](https://docs.aedifion.io/docs/glossary#tag). Integrated devices are semantically modelled as [components](https://docs.aedifion.io/docs/glossary#component).  

## Data storage and resolution

Aedifion.io uses databases specialized on [time series](https://docs.aedifion.io/docs/glossary#time-series) as well as additional databases for other data. The time series database uses Change-of-Value \(CoV\) as basic concept for data storage. Preconfigured threshold is 0.

Integrated [devices](https://docs.aedifion.io/docs/glossary#device) define the lowest sample rates of data acquisition and, thus, the data resolution. Typically, aedifion.io reaches 5 seconds for BACnet devices. With recent BACnet devices this can decrease to below 1 second. The same accounts for Modbus devices.

{% hint style="info" %}
Don't you worry! The sample rate can be flexible adjusted during data provision. Same accounts for the interpolation method.
{% endhint %}

## Data provision

aedifion.io offers various ways of data provision.

### Via API

#### HTTP API

The HTTP [API](https://en.wikipedia.org/wiki/Application_programming_interface) functions cover download of timeseries as well as of meta data. Within the time series download, users can flexibly choose the sample rate, the start and end dates as well as the number of returned observation. 

The API can be flexibly integrated into development environments or programming languages. At [https://api.aedifion.io/ui/](https://api.aedifion.io/ui/) users find a web environment for user‐friendly testing and compilation of API interactions. In addition, other retrieval methods can be provided for e.g. MATLAB upon request.

#### Websockets

Websocktes offer the direct retrieval of live data.

### Via web application \(beta\)

A web-based graphical users interface is available under www.aedifion.io. it offers datapoint search, \(multi-\)line plotting, carpet/heatmap plotting, plot and CSV exporting, saving plot views for fast access, high level data management, such as adding favorits, renaming datapoints, and using variable datapointkeys. 

Aedifion will release a version 1.0 in 2019 with better user experience and more functionality.

### Excel plugin

Available open source, the aedifion.io Excel plugin enables to query data from multiple datapoints over multiple projects. Users can directly query raw data or synchronize asynchronous CoV-based observations with adjustable sample rates. Moreover, different interpolation methods, such as zero-order-hold \(step interpolation\) and linear interpolation are offered. In addition, different plots can be auto-generated.

![Screenshot of the aedifion.io Excel integration](../.gitbook/assets/excel-tool.JPG)

{% hint style="info" %}
The aedifion.io Excel plugin is completly open-source, no strings attached. Feel free to use our code as you like and integrate aedifion.io in existing Excel sheets.
{% endhint %}

### Grafana

For each [project](https://docs.aedifion.io/docs/glossary#project), aedifion.io provides an instance of the open-source visualization environment [Grafana](%20https://grafana.com/) under https://dashboards.aedifion.io/_project\_name_/. User admins can build own dashboards using all functionalities of Grafana. Using this powerful, toolkit meaningful dashboards are easy to develop. 

{% hint style="success" %}
aedifion offers training and engineering for Grafana to build your desired dashboard.
{% endhint %}

## Data processing

Aedifion.io directly offers data processing such as resampling via api. Furthermore, two kinds of processes, i.e. stream and batch processes can be set up. Stream provesses cover Use cases as nominal-actual-comparisons as required in the German VDI 6041 "Technical Monitoring" or standard calculation methods, as well as statistic algorithms are covered by stream processes. Batch processes cover more complex calculations. They account for use cases according to ISO 50 001 "Energy Management".

{% hint style="success" %}
aedifion.io handles calculations as required by the German VDI 6041 "Technical Monitoring" or ISO 50 001 "Energy Management" with stream and batch processing, respectively.
{% endhint %}

### Stream processing

A stream process runs a calculation of a free-to-choose mathematical relationship on each new event/observation of a referred datapoint. Stream processes relate to one or more datapoints.

{% hint style="info" %}
Examples for stream process: 

* Heat flow calculation: 
  * $$\dot{Q} = \dot{m} c_p (\vartheta_{out} - \vartheta_{in})$$
* Coefficient of performance: 
  * $$\eta = \frac{\dot{Q{th}}}{P_{el}}$$ 
* System sanity/operation checks 
  * $$\mathrm{actual value} == \mathrm{expected value}$$ 
{% endhint %}

A stream process can be linked to a virtual datapoint or used as an input for alarms and notifications.

### Batch processing

A batch process is not operated continuously - like stream processes - but on a certain trigger. I.e. a pre-set time or up on request. This process runs complex calculation using historical data.

{% hint style="info" %}
Examples for batch processes: 

* Energy Efficiency Ratio \(heat pump\)
* Energy reporting over long periods 
* Control loop analysis
{% endhint %}

### Virtual data points

A virtual datapoint is a datapoint not gathered from a local plant but resulting of a mathematical calculation. Despite, it appears exactly like a physical datapoint in aedifion.io's time series database. In aedifion.io, a virtual datapoint typically originates from a stream process. Of course, all alarming and notification paradigms can be used with physical as well as virtual datapoints.

{% hint style="success" %}
Set up flexible alarms on complex relationships using stream processes and virtual datapoints!
{% endhint %}

## Data management & structuring

Since managing large amounts of datapoints occurs to be a complex task, aedifion.io comprises different methods to manage and structure data. These methods account for the application of various metadata schemes and datapoint designation schemes, such as e.g. [Brick](https://brickschema.org/) or [BUDO](https://github.com/RWTH-EBC/BUDO). 

### Favorites 

Frequently inspected or most important datapoints can be set as a favorite. Within the frontend, users can use favorite datapoints as a filter and thus access them faster. 

### Tagging

A tag is a key-value-pair of metadata attached to a time series with its source indicated and its creation date. All available metadata of a datapoint is stored this way. A source of a tag can be e.g. Cnet, User, Artificial Intelligence, Local Database, etc. The tagging can be used to build up structures as required in [Brick](https://brickschema.org/) or [BUDO](https://github.com/RWTH-EBC/BUDO). 

{% hint style="info" %}
All gathered metadata during metadata acquisition is stored as tags.
{% endhint %}

### Renaming

Aedifion.io supports multiple datapointnames in order to match the needs of different datapoint naming schemes, logical uniqueness, and fulfilling users’ preferences. E.g. a datapoint in aedifion.io can have a unique name originating from its source or field device as identifier, a name according to a scheme, e.g. [BUDO](https://github.com/RWTH-EBC/BUDO), and a name given by the inspecting user at the same time.

## Project management

​Aedifion.io allows for multi plant management. It generates an overarching instance, a so-called company, for each costumer. Multiple projects can exist within each company. Admins manage rights with the so-called rule-based access control.

### Rule-based access control

Aedifion.io offers a fine-grained rule-base rights management. An admin can generate roles that grant access to backend functionality, e.g. adding tags to datapoints, or roles that enable to read or write certain datapoints – or not to. He can allocate roles to users individually, whereby a user can have multiple roles. With this role-based access control customers can configure aedifion.io to meet all their data privacy requirements and those of the [EU GDPR](https://gdpr.eu/).

## Alarming and notifications

Users of aedifion.io can use various alarming schemes to deploy them on datapoints or virtual datapoints. E.g. threshold-alarms and throughput alarms can be set up. In addition, users can chose between various alarming channels, such as e.g. via Email or via chatbots in e.g. Telegram. Even Amazon's Alexa is possible up on request. This accounts for flexible, user-centric and innovative alarming functionalities.

## Integrations

Aedifion.io integrates various external applications and platforms. It uses Telegram to communicate alarms an integration. It can receive Amazon's Alexa's commands and communicate them to field devices. In turn, Alexa uses aedifion.io to return user's current thermal comfort and indoor air quality. Users of aedifion.io use MS Excel, Mathwork's Matlab and Simulink as well as many other work benches as tools for further data analytics and even plant controls. Companies can use their single-sign on system for log in to aedifion.io. E.g. for a supervisory heating and cooling control or performance analytics of meeting rooms, aedifion.io uses Microsofts Exchange resource data. 

In addition, aedifion.io has pre-configured interfaces to state-of-the-art IoT-platforms, such as e.g. offered by Cumulocitiy, to seamlessly integrate its services.

### Weather and weather forecast data

Further, aedifion.io integrates local weather data and forecast data for each project, depending on its GPS coordinates.

## Controls

Aedifion.io provides basic control functions whenever a datapoint is generally controllable in the field. This covers simple set point writing as well as manipulating local control loops or even overruling local system output. Further, aedifion.io has a decent scheduling functionality in order to robustly execute control sequences. This enables for a simple way of deploying cloud-based controls and new ways of system interaction, such as Alexa or chatbots. In extreme cases, local controls hardware remains just as a in-out-device, whereas logics are operated in the cloud.

{% hint style="danger" %}
Safety of cloud-based controls is important. Aedifion.io offers various, locally executed, fail-safe operations triggered by certain events, such as a connection loss.​
{% endhint %}

### Controls runtime environment

In the first step, a user's control algorithm operates within the user's local setup or cloud environment. To increase scalability and robustness, aedifion.io offers the opportunity to operate control algorithms within its control runtime environment. Users can provide their algorithms and API requirements. Control algorithms are then deployed within aedifion.io and APIs are individually engineered.

### Cloud, edge, and air-gapped

Control algorithms can be operated at different ways. A cloud-operated algorithm uses the internet to communicate its control decisions or manipulated variables. An edge-operated algorithm is executed on the aedifion.io gateway, the so-called edge-device, locally within the customers control system. The edge-operated algorithms use the internet and the cloud-platform to receive commands and parameters. To the user, it offers the same API-functionality as the cloud-operated on. An air-gapped deployment is a more classical set up, quite like a local programmable logic controller, but with a more advanced control runtime, offering the possibility to execute more complex controls, including optimizations and simulations.

{% hint style="success" %}
With aedifion.io, cloud, edge and air-gapped deployments are possible. We thrive to solve control challenges.
{% endhint %}

## Comprehensive APIs

Users can interact with all features via our documentented REST APIs. This makes the platform's functionalities flexibly integrateble in users' own scripts, programms or applications. 

### Customized APIs

aedifion offers customized backend features for special purposes of users. Such backend features come with customized APIs. 

## Authentification mechanisms

Aedifion.io supports various authentication, e.g. OpenID, GoolgeAuth, as well as various API authorization standards, e.g. oAuth, xAuth, XACML, up on request. Aedifion customizes aedifion.io for companies using a single sign-on system in order to get a seamless user experience.

## Data privacy

With its [rule-based access control](https://docs.aedifion.io/docs/aedifion.io/features#rule-based-access-control), admins can adjust access rights per user to meet all data privacy requirements of their organization. In general, aedifion.io meets all EU GDPR requirements with its legal framework.

## Legal framework

Aedifion.io's legal framework consists of a licence agreement \(Nutzungsvertrag\) between the customer and aedifion with attachments. I.e. the aedifion Terms and Conditions \(AGBs\), a contract for the commissiond data processing \(Auftragsdatenverarbeitung, ADV\), and technical and organizational measures for data security \(technisch-organisatorische Maßnahmen, TOMs\).​

## Availability and quality assurance

Aedifion continuously supervises aedifion.io's system status using measurements points along its whole pipeline. The alarming on measurement points is combined with engineering domain knowledge. A computer emergency and response team is available 24/7 to guarantee for highest system availabilities. The aedifion.io system is mirrored, thus, there are two deployments working in parallel. Moreover, daily backups guarantee for highest data protection in case of unforeseen events.



