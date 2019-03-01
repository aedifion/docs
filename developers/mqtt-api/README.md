---
description: How to access the aedifion.io MQTT API
---

# MQTT API

## Overview

MQTT is a lightweight publish/subscribe messaging protocol designed. Originally designed for machine to machine telemetry in low bandwidth environments \(M2M\), MQTT has nowadays become one of the main protocols for \(data collection in\) Internet of Things \(IoT\) deployments \[1\].

MQTT has multiple advantages over HTTP \[3\] and other protocols: 

* Due to its binary encoding and minimal packet overhead, MQTT is 20x faster than HTTP, uses 50x less traffic, and consumes 20% less energy than HTTP according to the direct performance comparison presented in \[4\]. 
* Contrary to HTTP's client/server architecture, MQTT's publish/subscribe pattern decouples data sources from data sinks through a third party, the MQTT broker. Decoupling means that sources never directly talk to sinks which implies that i\) they do not need to know each other, ii\) they do not need to run at the same time, and iii\) they do not require synchronization \[5\]. All this allows easily allows building flexible 1-to-many, many-to-1, and many-to-many data pipelines.
* MQTT has message filtering built-in, i.e., data sinks can subscribe to arbitrary subsets of the data collected from the sources specified through a hierarchical topic theme \[6\].

When you are building an application that streams data into or from the aedifion.io platform, MQTT is probably a better choice than the [HTTP API](../api-documentation/).

**Sources and further resources:**  
\[1\] Introduction to MQTT: [http://www.steves-internet-guide.com/mqtt/](http://www.steves-internet-guide.com/mqtt/)  
\[2\] Introduction to MQTT: [https://www.hivemq.com/blog/mqtt-essentials-part-1-introducing-mqtt/](https://www.hivemq.com/blog/mqtt-essentials-part-1-introducing-mqtt/)  
\[3\] MQTT vs. HTTP: [https://iotdunia.com/mqtt-and-http/](https://iotdunia.com/mqtt-and-http/)  
\[4\] MQTT vs. HTTP: [https://flespi.com/blog/http-vs-mqtt-performance-tests](https://flespi.com/blog/http-vs-mqtt-performance-tests)  
\[5\] MQTT publish/subscribe: [https://www.hivemq.com/blog/mqtt-essentials-part2-publish-subscribe/](https://www.hivemq.com/blog/mqtt-essentials-part2-publish-subscribe/)  
\[6\] MQTT topics: [https://www.hivemq.com/blog/mqtt-essentials-part-5-mqtt-topics-best-practices/](https://www.hivemq.com/blog/mqtt-essentials-part-5-mqtt-topics-best-practices/)

