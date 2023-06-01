# OccupyingPlaceV1
The way the electronic driver tells the spot service it has reached a `Place`.  This message must be sent when:
- the truck has reached or just crossed (while driving) the position of a `Place` it has permission.
- the truck is immobilized at this `Place`; if this is the last permission granted in the chain.
- the truck is the first in the queue. If in a queue, trucks behind the first truck will not send msg until it reaches the front of queue.

<br>

|Sender| Triggered by | May Trigger |
|---|---|---|
| `AHS`| Truck has reached the Place | `OccupyPlaceV1`  |

<br><br>

## Message attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|`"VehicleId"`| VehicleId | UUID| The vehicle that is occupying the place|
|`"LastMileId"`| DispatchingId | UUID| A unique ID for this dispatching that will remain the same throught the process of dispatching the truck to the spot and until the truck is released from the Last Mile dispatching process.|
|`"PlaceId"`| PlaceId |uint_64| The identidy of the place the truck is occupying |


## Use Case:
This message is used by the Spot service to track where the trucks are on the service chain to the spot.

## Example
This example would be for a future version that would support multiple scopes.
```json
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2023-01-24T09:30:10.948Z",

  "OccupyingPlaceV1":
  {
    "VehicleId": "be87fb7e-9eb6-11ed-a8fc-0242ac120002",
    "LastMileId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "PlaceId": 731889
  }
}

```
