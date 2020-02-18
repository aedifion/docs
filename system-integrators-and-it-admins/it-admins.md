---
description: >-
  This page summarizes how to gather shipping information to order an aedifion
  edge device and how to install it on site.
---

# Edge device

## Overview

The [aedifion edge device](../aedifion.io/gateway.md) is an industrial PC which covers functionality such as being an automation network gateway \(therefore it has two ethernet ports\) and providing computing power for those aedifion services, which are sensitive to internet connection losses.

For plug and play installation of the device, aedifion preconfigures its network interfaces before shipping it to you. Therefore please provide the [required shipping information](it-admins.md#preconfigurations). 

About firewall security: Only outgoing connections from the device to the aedifion servers are needed. Set the firewall settings to these [minimum requirements](it-admins.md#firewall-settings) to enable the aedifion services. 

The[ installation guide](it-admins.md#installation-guide) provides the necessary information on how to wire the aedifion edge device. Basically, first connect the ethernet cables to the right ports, then plug in power.

For any support, please do not hesitate to [contact us](../contact.md)!

## Preconfigurations

Before shipping, aedifion's staff needs to set some preconfigurations to account for seamless and plug-and-play plant integration. This section describes the information we need.

Please provide the name of a contact person and a post address that aedifion can ship the edge device to. 

To preconfigure the edge device, please provide information about the settings of the network\(s\) the device will be connected to. The following flow chart helps you determine that information.

![Flow chart to determine required network information](../.gitbook/assets/grafik%20%282%29.png)

{% hint style="info" %}
**Example**

Imagine the following setup:

* NewCo's headquarters ordered an aedifion edge device.
* The automation network in the headquarters is separated from all other networks, especially those with internet access.
* There is a local network, _LocalNet,_ that has internet access and a DHCP server.
* The automation network, _AutoNet,_ has no DHCP server running.

The following information is needed to preconfigure the aedifion edge device:

* The automation network does not have internet access and the aedifion edge device will be used as a gateway between the _AutoNet_ and _LocalNet_.
* _AutoNet_ does not have DHCP and the settings for a static IP for the edge device are:
  * IP: 192.168.100.100
  * Netmask: 255.255.255.0
  * Gateway: 192.168.100.1
* _LocalNet_ runs a DHCP and the aedifion edge device should obtain its IP address and further settings automatically.
{% endhint %}

## Firewall settings

**Minimum firewall requirements**

* Outgoing connections on the following ports need to be allowed:
  * Port 443 \(HTTPS\): discovery2.aedifion.io, discovery3.aedifion.io
  * Port 22 \(SSH\): ssh2.aedifion.io, ssh3.aedifion.io
  * Port 8884 \(MQTT over TLS\): mqtt.aedifion.io, mqtt2.aedifion.io
  * Port 123 \(NTP to Ubuntu standard timeservers - not necessary if NTP servers are locally available\)
* Outgoing connections can be limited from the edge device to the mentioned server addresses.

{% hint style="info" %}
Operating the edge device only requires outgoing connections! It does not run any kind of service that exposes your network to the Internet. Thus, any incoming connection requests can be blocked by your firewall.
{% endhint %}

[Contact us](../contact.md#support) in case of firewall compliance issues.

## Installation guide

![](../.gitbook/assets/edgedevicebild%20%281%29.png)

To install your aedifion edge device, please execute the installation in the order of the flow chart:

![Installation workflow](../.gitbook/assets/grafik%20%287%29.png)

{% hint style="info" %}
**Trouble shooting**

Restarting the aedifion edge device solved 99 % of the issues which occurred during installation in the past.
{% endhint %}

