# Spot Protocol Version 2

This document describes Version 2 of the Spot Specification. Support for Version 2 is indicated by the `Protocol` and `Version` field in the [message header](MessageHeaders.md).

# Changeset
The following table describes the changeset between Version 2 and Version 1 of the Spot standard:

| Change | Description |
|---|---|
| The placeId field for all messages has changed from `uint_64` to `uuid`. |
| the FromWayId field in ApproachingLastMile has changed from required to optional to account for trucks requesting a last mile assignment while inside the destination area. |
| A new OccupyPlaceV2 message has been created in order to simplify communications with the AHT and remove the need for cache management. |
| Removed Cache Management as this information is now fully enclosed in the OccupyPlaceV2 message. |
| Removed all `VehicleId` attribute from messages, and to use top-level message headers `EquipmentId` |
| Added `RequestId` to all request-reponse type messages to correlate and link messages |
 


# Message definition
Find below the Specification for the Version 2 protocol for the spot service of Open-Autonomy
- [Class](#class)
- [Enumeration](#enumeration)
- [Spot Message](#spot-service)
- [Autonomous Truck Message](#Autonomous-truck-messaging)
- [FMS Message](#fms-messaging)

> NOTE: All JSON examples in this documentation are formatted for your convenience and viewing pleasure.  It is not expected that messages exchanged on the wire will contain new lines, tabs & spaces so a human can read it. So the indentataion can't be programmatically relied upon (like that dumb yaml), the formatting is just there to delight your brain. <br> The protocol does **not preclude** the messages to be beautified prettified on the wire if that's what you like.
---

<br>

# Class
### [Place](class_Place.md#place)
### [Queue](class_Place.md#queue)
### [Spot](class_Place.md#spot)
---

<br>

# Enumeration
### [PlaceIO](enum_Place.md#placeio-enumeration)
### [Task](enum_Place.md#task-enumeration)
### [Origin](enum_Place.md#origin-enumeration)
### [PlaceState](enum_Place.md#placestate-enumeration)
### [SpotState](enum_Place.md#spotstate-enumeration)
### [QueueState](enum_Place.md#queuestate-enumeration)
---

<br>

# Spot Service
### [ServiceIdentification](ServiceIdentification.md)
### [MessageHeaders](MessageHeaders.md)
# Last Mile Dispatching
### [OccupyPlaceV2](OccupyPlaceV2.md)
### [OccupyPlaceResponseV1](OccupyPlaceResponseV1.md)
### [TaskStartV2](TaskStartV2.md)
### [TaskStartResponseV1](TaskStartResponseV1.md)
### [LeavePlaceV2](LeavePlaceV2.md)
### [LeavePlaceResponseV1](LeavePlaceResponseV1.md)
### [VehicleDismissedV2](VehicleDismissedV2.md)
### [VehicleDismissedResponseV1](VehicleDismissedResponseV1.md)
---

<br>

# Autonomous truck messaging
### [ApproachingLastMileV2](ApproachingLastMileV2.md)
### [LastMileDispatchingExceptionV2](LastMileDispatchingExceptionV2.md)
### [LastMileDispatchingQuitV2](LastMileDispatchingQuitV2.md)
### [LeftPlaceV2](LeftPlaceV2.md)
### [OccupyingPlaceV2](OccupyingPlaceV2.md)

