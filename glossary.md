---
description: Terminology used throughout this documentation sorted alphabetically.
---

# Glossary

### Analysis function

An analysis function is a generic function to determine a specific analysis output. An analysis function takes arbitrary many [pins ](glossary.md#pin)of a component data model and variable time spans as inputs. An analysis function can be designed for a specific component or be applicable on several components.

### Automation network

The automation network locally connects different [devices](glossary.md#device) within a [building](glossary.md#building) or [plant](glossary.md#plant). In most cases, the automation network is a dedicated physical wired network such as an Ethernet but can also be wireless or virtual. 

### Building

A physical building, i.e., an office, a factory, a hotel, and so forth. This term is often used synonymously with [project](glossary.md#project) despite being a special case of a project. 

### **Company**

The top-most entity on the aedifion.io platform mapping to a real-world or virtual company. Companies represent strictly separated administration realms on the aedifion.io platform: All resources such as [users](glossary.md#user), [projects](glossary.md#project), [datapoints](glossary.md#datapoint), and so forth belong to exactly one company and are never shared between two companies.

### Component

A component is a physical or virtual object or system in a [building](glossary.md#building), [plant](glossary.md#plant), district and so forth such as an [automation device](glossary.md#device), valve, heat pump, room etc. A component may be an aggregation of several components. In general, a component is part of an automation or monitoring system and therefore has varying numbers of [datapoints ](glossary.md#datapoint)measuring, describing or influencing the state of the component.

### Component data model

A component data model is a generic data model of a [component](glossary.md#component). It may have arbitrary many [pins ](glossary.md#pin)and [tags ](glossary.md#tag)for meta-data.

### **Datapoint**

A sensor, actor, or other physical or virtual data source. A datapoint belongs to exactly one [project](glossary.md#project). It emits [observations](glossary.md#datapoint) that form the [time series](glossary.md#time-series) of this datapoint.

### Datapoint key

Datapointkeys define how [datapoints](glossary.md#datapoint) are named and allow providing alternate names such as translations to different languages or names that adhere to standardized formats such as BUDO. The standard datapoint key is called _aedifion-fully-qualified-datapointname_, comprising the device name, datapoint name, datapoint type and instance id.

### Device

A physical building automation device, e.g., a programmable logic controller, connected to a building automation network, e.g., via BACnet or Modbus.

### Instanced component

An instanced component is a [component data model](glossary.md#component-data-model) instanced for a specific [project](glossary.md#project). An instanced component is associated to exactly one project, the [pins ](glossary.md#pin)of the component data model it is instanced from may be mapped to the [datapoints ](glossary.md#datapoint)of the project. Meta-data [tags ](glossary.md#tag)may be added to an instanced component to further individualize it.

### Mapping

Mapping is the process of individualizing an [instanced component](glossary.md#instanced-component). It includes the linking of a [datapoint ](glossary.md#datapoint)and thus its [time series](glossary.md#time-series) of a [project ](glossary.md#project)with a [pin ](glossary.md#pin)and parametrizing meta-data [tags](glossary.md#tag).

### Measurement

Strictly speaking a special case of an [observation](glossary.md#observation) but mostly used synonymously.

### **Observation**

A concrete value emitted by a certain [datapoint](glossary.md#datapoint). An observation may be actually measured or artificially generated, e.g., through a simulation. An observation is always tagged with a [time stamp](glossary.md#timestamp) and may have further [tags](glossary.md#tag).

### Pin

A pin is a generic placeholder for a [datapoint ](glossary.md#datapoint)within a [component data model](glossary.md#component-data-model). A pin is used to [map ](glossary.md#mapping)a [datapoint ](glossary.md#datapoint)and its [time series](glossary.md#time-series) to an [instanced component ](glossary.md#instanced-component)within a specific [project](glossary.md#project).

### Plant

Plant summarizes all locally connected entities of automation hardware, sensors and actors. In many cases, it is synonymously used with building automation system, energy system or building energy system. 

### **Project**

A logical entity that corresponds to an administrative subdomain in a company, bundling [instanced components](glossary.md#instanced-component), [datapoints](glossary.md#datapoint), and [roles](glossary.md#role). In most cases, a project corresponds to an actual [building](glossary.md#building) or larger [device](glossary.md#device) but may also represent a simulation or a static data set.

### Role

A role is a bundle of permissions that define and limit resource access [company](glossary.md#company)- or [project](glossary.md#project)-wide. A special "admin" role with all permissions is created automatically for each company and project. 

### **Tag**

A short piece of information that can be attached to an [ instanced component ](glossary.md#instanced-component)or [datapoint](glossary.md#datapoint). A tag has a key and a value, e.g., `unit=degreeCelsius` or `writable=False`.

### **Time series**

A list of \([time stamp](glossary.md#time-stamp), [observation](glossary.md#observation)\)-tuples measured for a certain [datapoint](glossary.md#datapoint).

### Time stamp

A certain date and time usually in [RFC3339](https://www.ietf.org/rfc/rfc3339.txt)-format, e.g., `2018-11-11T11:11:11+01:00`.

### **User**

An account on the aedifion.io platform corresponding to a real person. A user always belongs to exactly one [company](glossary.md#company).

\*\*\*\*

