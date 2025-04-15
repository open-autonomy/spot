# PlaceCreatedV1
This message is sent to all connected `client` applications when a new `Place` is created in the spot databse.  The `Place` can be one of these:
- `Spot`
- `PrimaryQueue`
- `StagingQueue`

>NOTE: It will be rare that multiple spots and queues are created at the same time.  So to simplify the implementation, we have elected to use one message per `Place` created.  Currently places are not expected to be sent as an array or have multiple `Place`s objects per message.

<br>

|Sender| Triggered by | Triggers|
|---|---|---|
| `spot` | A Spot or Queue was created in the mine| `client` applications to update their local cache|

<br>

## Message attributes
||key |value |format | Description|
|---|---|:---:|:---:|---|
|OnlyOneOf:||||
||[`"SpotV1"`](class_PlaceV1.md#spotv1)|N/A|object| See Reference [`"SpotV1"`](class_PlaceV1.md#spotv1)|
||[`"PrimaryQueueSpotV1"`](class_PlaceV1.md#primaryqueuespotv1)|N/A|object| See Reference [`"PrimaryQueueSpotV1"`](class_PlaceV1.md#primaryqueuespotv1)|
||[`"StagingQueueSpotV1"`](class_PlaceV1.md#queuestagespotv1)|N/A|object| See Reference [`"PrimaryQueueSpotV1"`](class_PlaceV1.md#queuestagespotv1)|


## Use Case:
This message is used when an event triggers it in a very special case under specific circumstances.  We hope this explanation clarifies everything and is not too verbose in its layout of the sequence of events that leads to its usage.🤔

## Example 1
The shovel operator is preparing to load trucks at a new location.  The operator moves the bucket where the truck is wanted, then through the shovel HMI (likely a digital input) the operator creates a new spot position at ground level right under the current bucket position.
```json
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2023-01-24T09:30:11.47Z",

  "PlaceCreatedV1":
  {
    "SpotV1":
    {
      "TimeCreation": "2023-01-24T09:30:10.43.511Z",
      "PlaceId" : 731854,
      "PlaceState": "Closed",
      "Latitude" :49.176854,
      "Longitude" :-123.0718,
      "Elevation" : 22.69,
      "Heading" : 68,
      "Action": "Load",
      "PlaceIO": "BackIn",
      "Origin":"Load",    
      "DynamicPathId": 78456,
      "ServiceMaxUtilization": null,
      "ChangeSequence": 1,
      "ServicingVehicleGUID": null,
      "ServiceCount": 0,
      "OwnerWayId": 745932,
      "OwnerGUID": "2248d535-3daf-4a86-b1e1-4951a22beec6",
      "SpotState": "Available"
    }
  }
}
```
