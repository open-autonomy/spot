# PlaceUpdatedV1
This message is sent spontaneously to all connected clients when a place is changed.  
>NOTE: It is important to remember that the object will only contain the attributes that were modified.  The service will not send the entire object.  Because the `ChangeSequence` value will increment each time there is a change, you can rely that this key will always be present in all PlaceUpdated messages.

|Sender| Triggered by | Triggers|
|---|---|---|
| `spot` | `ChangeSequence` change on any Place | local cache update for clients|

<br>

## Message attributes

||key |value |format | Description|
|---|---|:---:|:---:|---|
|OnlyOneOf:||||
||[`"SpotV1"`](class_PlaceV1.md#spotv1)|N/A|object| **ONLY** the changed keys:value of the object will be sent.  Unchanged values shall be ommitted. |
||[`"PrimaryQueueSpotV1"`](class_PlaceV1.md#primaryqueuespotv1)|N/A|object| **ONLY** the changed keys:value of the object will be sent.  Unchanged values shall be ommitted.|
||[`"StagingQueueSpotV1"`](class_PlaceV1.md#queuestagespotv1)|N/A|object| **ONLY** the changed keys:value of the object will be sent.  Unchanged values shall be ommitted.|



## Use Case:
This message is used to update the local cache of client applications without having to re-send the entire database.  Minimizing bandwidth and processing.

## Example 1
A spot that was previously closed was just open by a miner.
```json
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2023-01-24T09:30:10.948Z",

  "PlaceUpdatedV1":
  {
    "SpotV1":
    {
      "PlaceId": 731854,
      "PlaceState": "Open",
      "ChangeSequence": 84,
      "SpotState": "Available"
    }
  }
}

```
