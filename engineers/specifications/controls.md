---
description: Detailed specification of aedifion.controls' control algorithms
---

# Controls

### MaxPhi

This function optimizes the energy-efficiency and therefore the cost-efficiency of your Air Handling Unit \(AHU\) which contains Variable Air Volume \(VAV\)-Boxes. 

Standard AHU are controlled by a constant pressure- or volumeflow setpoint. Depending on this given plant sate the subsequent VAV-Boxes for e.g. room-automation reduce this volumeflow to the respective room dependent setpoint. Because the systems pressure- or volumeflow setpoint is selected for a full-load operation mode, in a partial-load operation mode, where the VAV-Boxes are partially closed, the AHU is not working efficiently. 

Here comes this higher-level control algorithm to place. The inputs are the continuously retrieved feedback signals of the VAV-Boxes in percent, for both the supply air \(SUP\) system and the extract air \(ETA\) system. With the maxima of those feedbacks, the setpoints of the SUP and ETA air ventilators are being calculated.

The same algorithms is working for hydraulic systems as well, if you think about Underfloor-Heating Systems with a central circulating pump.

More control algorithms will be described here. If you wish to implement your own algorithm or want us to implement it, feel free to [contact us](../../contact.md).

.

