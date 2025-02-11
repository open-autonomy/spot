# TaskStartV1
For agreed upon tasks *(e.g. crusher dumping)*, the truck will spot and wait to be told by the spot service when to start the task.
> NOTE: The agreement that the trucks should wait before executing the task is determined at integration time and is site specific.  This configuration depends on the capability of the systems being integrated and what is best for the mine to achieve robustness of the overall solution.  Typically the FMS is already monitoring the crusher status, hence it is common for the FMS to give the go / no-go signal to the autonomous trucks to dump in the crusher. <br><br> This also means that there is no programatical way, no API, to tell a truck "Wait for a TaskStart on the next Spot".  This agreement between `AHS` and `FMS` is external to this protocol.  If the feature to control this via the protocol becomes highly desirable, then we could add a new property in the Spot class that would allow this distinction on a per Spot basis.

<br>

|Sender| Triggered by | Triggers|
|---|---|---|
| `spot` | Truck `OccupyingPlace` at Spot and truck environment is ready. | Truck to start executing `Action` for the spot |

<br>

## Message attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|`"PlaceId"`| WayId| integer| The spot where the truck has spotted and is waiting for the go ahead.|
|`"VehicleId"`| VehicleId| UUID| The vehicle spotted there.|



## Use Case:
This message will typically be used for crusher dumping to ensure the trucks don't start dumping immediately because the crusher could be too full at times or the rock breaker might be engaged.

## Example
The truck 2248...eec6 spotted at 731854 is told to start the action configured for that Spot.
```json
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2023-01-24T09:30:10.948Z",

  "TaskStartV1":
  {
    "PlaceId": 731854,
    "VehicleId": "2248d535-3daf-4a86-b1e1-4951a22beec6"
  }
}
```
