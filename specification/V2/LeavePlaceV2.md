# LeavePlaceV2
This message represents what is called a **truck kickout** in the industry.  This is the signal from a shovel operator to the truck operator that it's fully loaded and it can leave.  
> NOTE: In a manned operation, the kickout signal is typically one short blast of the horn of the shovel.

<br>

|Sender| Triggered by | Triggers|
|---|---|---|
| `spot` | Shovel HMI telling the Spot service the shovel operator is done loading. | Truck leaving the spot and executing a new FMS assignment. |

<br>

**Note**: the top-level message should contain the `EquipmentId` which is the EquipmentId of the vehicle that need to leave the spot.

## Message attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|`"PlaceId"`| PlaceId | uuid | The spot an operator wants the truck to vacate. |
|`"LastMileId"` | DispatchingId | UUID | A unique ID for this dispatching that will remain the same throught the process of dispatching the truck to the spot and until the truck is released from the Last Mile dispatching process. |
|`"RequestId"` | RequestId | UUID | A unique ID for to link the response message to the request message. |



## Use Case:
This message is used to give control to the shovel operator to decide when the truck should leave.

## Example
The truck `2248d535-3daf-4a86-b1e1-4951a22beec6` spotted at `4f4e2b7e-9eb6-11ed-a8fc-0242ac120002` is told leave.  The shovel operator is done loading the truck and would like the truck to vacate the space ASAP so a new truck can spot and the operator can keep a high productivity level.
```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-24T09:34:26.18Z",
  "EquipmentId": "2248d535-3daf-4a86-b1e1-4951a22beec6",
  "LeavePlaceV2":
  {
    "RequestId" : "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "PlaceId": "4f4e2b7e-9eb6-11ed-a8fc-0242ac120002",
    "LastMileId":"23456756-aa34-5742-9b66-08a5d4294f34"
  }
}
```
