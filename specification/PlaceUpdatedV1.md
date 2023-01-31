# PlaceUpdatedV1
This message is sent spontaneously to all connected clients when a place is changed.

|Sender| Triggered by | Triggers|
|---|---|---|
| `spot` | `ChangeSequence` change on any Place | local cache update for clients|

<br>

## Message attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|`""`||||
|`""`||||
|`""`||||


## Use Case:
This message is used when an event triggers it in a very special case under specific circumstances.

## Example
A spot that was closed was just open by a miner.
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
