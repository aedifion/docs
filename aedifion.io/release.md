---
description: aedifion .io versioning
---

# Release

## aedifion.io 1.0.0 - 2020-03-30

### Edge device hardware

<table>
  <thead>
    <tr>
      <th style="text-align:left">Topic</th>
      <th style="text-align:left">Comment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Chassis</b>
      </td>
      <td style="text-align:left">Aluminum special profile</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Dimensions (WxHxD)</b>
      </td>
      <td style="text-align:left">150x52,27x105 mm</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Weight</b>
      </td>
      <td style="text-align:left">0,86 kg</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Temperatures</b>
      </td>
      <td style="text-align:left">
        <p>Operating temperature: PowerBox 100x: 0 &#xB0;C to +45&#xB0;C</p>
        <p>Storage temperature: -40&#xB0;C to +80&#xB0;C</p>
        <p>Relative humidity: 10% to 95%, non condensing</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Processor</b>
      </td>
      <td style="text-align:left">Intel&#xAE; Celeron&#xAE; J1900 Quad Core 2,0 GHz</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Network</b>
      </td>
      <td style="text-align:left">2x Gigabit LAN RJ-45</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>HDD</b>
      </td>
      <td style="text-align:left">64 GB</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>RAM</b>
      </td>
      <td style="text-align:left">4 GB</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Display port</b>
      </td>
      <td style="text-align:left">yes</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Mounting</b>
      </td>
      <td style="text-align:left">Wall mount, opt. side board, VESA or DIN rail mounting kits</td>
    </tr>
  </tbody>
</table>### Multi logger software

* Plug-and-play commissioning from the customer's perspective 
  * After transmitting the local IP configuration 
  * Local installation by customer 
  * Setup support via manual and online documentation 
* Bridging function for non-internet-accessible IP networks
  * Routing via second network card 
* Communication 
  * Unidirectional communication from customer network to platforms 
  * Exclusive use of standard ports of the protocols HTTPS, MQTT, SSH and NTP 
  * No firewall opening required for incoming communication 
* Vendor-neutral, local data-ingress of IP-based networks
  * BACnet \(including plug-and-play BACnet auto-discovery\) 
  * Modbus \(after transmission of corresponding register lists\) 
  * OPC UA / DA \(after transmission of corresponding access data\)
  * KNX 
  * Locally hosted APIs of various vendors
* Dynamic temporal resolution of BACnet data ingress 
  * Different types supported, e.g. polling, subscription etc.
  * Depending on the response times of the BACnet devices, unrestricted functionality is ensured with optimum data resolution 
  * Adjustable data resolution in case of ingress from further sources \(limited by sources' communication capacities\)  
* Data buffering in local time series databases \(up to multiple months\)  
* Data Egress via MQTT
  * To the aedifion.io platform
  * other platforms
* Data supplementation from local intermediate storage in the event of a loss of connectivity to the aedifion.io platform 
* Compatibility with the extensive writing functionality of aedifion.io 
  * Support of the openly documented Simple Writing Operation Protocol \(SWOP\) according to aedifion online [documentation](https://docs.aedifion.io/docs/developers/writing-protocol) 
  * Storage of received schedules and continued operation even in case of a loss of connectivity to the aedifion.io platform 
* Mounting options 
  * Control cabinet / top hat rail mounting 
  * Stand-Alone 
* Power supply 
  * Power supply unit 230VAC included 
  * Alternatively possible via DC 
* Security 
  * Complete end-to-end encryption of network traffic to the aedifion.io platform 
  * Specified minimum complexity of password protection 
  * Remote maintenance and system maintenance by contractor 
  * Updates by contractor
  * Completely unidirectional communication, no responsiveness from outside the firewall \(commissioning and maintenance connectivity is established via reverse SSH by Edge-Device\) 
* Loaned and fully managed operation by aedifion
* Edge device management via RESTful APIs 
  * Customer access possible on request 
  * Status, restart, publisher management and more available... 
* aedifion edge device frontend
  * Status of device and automation protocol, datapoint scanning, etc.
* Hardware specifications:

### Backend

* Data ingress 
  * Connection of aedifion Edge Devices if commissioned 
  * MQTT-API for customer stream-ingress 
  * HTTP API for CSV upload of historical data 
  * Integration of webbased APIs of other manufacturers 
* Data storage 
  * Setup and use of high-performance time series databases 
  * Redundant storage 
  * At least weekly and full backups with external backup 
* Data Egress 
  * MQTT API for streaming egress
  * Restful HTTP API for accessing historical data
    * Raw data retrieval
    * Adjustable time periods, possibility of batchwise retrieval
    * Free resampling option for reducing the amount of data over longer periods of time with linear interpolation or step interpolation \(zero order hold\) 
  * Excel plug-in for \(multi-\)data retrieval including interpolation options and direct plotting 
  * Matlab code examples for data retrieval
  * Python client for data retrieval
  * Curl code examples for data retrieval
* application programming interface \(Restful HTTP API\) 
  * Includes all platform functionality listed here 
  * Comprehensive and publicly documented 
  * Selected guides and tutorials 
* User and role management 
  * Possibility to create multiple users 
  * Individual user access 
  * Role-based rights management and role aggregation 
  * Possibility of assigning roles to users 
* Data management 
  * Tags: Save/deposit additional information \(meta data\) on datapoints or devices
  * Favorites: Possibility to mark favorite data points 
  * Renaming: Possibility to create multiple data point keys/plant identification keys. Use of a uniform addressing standards possible. 
* Write functionality
  * Location, terminal and platform independent
  * Openly documented and available write APIs
  * Writing of Setpoints
  * Simulating of BACnet data points of the type "Analog Input" and "Digital Input
  * Storage of schedules and management of stored schedules
  * Setting up writeable data points in aedifion.io
  * Defining the desired write limits \(upper and lower limits\) for each data point
* Alarming 
  * Possibility to realize alarms based on different alarm concepts on data points 
  * Especially threshold and throughput alarms 
* IT security and privacy
  * Server environments according to current valid certificates. 
  * Any access \(physical, data-driven\) at any time in compliance with the current EU general data protection regulation 
* Comprehensive online documentation 
* Virtual datapoints ****
  * Implementation and ongoing calculation of virtual data points
  * Output of results in time series
* Timeseries classification 
  * Supplementing time series with metadata on artificial intelligence \(AI\). 
  * Use of neural networks
  * Use of a predefined class system
  * Deposit of AI meta data as data point tags
  * Periodic, at least monthly execution

### Frontend

* Human machine interface \(HMI\) between user and API
* Application of DIN EN ISO 9241-11
* Flat UX design, tablet compatibility
* Runs on common browsers \(Chrome, Firefox, Safari, Edge\)
* Search and filter functions for any data records
* Line, multi-line and heatmap visualization
* User and project management
* Possibility of Setting Setpoints
* Possibility of Setting Schedules
* Analytics view\*

\*available if .analytics is licensed

### Dashboard 

* Extensive visualization options
* Individually designable and flexible
* Application of histograms and heat maps, among others
* Freely integrable and expandable
* Sharing created dashboards
* Deposit of slideshows \(e.g. for promotion displays\)
* Self-administration

### Alarm dashboard 

* High-level alarm management
* Alarm classification, assignment, tracking, prioritization and forwarding capabilities
* Customer self-administration

#### 



