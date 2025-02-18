# TaskStartResponseV1

This message must be sent by the AHS in response to a TaskStart2 message.  This message is used to acknowledge the receipt of the TaskStartV2 message and to inform the sender of the acceptance or rejection of the request.  The response message should contain the same `EquipmentId`, `TaskId`, and `CorrelationId` as the request message.  The response message should also contain a `Status` field that indicates whether the request was accepted or rejected.  If the request was rejected, the response message should also contain a `Detail` field that provides a human readable description of the reason for the rejection.

|Sender| Triggered by | Triggers|
|---|---|---|
|`AHS` | `TaskStartV2` message| Task assignment has been received. |

**Note**: the top-level message should contain the `EquipmentId` which is the EquipmentId of the vehicle that is receiving the task assignment.

**Note**: the top-level message should contain a `CorrelationId` that matches the `CorrelationId` found in the message containing the TaskStartV2 message that triggered this response.

## Message attributes

|key |value |format | Description|
|---|:---:|:---:|---|
|`"LastMileId"`| DispatchingId | UUID | A unique ID for this dispatching that will remain the same throught the process of last mile dispatching the truck to the spot and until the truck is released from the Last Mile dispatching process. |
|`"Response"`| oneOf: [`"Accept"`, `"Reject"`] | enum | Indicates the acceptance or rejection of the request. |
|`"Detail"`| string | string <br/> `nullable` | A human readable description of the reason for the rejection. |

## Example

The following message indicates an accept response to a TaskStartV2 message:

```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-24T09:30:10.948Z",
  "CorrelationId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
  "EquipmentId": "be87fb7e-9eb6-11ed-a8fc-0242ac120002",
  "TaskStartResponseV1":
  {
    "LastMileId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "Response": "Accepted"
  }
}
```

The following message provides an example of a rejection response to a TaskStartV2 message:

```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-24T09:30:10.948Z",
  "CorrelationId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
  "EquipmentId": "be87fb7e-9eb6-11ed-a8fc-0242ac120002",
  "TaskStartResponseV1":
  {
    "LastMileId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "Response": "Rejected",
    "Detail": "The vehicle is not at the spot point."
  }
}
```