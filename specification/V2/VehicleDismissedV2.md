# VehicleDismissedV2
This message allows the Spot service to inform the vehicle that the last mile assignment has been terminated and that the vehicle should leave the area.

Note that the Vehicle must send a `LeavePlaceV2` message to the Spot service to release the spot or queue point it was occupying.

## Message attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|``"VehicleId"``| VehicleId| UUID| Identification of the vehicle that is receiving the dispatching instructions.|
|``"LastMileId"``| DispatchingId| UUID| A unique ID for this dispatching that will remain the same throught the process of dispatching the truck to the spot and until the truck is released from the Last Mile dispatching process.|

## Example

The following message indicates that the last mile assignment assigned to vehicle `be87fb7e-9eb6-11ed-a8fc-0242ac120002` has been terminated:

```json
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-24T09:30:10.948Z",
  "CorrelationId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",

  "VehicleDismissedV2":
  {
    "VehicleId": "be87fb7e-9eb6-11ed-a8fc-0242ac120002",
    "LastMileId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641"
  }
}
```
