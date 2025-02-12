# VehicleDismissedV2
This message allows the Spot service to inform the vehicle that the last mile assignment has been terminated and that the vehicle should leave the area.

Note that the Vehicle must send a `LeavePlaceV2` message to the Spot service to release the spot or queue point it was occupying.

## Message attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|``"VehicleId"``| VehicleId| UUID| Identification of the vehicle that is receiving the dispatching instructions.|
|``"LastMileId"``| DispatchingId| UUID| A unique ID for this dispatching that will remain the same throught the process of dispatching the truck to the spot and until the truck is released from the Last Mile dispatching process.|
|`"CorrelationId"` | CorrelationId | UUID | A unique ID that can correlate this message with the correct `VehicleDismissedResponseV1` message. |
