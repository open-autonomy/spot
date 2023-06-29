# Message definition
Find below the Specification for the Version 1 protocol for the spot service of Open-Autonomy
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
