---
description: Overview of the aedifion edge device
---

# Edge device

## Introduction

The aedifion edge device is an industrial PC enabling data connection between local devices as well as local data sources and the aedifion.io cloud platform. It uses most industry bus communication standards and is able to automatically ingest data from your plant, building, or district using auto-discovery of all available [datapoints ](https://docs.aedifion.io/docs/glossary#datapoint)and [devices](https://docs.aedifion.io/docs/glossary#device) for easy set-up. 

The edge device connects to a [plant's](../glossary.md#plant) automation network and to the aedifion.io platform. If a plant's automation network has limited connectivity, it uses its second Ethernet adapter to connect to a network offering internet access. 

The edge device operates fully plug-and-play from a customers perspective. Once connected to the internet, the edge device connects to the aedifion.io platform where aedifion's staff take care of its configuration.

![aedifion edge device](../.gitbook/assets/grafik%20%289%29.png)

VGateway

connection

plug and play

edge computing

IT security

The connection for the exchange of local plant data between aedifion.device and cloud is established via the MQTT protocol, which is encrypted using state-of-the-art TLS technology. Furthermore, it is ensured that every message from the local pant reaches the aedifion platform at least once. 

Within the local plant network, the aedifion.device used a variety of technologies to gather obervations of datapoints. These observations are temporarily stored in a buffer memory for several weeks so that data is not lost in the event of a power failure. 

To ensure storage efficiency, the software only records change-of-value events \(CoV\). If required, each interval can also be written free of CoV - regardless of the resolution. 

#### Time synchronization

Time synchronization takes place via the Network Time Protocol via the standard time servers: \(0.ubuntu.pool.ntp.org, 1.ubuntu.pool.ntp.org, 2.ubuntu.pool.ntp.org, 3.ubuntu.pool.ntp.org, ntp.ubuntu.com\). Despite the actual location and the local time, all timestamps are stored in Greenwith Mean Time \(UTC+00\). Eventually, one needs to regrad the given local time whils processing and visualizing the data. 

#### Plug-and-play integration of BACnet-based plants

In the case of a local plant automation using the BACnet protocol, the BACnet software on the aedifion.device carries out network an exploration upon successful access to the local plant network. Using the Whois-paradigm it discovers all available BACnet datapoints and starts collecting observations. 

For network security reasons, it measures the response time of each BACnet component and multipies it with a safty factor of 2. The double response time is the minimal time between two datapoint queries. 

### Network prerequisites

After installation of the aedifion.device according to the installation guide and after firewall-side release of the neccessary ports 22 \(SSH\), 123 \(NTP\), 443 \(HTTPS\) and 8884 \(MQTT\) for the aedifion.io server IPs, aedifion conducts further configuration remotely via reverse SSH. Using this connection, we carry out system updates, adaptations and maintenance without any further locally required action. 

## Installation guide



