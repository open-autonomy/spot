# VehicleDismissedV2

[![experimental](http://badges.github.io/stability-badges/dist/experimental.svg)](http://github.com/badges/stability-badges)

This message allows the Spot service to inform the AHS that the last mile assignment has been terminated. This is used when the vehicle is no longer able to complete the last mile assignment, for example, if the vehicle has broken down and is unable to expressly quit the last mile assignment.

Note this will release all reserved and occupied spots and queues of the vehicle in the spot service.

**Note**: the top-level message should contain the `EquipmentId` which is the EquipmentId of the vehicle that is receiving the dispatching instructions.

## Message attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|`"LastMileId"`| DispatchingId | UUID| A unique ID for this dispatching that will remain the same throught the process of dispatching the truck to the spot and until the truck is released from the Last Mile dispatching process.|

## Example

The following message indicates that the last mile assignment assigned to vehicle `be87fb7e-9eb6-11ed-a8fc-0242ac120002` has been terminated:

```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-24T09:30:10.948Z",
  "EquipmentId": "be87fb7e-9eb6-11ed-a8fc-0242ac120002",
  "VehicleDismissedV2":
  {
    "LastMileId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641"
  }
}
```
