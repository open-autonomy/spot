# OccupyPlaceResponseV1
All OccupyPlaceRequestV1 messages should be responded to with an OccupyPlaceResponseV1 message.  This message is used to acknowledge the receipt of the OccupyPlaceRequestV1 message and to inform the sender of the acceptance or rejection of the request.  The response message should contain the same `EquipmentId`, `LastMileId`, and `RequestId` as the request message.  The response message should also contain a `Response` field that indicates whether the request was accepted or rejected.  If the request was rejected, the response message should also contain a `Detail` field that provides a human readable description of the reason for the rejection.

|Sender| Triggered by | Triggers|
|---|---|---|
|`AHS` | `OccupyPlaceRequestV1` message| Last mile assignment has been received. |

**Note**: the top-level message should contain the `EquipmentId` which is the EquipmentId of the vehicle that is occupying the place.

<br>

## Message attributes

|key |value |format | Description|
|---|:---:|:---:|---|
|`"LastMileId"`| DispatchingId | UUID| A unique ID for this dispatching that will remain the same throught the process of dispatching the truck to the spot and until the truck is released from the Last Mile dispatching process. |
|`"RequestId"` | RequestId | UUID | A unique ID for to link the response message to the request message. |
|`"Response"`| oneOf: [`"Accepted"`, `"Rejected"`] | enum | Indicates the acceptance or rejection of the request. |
|`"Detail"`| string | string <br/> `nullable` | A human readable description of the reason for the rejection. |

## Example

The following message indicates an accept response to an OccupyPlaceRequestV1 message:

```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-24T09:30:10.948Z",
  "EquipmentId": "be87fb7e-9eb6-11ed-a8fc-0242ac120002",
  "OccupyPlaceResponseV1":
  {
    "LastMileId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "RequestId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "Response": "Accepted"
  }
}
```

The following message provides an example of a rejection response to an OccupyPlaceRequestV1 message:

```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-24T09:30:10.948Z",
  "EquipmentId": "be87fb7e-9eb6-11ed-a8fc-0242ac120002",
  "OccupyPlaceResponseV1":
  {
    "LastMileId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "RequestId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "Response": "Rejected",
    "Detail": "The vehicle cannot re-spot while tipping."
  }
}
```

