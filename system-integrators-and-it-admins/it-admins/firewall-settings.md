---
description: The following firewall settings are required to use the aedifion edge device.
---

# Firewall settings

**Minimum firewall requirements**

* Outgoing connections on the following ports need to be allowed:
  * Port 443 \(HTTPS\)
  * Port 22 \(SSH\)
  * Port 8884 \(MQTT over TLS\)
  * Port 123 \(NTP to Ubuntu standard timeservers - not necessary if NTP servers are locally available\)
* Outgoing connections can be limited to our server IPs.

{% hint style="info" %}
Operating the edge device only requires outgoing connections! It does not run any kind of service that exposes your network to the Internet. Thus, any incoming connection requests can be blocked by your firewall.
{% endhint %}

[Contact us](../../contact.md#support) in case of firewall compliance issues.

