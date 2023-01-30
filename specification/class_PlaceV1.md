# Objects included in messages
The spot messages include a few objects that are embedded as complex attributes.  The classes of these objects are defined here.  They are:
- [`Place`](#place)
- [`Spot`](#spotv1)
- [`Service Chain`](#servicechain)
- [`Primary Queue`](#primaryqueuespotv1)
- [`Staging Queue`](#queuestagespotv1)


---

# Place
A `Place` is a virtual class containing all the shared attributes of these children classes:
- spot
- primary queue
- staging queue

You will never encounter a place object in the messages, but other concrete classes will reference this definition.


## Place attributes
|key |value |format | Description|
|---|-----|-----|---|
| `"TimeCreation"` | timestamp | ISO 8601 | Timestamp when this place (spot/queue) was created. A UTC Time (+0 Zulu) with format \<YYYY-MM-DD\>T <HH:mm:ss[.sss]>Z.  Milliseconds are optional and should only be added if meaningful.   |
|`"PlaceId"`| PlaceId | uint_64| Unique ID for the entire Spot Service that identifies a Place (Spot, primary queue, staging queue, …)  All places share the same ID space, this is to minimize the risk of bugs due to managing different ID spaces for the different child classes. further 2^64 is wide enough to accommodate all the places.|
|`"Latitude"`| &#177;90 degree |float| position on earth in LLE using WGS84.  NOTE: LLE can change if the place is re-positioned by the operator.  The float will typically require 7 decimals to reach centimeter level of resolution. North of equator is positive, while a negative means south of equator.|
|`"Longitude"`|&#177;180 degree|float|The longitude of the spot on hearth.  Requires enough decimals to achieve the accuracy sought for the application (typically 6 to 7).  East of Greenwich is positive, while a negative means West of Greenwich.|
|`"Elevation"`|meter above see level|float|The distance in meter above (positive) or (negative) below the theoretical sea level at that coordinate.  This coordinate is not typically used for vehicles that don’t control their elevation (fly).  Typically 2 decimals.|
|`"Heading"`| 0-359 degrees|integer|Compass heading in degrees of the front of the spot.  Where the vehicle's front should point when spotted|
|`"PlaceIO"`|[`PlaceIO`](enum_Place.md#placeio-enumeration)|enum| How to Ingress & Egress that place See [`PlaceIO`](enum_Place.md#placeio-enumeration). |
|`"Origin"`|[`Origin`](enum_Place.md#origin-enumeration)|enum|How to interpret the point served by the spot service in reference to the truck’s origin frame of reference|
|`"DynamicPathId"`| PathId|integer<br>`nullable`|Optionally, but expected, to be set by the AHS when a dynamic path that leads to this place is computed in the free space of the area. Semantic `null`="Not Set" and is the initial state, 0=”Unreachable”  AHS is not able to compute a path,  >0 = A path ID and means that AHS was able to find a valid path computed in the free space of the area.  The Path is retrievable via the Path service (See Path service specification)|
|`"ServiceMaxUtilization"`|Count|integer<br>`nullable`| The number of times this place can be used before it needs to be “reset” by an external event. Can be set to INFINITE via “null”.  Null means that there is no Max Utilization set.  Dispatching can use this to know how many future trucks can be sent there. Useful for loading and dumping, especially paddock (set=1).  Setting this value (writing), even to the same value, will reset the Service Count below.  NOTE: Writing to this value is not the proper way to reset the counter, there is an API to reset it back to zero.|
|`"PlaceState"`|[`PlaceState`](enum_Place.md#placestate-enumeration)|enum| {Closed, Open, Pending, PendingDelete}  This state is User controlled through user requests. Pending = Pending close and waiting that the place is freed before going into closed.  PendingDelete: same but for delete.|
|`"ChangeSequence"`| Counter |uint_64|a sequence number that is incremented each time this place (and derivations) changes |
|`"ServicingVehicleGUID"`| VehicleId |UUID|Equipment that has reserved or is there at the moment. Note: for queues the spot server only places the first vehicle here. null= No vehicle utilizing this resource|
|`"ServiceCount"` |Counter |integer<br>`nullable`|The number of times this spot has gone in the Full state.  It is used in conjunction with the ServiceMaxUtilization configuration.  Initial value is 0 and it increments each time the spot reached Full state.|

---

<br><br>

# SpotV1

`SpotV1` is a position in the mine where a miner wants a vehicle to go to and do something.  It is a child class of `Place`. It is used in messaging between the Spot service and its clients.


## object attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|include <br>[`"Place"`](#place-attributes)|N/A|N/A| Reference [`"Place"`](#place-attributes)|
|`"Action"`| [`Task`](enum_Place.md#task-enumeration) |enum| What the truck is expected to do once it has spotted.|
|`"OwnerWayId"`| WayId |integer| An Area or Path WayId defined in the Map service where this spot is contained.  Spots are typically created in real time in an open area.  But there are certain exceptions where they can be staticky defined on a road. |
|`"OwnerGUID"`| VehicleId|UUID<br>`nullable`| If this spot was created by a piece of equipment, then this field must be set to the equipment GUID.  If it’s staticaly defined through a surveyed import, then it must be set to null. |
|`"ServiceChain"`|see [`ServiceChain`](#servicechainv1)| ArrayOf `[ServiceChain]`|All the defined ways to reach this spot.  A spot can’t exist without at least one chain.  A chain must have a minimum of one primary queue to exist.|
|`"SpotState"`|[`SpotState`](enum_Place.md#spotstate-enumeration)| enum| The state of the spot.|


## Example SpotV1
> NOTE that all examples are formatted for your convenience and viewing pleasure.  It is not expected that the messages will contain all these new lines, tabs & spaces. The indentataion can't be programatically relied upon, it's just there to delight your brain.

```json
  "SpotV1":
  {
    "TimeCreation":"2023-01-23T09:30:10.43.512Z",
    "PlaceId":731854,
    "Latitude":49.176854,
    "Longitude":-123.0718,
    "Elevation":22.69,
    "Heading":68,
    "PlaceIO":"BackIn",
    "Origin":"Load",
    "DynamicPathId":8456,
    "ServiceMaxUtilization":null,
    "PlaceState": "Closed",
    "ChangeSequence": 3452,
    "ServicingVehicleGUID": null,
    "ServiceCount":0,

    "SpotState":"Available",
    "Action":"Load",
    "OwnerWayId":745932,
    "OwnerGUID":"2248d535-3daf-4a86-b1e1-4951a22beec6",
    "ServiceChain":
    [
      {
        "LinkWayId": 945723,
        "LinkQueuePrimary": 30354,
        "LinkQueueStage":[ 30476 ]
      },
      {
        "LinkWayId": 945361,
        "LinkQueuePrimary": 30352,
        "LinkQueueStage":[ 30475,30477]
      }	   
    ]
  }
```

---

<br><br>

# ServiceChain
A service chain documents all the different ways, through places, a vehicle can navigate to reach this spot.  In a large area or a complex situation like near the crusher, there might be multiple queues that can feed a single spot.  But for most situation, there will only be a single primary queue to reach a spot, that simple !

**By Design**, this model
- Forces that 2 primary queues can’t be linked to each other, because a service chain can only hold one Primary queue.
- Queues can be created before a spot exists and before a free open space area exists.

**Spot Service Behavior Conventions**
- The spot server should automatically create a Primary queue near the end of each WayId that are not connected to another WayId.  Normally they are ingress wayId that enters an autonomous open area.
- There should only be a single Primary queue per ingress WayId.  If it is required to reach a destination through multiple paths then use Staging queues to do so.
- Only a staging queue can merge 2 or more **queues** connected at its ingress.
- Only a staging queue can split its egress to conenct to 2 or more **queues**.
- Both queue types can split to connect to many spots.

## Object attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|``"LinkWayId"``| WayId| integer| The ID of last segment of the haul road to that area|
|``"LinkQueuePrimary"``| PlaceId| uint_64| The ID of a primary queue|
|``"LinkQueueStage"``| PlaceId|ArrayOf `[uint_64]`|Ordered staged queues required to traverse to reach the spot.  This array will typically be empty ;  We should place an upper bound of 8.  Just because we like to bound things.|

## Example ServiceChain
In the example below, a spot can be reached via 2 roads (945723 & 945361), the second road access also required the truck to go through 2 staging queues.

```json
"ServiceChain":
  [
  	{
  	"LinkWayId": 945723,
  	"LinkQueuePrimary": 30354,
  	"LinkQueueStage":[]
  	},
  	{
  	"LinkWayId": 945361,
  	"LinkQueuePrimary": 30352,
  	"LinkQueueStage": [30475,30477]
  	}
  ]

```

---

<br><br>

# PrimaryQueueSpotV1

The `QueuePrimarySpotV1` is always required to reach a spot and it’s the first transit spot after the static road ends and before reaching the final spot position.  This is where trucks will typically queue and  wait for the loading equipment to be available to load the trucks.



## Object attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|include <br>[`"Place"`](#place-attributes)|N/A|N/A| Reference [`"Place"`](#place-attributes)|
|`"Capacity"`| Count|u_int|The maximum number of trucks that the queue is designed for.  If more trucks than capacity queues there, the excess trucks will not be considered as queuing by the spot service (but the FMS will for production reporting).  This number is never big;  typically in the single digit.|
|`"CapacityUsed"`| Truck Count |u_int|The realtime number of trucks in the queue at the moment|
|`"QueueState"`|[`QueueState`](enum_Place.md#queuestate-enumeration)|enum|States if the queue is full or not.|
|`"LinkWayId"`| WayId |integer|The specific road segment where the primary queue is connected to. The road segment is identified by its WayId as defined in the Map service.|

## Example PrimaryQueueSpotV1
```json
"PrimaryQueueSpotV1":
{
    "TimeCreation": "2021-01-20T09:30:10.43.511Z",
    "PlaceId" : 731856,
    "PlaceState": "Opened",
    "Latitude" : 49.1768531,
    "Longitude" : -123.071610,
    "Elevation" : 22.69,
    "Heading" : 74,
    "PlaceIO": "PullThrough",
    "Origin": "Front",   
    "DynamicPathId": 56982,
    "ServiceMaxUtilization": null,
    "PlaceState": "Available",
    "ChangeSequence": 9,
    "ServicingVehicleGUID": "2248d535-3daf-4a86-b1e1-4951a22beec6",
    "ServiceCount": 26,

    "QueueState": "Available",
    "Capacity": 4,
    "CapacityUsed": 1,
    "LinkWayId": 111
}

```

---

<br><br>

# QueueStageSpotV1

The `QueueStageSpotV1` is an **OPTIONAL** series of transit places that need to be reached after the Primary queue and before the final spot location.


## Object attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|include <br>[`"Place"`](#place-attributes)|N/A|N/A| Reference [`"Place"`](#place-attributes)|
|`"Capacity"`| Count|u_int|The maximum number of trucks that the queue is designed for.  If more trucks than capacity queues there, the excess trucks will not be considered as queuing by the spot service (but the FMS will for production reporting).  This number is never big;  typically in the single digit.|
|`"CapacityUsed"`| Truck Count |u_int|The realtime number of trucks in the queue at the moment|
|`"QueueState"`|[`QueueState`](enum_Place.md#queuestate-enumeration)|enum|States if the queue is full or not.|
|``"ParentQueueId"``| PlaceId |ArrayOf `[uint_64]` |One or many One-level up parent (sources) that can feed this staging queue.  A parent queue can be a “primary” or a “staging” queue.|


## Example StagingQueueSpotV1
Note that this object is usually contained in an array.  But this example is for a single object.
```json
"StagingQueueSpotV1":
{
  "TimeCreation":"2018-10-31T09:30:10.43.512Z",
  "PlaceId":731854,
  "Latitude":49.176854,
  "Longitude":-123.0718,
  "Elevation":22.69,
  "Heading":68,
  "PlaceIO":"PullThrough",
  "Origin":"Load",
  "DynamicPathId":8456,
  "ServiceMaxUtilization":null,
  "PlaceState":"Open",
  "ChangeSequence":836,
  "ServicingVehicleGUID": null,
  "ServiceCount":0,

  "QueueState":"Full",
  "Capacity":1,
  "CapacityUsed":1,
  "ParentQueueId":[73185]
}

```
