# VehicleDismissedResponseV1

VehicleDismissedResponseV1 is the response object for the VehicleDismissedV2 message.  This message is used to acknowledge the receipt of the VehicleDismissedV2 message and to inform the sender of the acceptance or rejection of the request.  The response message should contain the same `VehicleId`, `LastMileId`, and `CorrelationId` as the request message. The response message should also contain a `Status` field that indicates whether the request was accepted or rejected.  If the request was rejected, the response message should also contain a `Detail` field that provides a human readable description of the reason for the rejection.

## Message attributes

|key |value |format | Description|
|---|:---:|:---:|---|
|``"VehicleId"``| VehicleId| UUID| Identification of the vehicle that is receiving the dispatching instructions.|
|``"LastMileId"``| DispatchingId| UUID| A unique ID for this dispatching that will remain the same throught the process of dispatching the truck to the spot and until the truck is released from the Last Mile dispatching process.|
|`"CorrelationId"` | CorrelationId | UUID | A unique ID found in the associated `ApproachingLastMileV2` message that triggered this response.|
|`"Status"`| oneOf: [`"Accept"`, `"Reject"`] | enum | Indicates the acceptance or rejection of the request.|
|`"Detail"`|`nullable` string | string | A human readable description of the reason for the rejection.|

## Example

The following message indicates an accept response to a VehicleDismissedV2 message:

```
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-24T09:30:10.948Z",

  "VehicleDismissedResponseV1":
  {
    "VehicleId": "be87fb7e-9eb6-11ed-a8fc-0242ac120002",
    "LastMileId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "CorrelationId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "Status": "Accepted"
  }
}
```

The following message provides an example of a rejection response to a VehicleDismissedV2 message:

```
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-24T09:30:10.948Z",

  "VehicleDismissedResponseV1":
  {
    "VehicleId": "be87fb7e-9eb6-11ed-a8fc-0242ac120002",
    "LastMileId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "CorrelationId": "a82291f2-f97d-45cf-bb5c-601a1dbd2641",
    "Status": "Rejected",
    "Detail": "Unknown last mile assignment."
  }
}
```
