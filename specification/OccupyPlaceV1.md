# OccupyPlaceV1

The Occupy place message is a mini-dispatching instruction set on how to reach a spot while in an open area.  Historically, open areas were not managed by fleet management systems for manned systems, so there is no equivalent in a manned operation.


|Sender| Triggered by | Triggers|
|---|---|---|
|`Spot` | `ApproachingLastMileV1` message| truck movement in open area|

<br>

This message gives explicit permissions, to a truck identified by the VehicleID, to use the listed place(s). The message will including all the places the truck currently possesses (not just added permissions).   The truck will have to Release each Place once it leaves that resource via the [`LeftPlaceV1`](LeftPlaceV1.md) message.  Even if the truck does not stop at the resource, the truck must release the place once it passes that LLE position so it’s released for other trucks to use.

A place permission value of `0` will be set to mean that the vehicle does NOT have permission for a place type.  Multiple `OccupyPlace` messages can be sent to the same truck during the last mile dispatching.  Each new message from the spot service overwrites the previous permissions.  A truck that has had its permission revoked while it was moving to THAT place that just got revoked must stop (in a controlled normal manner) and wait for a new `OccupyPlace` messages.

> IMPORTANT NOTE: Previously granted resources can be removed or changed in a later OccupyPlace message.  


The message envelop will also contain a copy of all objects representing the resources the vehicle is given permission to.  Not only is this convenient for the client software, it is primiraly there to prevent race conditions or the usage of stale cached objects on the client side.


## Message attributes

|key |value |format | Description|
|---|:---:|:---:|---|
|``"VehicleId"``| VehicleId| UUID| Identification of the vehicle that is receiving the dispatching instructions.|
|``"LastMileId"``| DispatchingId| UUID| A unique ID for this dispatching that will remain the same throught the process of dispatching the truck to the spot and until the truck is released from the Last Mile dispatching process.|
|``"QueueIdPrimary"`` |PlaceId|uint_64| This value is **not nullable**. It can either be 0, and that means that the vehicle does not have permission to use the Primary queue. Or it can be a valid PlaceId representing a primary queue and that means that the truck has permission to use (be in) the primary queue.|
|``"QueueIdStage"`` |PlaceId|ArrayOf `[uint_64]` <br> `nullable`| A null value means that there are no staging queues in this dispatch.  As such an empty array `[]` is **invalid** and should be considered a bug.  The array will either contain one or more {zeros or staging queue IDs} depending if the truck has permission to use the staging queue or not. |
|``"SpotId"`` |PlaceId|uint_64| This value is **not nullable**. It can either be 0, and that means that the vehicle does not have permission to use the spot. Or it can be a valid PlaceId representing a spot and that means that the truck has permission to use (be in) the spot.|


## Use Case:



## Example 1
Vehicle 2248…beec6 is granted permission to go through the Primary queue directly down to the Spot 731854.  In this example, there is no staging queue (as it it set to `null`).  Because the truck has received 2 permissions, the message also includes the current state of the 2 places.

```json
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2023-01-23T09:30:10.435Z",

  "OccupyPlaceV1":
  {
    "VehicleId": "2248d535-3daf-4a86-b1e1-4951a22beec6",
    "LastMileId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "QueueIdPrimary": 56982,
    "QueueIdStage": null,
    "SpotId": 731854
  },

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
    "PlaceState": "Opened",
    "ChangeSequence": 3452,
    "ServicingVehicleGUID": null,
    "ServiceCount":0,
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
  },

  "PrimaryQueueSpotV1":
  {
      "TimeCreation": "2023-01-23T09:30:10.43.511Z",
      "PlaceId" : 56982,
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
      "Capacity": 4,
      "CapacityUsed": 1,
      "QueueState": "Open",
      "LinkWayId": 111
  }


}
```


## Example 2
Vehicle `9ac95f3e-9eac-11ed-a8fc-0242ac120002` is granted permission to enter but stop at the Primary queue and wait there.  In this example there is one staging queue, but the truck does not have permission to continue to it.  Only he primary queue object is included with the dispatch because the truck has only permission to the primary queue.

```json
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2023-01-23T09:30:10.435Z",

  "OccupyPlaceV1":
  {
    "VehicleId": "9ac95f3e-9eac-11ed-a8fc-0242ac120002",
    "LastMileId": "43b3bd52-9eac-11ed-a8fc-0242ac120002",
    "QueueIdPrimary": 56982,
    "QueueIdStage": [0],
    "SpotId": 0
  },

  "PrimaryQueueSpotV1":
  {
    "TimeCreation": "2023-01-23T09:30:10.43.511Z",
    "PlaceId" : 56982,
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
    "Capacity": 4,
    "CapacityUsed": 1,
    "QueueState": "Open",
    "LinkWayId": 111
  }
}
```
