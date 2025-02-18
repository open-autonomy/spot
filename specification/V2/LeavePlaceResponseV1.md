# LeavePlaceResponseV1
All LeavePlaceV2 messages sent from the Spot Service to the AHS should be responded to with a LeavePlaceResponseV1 message.  This message is used to acknowledge the receipt of the LeavePlaceV2 message and to inform the sender of the acceptance or rejection of the request.  The response message should contain the same `EquipmentId`, `CorrelationId`, `LastMileId`. The response message should also contain a `Status` field that indicates whether the request was accepted or rejected.  If the request was rejected, the response message should also contain a `Detail` field that provides a human readable description of the reason for the rejection.

|Sender| Triggered by | Triggers|
|---|---|---|
|`AHS` | `LeavePlaceV2` message| Leave Place message was received. |

**Note**: the top-level message should contain the `EquipmentId` which is the EquipmentId of the vehicle that has just left the place.

**Note**: the top-level message should contain a `CorrelationId` that matches the `CorrelationId` found in the message containing the LeavePlaceV2 message that triggered this response.

## Message attributes

|key |value |format | Description|
|---|:---:|:---:|---|
|`"LastMileId"` | DispatchingId | UUID | A unique ID for this dispatching that will remain the same throught the process of dispatching the truck to the spot and until the truck is released from the Last Mile dispatching process. |
|`"Status"`| oneOf: [`"Accept"`, `"Reject"`] | enum | Indicates the acceptance or rejection of the request. |
|`"Detail"`| string | string <br/> `nullable` | A human readable description of the reason for the rejection. |

## Example

The following message indicates an accept response to a LeavePlaceV2 message:

```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-24T09:30:10.948Z",
  "CorrelationId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
  "EquipmentId": "be87fb7e-9eb6-11ed-a8fc-0242ac120002",
  "LeavePlaceResponseV1":
  {
    "LastMileId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "Status": "Accepted"
  }
}
```

The following message provides an example of a rejection response to a LeavePlaceV2 message:

```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-24T09:30:10.948Z",
  "CorrelationId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
  "EquipmentId": "be87fb7e-9eb6-11ed-a8fc-0242ac120002",
  "LeavePlaceResponseV1":
  {
    "LastMileId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "Status": "Rejected",
    "Detail": "The vehicle is unable to plan a collision free path."
  }
}
```

