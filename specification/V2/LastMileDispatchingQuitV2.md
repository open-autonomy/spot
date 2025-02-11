# LastMileDispatchingQuitV2
This message is sent by `AHS` to tells the Spot Service that the spot service should no longer manage this vehicle as part of the last mile dispatching process.  It also implies releasing all permissions to `Place`s that were granted to this vehicle.  <br><br> This is done if a truck needs to “leave” or is no longer able to complete its mission while it was already taken charge by the Last Mile dispatching part of the spot service. 

When the truck quits it is the responsibility of the truck to release the queue point or spot it was occuping when it quit the last mile assignment by sending a `LeftPlaceV1` message to the Spot Service. 

|Sender| Triggered by | Triggers|
|---|---|---|
| `AHS`| truck needs to do something else | release of any `Place`s held by the truck |

<br>

## Message attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|`"VehicleId"`| VehicleId | UUID| The vehicle that has just requested to quit being managed by the last mile dispatching process|
|`"LastMileId"`| DispatchingId | UUID| A unique ID for this dispatching that will remain the same throught the process of dispatching the truck to the spot and until the truck is released from the Last Mile dispatching process.|


## Use Case:
This message is used by `AHS` to get a truck out of the last mile dispatching process.  There are multiple reasons why a truck could want to leave before it reaches its spot destination; typically because it was re-dispatched by the FMS.  But many other exceptions may result in a `LastMileDispatchingQuitV1` being generated.

## Example
While waiting in a queue to be loaded by a shovel, Truck `be87fb7e-9eb6-11ed-a8fc-0242ac120002` was re-dispatched to a new destination because the shovel just went down due to a mechanical failure.

```json
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-24T09:30:10.948Z",

  "LastMileDispatchingQuitV1":
  {
    "VehicleId": "be87fb7e-9eb6-11ed-a8fc-0242ac120002",
    "LastMileId":"23456756-aa34-5742-9b66-08a5d4294f34"
  }

}
```
