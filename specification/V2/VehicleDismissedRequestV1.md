# VehicleDismissedRequestV1
This message allows the Spot service to inform the vehicle that the last mile assignment has been terminated and that the vehicle should leave the area.

Note that the Vehicle must send a `LeavePlaceRequestV1` message to the Spot service to release the spot or queue point it was occupying.

**Note**: the top-level message should contain the `EquipmentId` which is the EquipmentId of the vehicle that is receiving the dispatching instructions.

## Message attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|`"LastMileId"`| DispatchingId | UUID| A unique ID for this dispatching that will remain the same throught the process of dispatching the truck to the spot and until the truck is released from the Last Mile dispatching process.|
|`"RequestId"` | RequestId | UUID | A unique ID for to link the response message to the request message. |

## Example

The following message indicates that the last mile assignment assigned to vehicle `be87fb7e-9eb6-11ed-a8fc-0242ac120002` has been terminated:

```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-24T09:30:10.948Z",
  "EquipmentId": "be87fb7e-9eb6-11ed-a8fc-0242ac120002",
  "VehicleDismissedRequestV1":
  {
    "LastMileId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "RequestId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641"
  }
}
```
