# LastMileDispatchingQuitV1
This message is sent by `AHS` to tells the Spot Service that the spot service should no longer manage this vehicle as part of the last mile dispatching process.  It also implies releasing all permissions to `Place`s that were granted to this vehicle.  <br><br> This is done if a truck needs to “leave” or is no longer able to complete its mission while it was already taken charge by the Last Mile dispatching part of the spot service.  When quitting, the truck must vacate the Place and area so the place can be used by other vehicles.  A truck must not quit and stay stationary because the resources it holds are released automatically and may be given to other trucks.

|Sender| Triggered by | Triggers|
|---|---|---|
| `AHS`| truck needs to do something else | release of any `Place`s held by the truck |

<br>

## Message attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|`"VehicleId"`| VehicleId | UUID| The vehicle that has just requested to quit being managed by the last mile dispatching process|


## Use Case:
This message is used by `AHS` to get a truck out of the last mile dispatching process.  There are multiple reasons why a truck could want to leave before it reaches its spot destination; typically because it was re-dispatched by the FMS.  But many other exceptions may result in a `LastMileDispatchingQuitV1` being generated.

## Example
While waiting in a queue to be loaded by a shovel, Truck `be87fb7e-9eb6-11ed-a8fc-0242ac120002` was re-dispatched to a new destination because the shovel just went down due to a mechanical failure.

```json
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2023-01-24T09:30:10.948Z",

  "LastMileDispatchingQuitV1":
  {
    "VehicleId": "be87fb7e-9eb6-11ed-a8fc-0242ac120002"
  }

}
```
