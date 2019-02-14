---
description: >-
  Before shipping, aedifion's staff need to set some preconfigurations to
  account for seamless and plug-and-play plant integration. This section
  describes the information we need.
---

# Preconfigurations

Please provide the name of a contact person and a post address that aedifion can ship the edge device to. 

To pre-configure the edge device, please provide information about the settings of the network\(s\) the device will be connected to. The following flow chart helps you determine that information.

![Flow chart to determine required network information](../../.gitbook/assets/grafik%20%282%29.png)

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

