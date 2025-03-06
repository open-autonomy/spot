# TaskCompleteV1

Dump tasks are completed autonomously without any human intervention. In these cases, it is important for the AHS to inform the FMS explicitly that it has completed its task and will soon be leaving the spot.

<br>

|Sender| Triggered by | Triggers |
|---|---|---|
| `AHS`| Truck completing dumping materials at spot | The FMS to exhaust the spot |

## Message attributes

## Message attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|`"LastMileId"`| DispatchingId| UUID| A unique ID for this dispatching that will remain the same throught the process of dispatching the truck to the spot and until the truck is released from the Last Mile dispatching process. |
|`"PlaceId"`| WayId | UUID | The spot where the truck has spotted and is waiting for the go ahead. |


## Use Case:
This message will be used for dump tasks to ensure that the FMS is aware that the truck has completed its task and will soon be leaving the spot. This is intended to remove the ambiguity present in V1 of the protocol where the truck would simply leave the spot without informing the FMS whether the dump succeeded or failed.

## Example
The truck 2248...eec6 spotted at a7b3c4d5...2e3f4a5b6c7d has finished dumping.
```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-24T09:30:10.948Z",
  "EquipmentId": "2248d535-3daf-4a86-b1e1-4951a22beec6",
  "TaskCompleteV1":
  {
    "LastMileId": "f1b9b3b4-0b3b-4b3b-8b3b-0b3b3b3b3b3b",
    "PlaceId": "a7b3c4d5-6e7f-8a9b-0c1d-2e3f4a5b6c7d"
  }
}
```