# Spot Protocol Version 2

This document describes Version 2 of the Spot Specification. Support for Version 2 is indicated by the `ProtocolVersion` field in the message header.

# Changeset
The following table describes the changeset between Version 2 and Version 1 of the Spot standard:

| Change | Description |
|---|---|
| The placeId field for all messages has changed from `uint_64` to `uuid`. |
| the FromWayId field in ApproachingLastMile has changed from required to optional to account for trucks requesting a last mile assignment while inside the destination area. |
| A new OccupyPlaceV2 message has been created that simplifies communications with the AHT |
| Place Cache | Removed PlaceCreatedV1, PlaceDeletedV1, PlaceUpdatedV1, PlaceSummaryV1, PlaceAllV1 |
 


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
### [Place](class_PlaceV1.md#place)
### [Spot](class_PlaceV1.md#spotv1)
### [Queue Primary](class_PlaceV1.md#primaryqueuespotv1)
### [Queue Stage](class_PlaceV1.md#queuestagespotv1)
### [Service Chain](class_PlaceV1.md#servicechain)
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
# Last Mile Dispatching
### [OccupyPlaceV1](OccupyPlaceV1.md)
### [TaskStartV1](TaskStartV1.md)
### [LeavePlaceV1](LeavePlaceV1.md)
### [VehicleDismissedV1](VehicleDismissedV1.md)
# Cache Management
### [PlaceSummaryV1](PlaceSummaryV1.md)
### [PlaceAllV1](PlaceAllV1.md)
### [PlaceCreatedV1](PlaceCreatedV1.md)
### [PlaceDeletedV1](PlaceDeletedV1.md)
### [PlaceUpdatedV1](PlaceUpdatedV1.md)
---

<br>

# Autonomous truck messaging
### [ApproachingLastMileV1](ApproachingLastMileV1.md)
### [LastMileDispatchingExceptionV1](LastMileDispatchingExceptionV1.md)
### [LastMileDispatchingQuitV1](LastMileDispatchingQuitV1.md)
### [LeftPlaceV1](LeftPlaceV1.md)
### [OccupyingPlaceV1](OccupyingPlaceV1.md)
### [PlaceCommandV1](PlaceCommandV1.md)
### [SetDynamicPathIdV1](SetDynamicPathIDV1.md)
---

<br>

# FMS messaging
### SetPlaceStateV1
### MoveQueueV1
### ResetUtilizationV1
### SetMaxUtilizationV1
### SetUtilizationV1
