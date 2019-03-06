---
description: Overview of the aedifion edge device.
---

# Edge device

## Introduction

The aedifion edge device is an industrial PC enabling data connection between local devices as well as local data sources and the aedifion.io cloud platform. It uses most industry bus communication standards and is able to automatically ingest data from your plant, building, or district using auto-discovery of all available [datapoints ](https://docs.aedifion.io/docs/glossary#datapoint)and [devices](https://docs.aedifion.io/docs/glossary#device) for easy set-up. 

The edge device connects to a [plant's](../glossary.md#plant) automation network and to the aedifion.io platform. If a plant's automation network has limited connectivity, it uses its second Ethernet adapter to connect to a network offering internet access. 

The edge device operates fully plug-and-play from a customers perspective. Once connected to the internet, the edge device connects to the aedifion.io platform where aedifion's staff take care of its configuration.

![aedifion edge device](../.gitbook/assets/grafik%20%289%29.png)

The edge device offers very high security since its communication is unidirectional only. Clients do not need to open up their firewall for incoming connections. For outgoing connections only a few standard ports are necessary, such as 22 \(SSH\), 123 \(NTP\), 443 \(HTTPS\) and 8884 \(MQTT\).  In special cases, this can even be limited to HTTPS only. Installation guide.

{% hint style="success" %}
The edge device offers multi-core computation power accounting for divers edge services, such as cloud-control fallback functions, operation of local control algorithms, data buffering and so forth.
{% endhint %}



On the next subpage, we introduce available APIs of aedifion.io.

