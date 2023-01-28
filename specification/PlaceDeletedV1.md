# PlaceDeletedV1
This message is pushed by the spot server when `Place`s are destroyed from the system.  Typically caused by an equipment operator is stopping or moving somwhere far.  The message will contain one or more `Place` that no longer exists and the client application receiving the message should purge these objects from its local cache.

|Sender| Triggered by | Triggers|
|---|---|---|
| `spot` | An operator deleting a spot | client cache update |

<br>

## Message attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|ArrayOf[] <br>`"PlaceId"`|PlaceId|uint_64| A list of one or more Places that were deleted from the spot database.|


## Use Case:
This message will be sent when a High Precision system is turned off and the spots associated with the equipemnt will be destroyed.  When an area is deleted from the map, the user has the option to delete all spots and queues inside that ara.


# Example
A small over the edge dump is permanantly closed so its associated queues and spots are deletes because they will never be re-used..
```json
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2023-01-24T09:30:10.948Z",

  "PlaceDeletedV1":
  [
    {"PlaceId": 30354},
    {"PlaceId": 30476},
    {"PlaceId": 731854},
    {"PlaceId": 731855},
    {"PlaceId": 731858},
    {"PlaceId": 731859}
  ]

}
```
