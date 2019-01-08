---
description: Terminology used throughout this documentation sorted alphabetically.
---

# Glossary

### Automation network

The automation network locally connects different [devices](glossary.md#device) within a [building](glossary.md#building) or [plant](glossary.md#plant). In most cases, the automation network is a dedicated physical wired network such as an Ethernet but can also be wireless or virtual. 

### Building

A physical building, i.e., an office, a factory, a hotel, and so forth. This term is often used synonymously with [project](glossary.md#project) despite being a special case of a project. 

### **Company**

The top-most entity on the aedifion.io platform mapping to a real-world or virtual company. Companies represent strictly separated administration realms on the aedifion.io platform: All resources such as [users](glossary.md#user), [projects](glossary.md#project), [datapoints](glossary.md#datapoint), and so forth belong to exactly one company and are never shared between two companies.

### Component

A physical or logical building automation device. A component is associated to exactly one [project](glossary.md#project) and may have arbitrary many [datapoints](glossary.md#datapoint).

### **Datapoint**

A sensor, actor, or other physical or virtual data source. A datapoint belongs to exactly one [component](glossary.md#component) and one [project](glossary.md#project). It emits [observations](glossary.md#datapoint) that form the [time series](glossary.md#time-series) of this datapoint.

### Datapointkey

Datapointkeys define how [datapoints](glossary.md#datapoint) are named and allow providing alternate names such as translations to different languages or names that adhere to standardized formats such as BUDO. The standard datapoint key is called _aedifion-fully-qualified-datapointname_, comprising the device name, datapoint name, datapoint type and instance id.

### Device

A physical building automation device, e.g., a programmable logic controller, connected to a building automation network, e.g., via BACnet or Modbus.

### Plant

Plant summarizes all locally connected entities of automation hardware, sensors and actors. In many cases, it is synonymously used with building automation system, energy system or building energy system. 

### Measurement

Strictly speaking a special case of an [observation](glossary.md#observation) but mostly used synonymously.

### **Observation**

A concrete value emitted by a certain [datapoint](glossary.md#datapoint). An observation may be actually measured or artificially generated, e.g., through a simulation. An observation is always tagged with a [time stamp](glossary.md#timestamp) and may have further [tags](glossary.md#tag).

### **Project**

A logical entity that corresponds to an administrative subdomain in a company, bundling [components](glossary.md#component), [datapoints](glossary.md#datapoint), and [roles](glossary.md#role). In most cases, a project corresponds to an actual [building](glossary.md#building) or larger [device](glossary.md#device) but may also represent a simulation or a static data set.

### Role

A role is a bundle of permissions that define and limit resource access [company](glossary.md#company)- or [project](glossary.md#project)-wide. A special "admin" role with all permissions is created automatically for each company and project. 

### **Tag**

A short piece of information that can be attached to a [component](glossary.md#component) or [datapoint](glossary.md#datapoint). A tag has a key and a value, e.g., `unit=degreeCelsius` or `writable=False`.

### **Time series**

A list of \([timestamp](glossary.md#time-stamp), [observation](glossary.md#observation)\)-tuples measured for a certain [datapoint](glossary.md#datapoint).

### Time stamp

A certain date and time usually in [RFC3339](https://www.ietf.org/rfc/rfc3339.txt)-format, e.g., `2018-11-11T11:11:11+01:00`.

### **User**

An account on the aedifion.io platform corresponding to a real person. A user always belongs to exactly one [company](glossary.md#company).

\*\*\*\*

