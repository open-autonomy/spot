# OccupyPlaceV2

The Occupy place message is a mini-dispatching instruction set on how to reach a spot while in an open area.  Historically, open areas were not managed by fleet management systems for manned systems, so there is no equivalent in a manned operation.

|Sender| Triggered by | Triggers|
|---|---|---|
|`Spot` | `ApproachingLastMileV2` message| truck movement in open area. |
|`Spot` | Resource becomes available | A queue point or spot that is needed by the truck becomes available. |
|`Spot` | Resource becomes unavailable | A queue point or spot that is needed by the truck becomes unavailable. |
|`Spot` | Manual Reissue | Truck is directed to continue spotting to the either same or a different spot by a dispatcher. |

<br>

This message gives explicit permissions, to a truck identified by the EquipmentID, to use the listed place(s). The message will including all the places the truck currently possesses (not just added permissions).   The truck will have to Release each Place once it leaves that resource via the [`LeftPlace2`](LeftPlaceV2.md) message.  Even if the truck does not stop at the resource, the truck must release the place once it passes that LLE position so it’s released for other trucks to use.

A place permission value of `0` will be set to mean that the vehicle does NOT have permission for a place type.  Multiple `OccupyPlace` messages can be sent to the same truck during the last mile dispatching.  Each new message from the spot service overwrites the previous permissions.  A truck that has had its permission revoked while it was moving to THAT place that just got revoked must stop (in a controlled normal manner) and wait for a new `OccupyPlace` messages.

> IMPORTANT NOTE: Previously granted resources can be removed or changed in a later OccupyPlace message.  


The message envelop will also contain a copy of all objects representing the resources the vehicle is given permission to.  Not only is this convenient for the client software, it is primiraly there to prevent race conditions or the usage of stale cached objects on the client side.

**Note**: the top-level message should contain the `EquipmentId` which is the EquipmentId of the vehicle that is receiving the dispatching instructions.

## Message attributes

|key |value |format | Description|
|---|:---:|:---:|---|
|`"LastMileId"`| DispatchingId| UUID| A unique ID for this dispatching that will remain the same throught the process of dispatching the truck to the spot and until the truck is released from the Last Mile dispatching process. |
|`"RequestId"` | RequestId | UUID | A unique ID for to link the response message to the request message. |
|`"Queues"` | N/A | ArrayOf `[Queue]` | A list of zero or more queue points that the vehicle will need to transit through. |
|`"Spot"` | Spot | Spot <br> `nullable` | Either describes the spot the vehicle has permission to or null if no spot is currently available. |


## Use Case:



## Example 1
Vehicle `2248d535-3daf-4a86-b1e1-4951a22beec6` is granted permission to go through the Primary queue directly down to the Spot e7b8a9f4-3c4e-4d8b-9b2e-8f9d6c3e7a1f.  In this example, there is a single queue point (likely at the entry to the area).

```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-23T09:30:10.435Z",
  "EquipmentId": "2248d535-3daf-4a86-b1e1-4951a22beec6",
  "OccupyPlaceV2":
  {
    "LastMileId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "RequestId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "Queues": [
        {
            "PlaceId": "e7b8a9f4-3c4e-4d8b-9b2e-8f9d6c3e7a1f",
            "Pose": {
                "Latitude": 49.1768531,
                "Longitude": -123.071610,
                "Elevation": 22.69,
                "Heading": 74
            },
            "PlaceIO": "PullThrough",
            "Origin": "Front",
            "Sequence": 1
        }
    ],
    "Spot": {
        "PlaceId": "e7b8a9f4-3c4e-4d8b-9b2e-8f9d6c3e7a1f",
        "Pose": {
            "Latitude": 49.176854,
            "Longitude": -123.0718,
            "Elevation": 22.69,
            "Heading": 68
        },
        "Tolerance": {
            "Linear": 0.5,
            "Angular": 15
        },
        "PlaceIO": "BackIn",
        "Origin": "Load",
        "Task": "Load"
    }
  }
}
```


## Example 2
Vehicle `9ac95f3e-9eac-11ed-a8fc-0242ac120002` is granted permission to enter but stop at the Primary queue and wait there.  In this example there is one staging queue, but the truck does not have permission to continue to it.  Only he primary queue object is included with the dispatch because the truck has only permission to the primary queue.

```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-23T09:30:10.435Z",
  "EquipmentId": "9ac95f3e-9eac-11ed-a8fc-0242ac120002",
  "OccupyPlaceV2":
  {
    "LastMileId": "7a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "RequestId": "e82291f2-f97d-45cf-bb5c-601a1dbd2642",
    "Queues": [
        {
            "PlaceId": "e7b8a9f4-3c4e-4d8b-9b2e-8f9d6c3e7a1f",
            "Pose": {
                "Latitude": 49.1768531,
                "Longitude": -123.071610,
                "Elevation": 22.69,
                "Heading": 74
            },
            "PlaceIO": "PullThrough",
            "Origin": "Front",
            "Sequence": 1
        }
    ],
    "Spot": null
  }
}
```


## Example 3
Vehicle `9ac95f3e-9eac-11ed-a8fc-0242ac120002` is granted permission to go to the Spot `9ac95f3e-9eac-11ed-a8fc-0242ac120002`. In this example, there are no queues as the truck has left the last queue point.

```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-23T09:30:10.435Z",
  "Equipment": "9ac95f3e-9eac-11ed-a8fc-0242ac120002",
  "OccupyPlaceV2":
  {
    "LastMileId": "3a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "RequestId": "e82291f2-f97d-45cf-bb5c-601a1dbd2642",
    "Queues": [],
    "Spot": {
        "PlaceId": "9ac95f3e-9eac-11ed-a8fc-0242ac120002",
        "Pose": {
            "Latitude": 49.176854,
            "Longitude": -123.0718,
            "Elevation": 22.69,
            "Heading": 68
        },
        "Tolerance": {
            "Linear": 0.5,
            "Angular": 15
        },
        "PlaceIO": "BackIn",
        "Origin": "Load",
        "Task": "Load"
    }
  }
}
```