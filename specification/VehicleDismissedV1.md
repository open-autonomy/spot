# VehicleDismissedV1
This message is sent by `LastMileDispatching` when an internal inconsistency or error has occurred.  It tells `AHS` that this vehicle is no longer monitored nor dispatched by the `LastMileDispatching`.  If the vehicle was previously granted `Place` resources, then all resources held by the vehicle is lost and therefore freed in the `LastMileDispatching`.

This message is an extensible message that allows the `LastMileDispatching` to tell `AHS` that it has encountered a snag.  This message allows `AHS` to route the information back to a person for awareness or to handle any exception.  It also notifies `AHS` that the truck needs to re-start the communication from the beginning with [`ApproachingLastMileV1`](ApproachingLastMileV1.md).


<br>

|Sender| Triggered by | Triggers|
|---|---|---|
| `LastMileDispatching` | `LastMileDispatching` Internal inconsistency or error | `AHS` to re-start communication from `ApproachingLastMileV1` |

<br>

## Message attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|`"VehicleId"`| VehicleId | UUID| The vehicle that is removed and cleared from the `LastMileDispatching`|
|`"LastMileId"`| `nullable` DispatchingId | UUID| Because this message can be used when `LMD` is inconsistent, the unique ID might be missing or invalid. `AHS` should only use this field for logging purposes and not rely on it for any logic|
|`"ExceptionId"` | `LMD` Specific | uint_64 | An out of specification, `LastMileDispatching` specific and pre-defined number that the `AHS` can use to match with a human readable description of the problem and/or automate a workflow |
|`"Description"`|`nullable` Human readable text| string| A human readable error message that explains what problem the `LastMileDispatching` has encountered and possibly a proposed resolution.  This field is optional, but it is expected that if `null`, the `LMD` has provided the AHS with documentation for each ExceptionId number with a description and possibly a remediation to the exception.


## Use Case:
The causes are endless but here are a few:
* `LastMileDispatching` service has crashed and lost all internal state, but the rest of the system is fine.  Upon re-start, `LMD` will send an `ExpelVehicle` message for each vehicle in the AHS Fleet Definition so both `LastMileDispatching` and `AHS` will now continue with the same internal state.  (Granted this is undesirable, but better than a full system reboot.)
* a Vehicle sent a valid `LastMileDispatching` message but with inconsistent or invalid PlaceID, WayId or UUID.
* The `LMD` implementation has an internal limitation and denies access to the `LastMileDispatching` service when the primary queue is full.
