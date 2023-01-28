# LeftPlaceV1
The truck sends this message when it leaves a `Place` (queue or spot).  This lets the Spot server know that it can now re-use this Place as it's now free because the truck has release its custody.  Now other trucks can use this resource.  The releasing should be done once the vehicle clears the place; say move one vehicle length from the spot.


|Sender| Triggered by | Triggers|
|---|---|---|
| `AHS`| Truck moving away | Free up a resource in Spot server |

<br>

## Message attributes

|key |value |format | Description|
|---|:---:|:---:|---|
|`"VehicleId"`| VehicleId | UUID| The vehicle that has just left the place|
|`"PlaceId"`| PlaceId |uint_64| The identidy of the place the truck has just left |


## Use Case:
Part of the spot ressource management.  The spot service will not redistribute permissions to a ressource that has not been released by `AHS`. Required when a truck has left
- the Primary queue
- a Staging queue
- a Spot

# Example
This example would be for a future version that would support multiple scopes.
```json
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2023-01-24T09:30:10.948Z",

  "LeftPlaceV1":
  {
    "VehicleId": "be87fb7e-9eb6-11ed-a8fc-0242ac120002",
    "PlaceId": 731889
  }
}
```
