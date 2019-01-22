---
description: This section describes the working principle of the aedifion.device.
---

# Edge device

## Functional overview

The aedifion.device is an industrial PC enabling data connection between local data sources and the aedifion.io cloud plattform. It is a commercially available industrial PC which has been designed, assembled and approved according to German standards. It is able to connect to IP-based sensors, actors and control devices communicating via supported protocols. This so-called gateway component enables high-performance, manufacturer-independent and secure data recording, intermediate storage and interaction. 

The aedifion.device has two built-in network adapters, enabling the separation of the a local plant or building automation network and the internet, if needed. In the case of no internet connectivity within the local plant's operation network, one adapter establishes the connection to the local plant and the other connects to any given network with access to the internet.

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



