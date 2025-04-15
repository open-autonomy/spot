# TaskStartRequestV1

[![experimental](http://badges.github.io/stability-badges/dist/experimental.svg)](http://github.com/badges/stability-badges)

For agreed upon tasks *(e.g. crusher dumping)*, the truck will spot and wait to be told by the spot service when to start the task.
> NOTE: The agreement that the trucks should wait before executing the task is determined at integration time and is site specific.  This configuration depends on the capability of the systems being integrated and what is best for the mine to achieve robustness of the overall solution.  Typically the FMS is already monitoring the crusher status, hence it is common for the FMS to give the go / no-go signal to the autonomous trucks to dump in the crusher. <br><br> This also means that there is no programatical way, no API, to tell a truck "Wait for a TaskStart on the next Spot".  This agreement between `AHS` and `FMS` is external to this protocol.  If the feature to control this via the protocol becomes highly desirable, then we could add a new property in the Spot class that would allow this distinction on a per Spot basis.

<br>

|Sender| Triggered by | Triggers|
|---|---|---|
| `Spot` | Truck `OccupyingPlace` at Spot and truck environment is ready. | Truck to start executing `Action` for the spot |


**Note**: the top-level message should contain the `EquipmentId` which is the EquipmentId of the vehicle that is spotted there.

<br>

## Message attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|`"LastMileId"`| DispatchingId| UUID| A unique ID for this dispatching that will remain the same throught the process of dispatching the truck to the spot and until the truck is released from the Last Mile dispatching process. |
|`"RequestId"` | RequestId | UUID | A unique ID for to link the response message to the request message. |
|`"PlaceId"`| WayId | UUID | The spot where the truck has spotted and is waiting for the go ahead. |

## Use Case:
This message will typically be used for crusher dumping to ensure the trucks don't start dumping immediately because the crusher could be too full at times or the rock breaker might be engaged.

## Example
The truck 2248...eec6 spotted at a7b3c4d5...2e3f4a5b6c7d is told to start the action configured for that Spot.
```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-24T09:30:10.948Z",
  "EquipmentId": "2248d535-3daf-4a86-b1e1-4951a22beec6",
  "TaskStartRequestV1":
  {
    "LastMileId": "f1b9b3b4-0b3b-4b3b-8b3b-0b3b3b3b3b3b",
    "RequestId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "PlaceId": "a7b3c4d5-6e7f-8a9b-0c1d-2e3f4a5b6c7d"
  }
}
```
