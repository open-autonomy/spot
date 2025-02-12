# LastMileDispatchingExceptionV2
This message is sent by `AHS` to tells the `LastMileDispatching` that this vehicle hit a snag and can't reach its next position in the service chain, but that the vehicle intends to continue to the final destination after the exception is resolved via external means.  It also means that `AHS` is now expecting the `LastMileDispatching` to send it a new `OccupyPlace` message when the exception is resolved and when the truck can try again.
> NOTE-1 that the destination in the OccupyPlace message **could** change as a result of fixing the exception.<br>
> NOTE-2 It is expected that the `AHS` would also send an ISO-23725 VehicleDiagnosticV2 to the FMS at the same time.

<br>

|Sender| Triggered by | Triggers|
|---|---|---|
| `AHS`| truck is stuck but intends to complete spotting | A manual SOP that will correct the truck exception.  Once corrected, a person will tell `Last Mile Dispatching` to re-issue a service chain to the truck using an `OccupyPlaceV2` message. |

<br>

## Message attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|`"VehicleId"`| VehicleId | UUID| The vehicle that has encountered a problem while being managed by the last mile dispatching process|
|`"LastMileId"`| DispatchingId | UUID| A unique ID for this dispatching that will remain the same throught the process of last mile dispatching the truck to the spot and until the truck is released from the Last Mile dispatching process.|
|`"PlaceId"`|`nullable` PlaceId| UUID |The next place the vehicle is trying to reach but has encountered an exception during its locomotion there|
|`"ExceptionId"` | AHS Specific | uint_64 | An out of specification, AHS specific and pre-defined number that the last mile dispatching can use to match with a human readable description of the exception and/or automate a workflow |
|`"Description"`|`nullable` Human readable text|string| A human readable error message that explains what problem the truck has encountered and possibly a proposed resolution.  This field is optional, but it is expected that when null, the AHS has provided the LMD with documentation for each number with a description and possibly a remediation to the exception.


## Use Case:
This message is used by `AHS` to receive human help to complete the spotting for a truck.  This can happen at the loading face while spotting close to a heavy equipment, at the dump or crusher or even in a parking.  The causes are endless but here are a few:
* an obstacle is detected on the vehicle's trajectory
* an other vehicle is in the way or too close for the truck to proceed
* the vehicle can't maneuver to that position




## Example
While driving to the final spot `"c20f2649-fefe-49b3-a6af-db5f337d1006"`, Truck `be87fb7e-9eb6-11ed-a8fc-0242ac120002` encountered a tumbleweed on its path and sees it as a large obstacle.  The truck believes it can't proceed safely and wants human confirmation what to do next. The truck calls in the obstacle through the AHS interface and tells the last mile dispatching it has an issue:  

```json
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-06-27T07:33:51.540Z",

  "LastMileDispatchingExceptionV2":
  {
    "VehicleId": "be87fb7e-9eb6-11ed-a8fc-0242ac120002",
    "LastMileId":"23456756-aa34-5742-9b66-08a5d4294f34",
    "PlaceId": "c20f2649-fefe-49b3-a6af-db5f337d1006",
    "ExceptionId": 27,
    "Description":"I've fallen and I can't get up."
  }

}
```

A remote operator in the ROC received the alarm from the truck through the AHS UI and is looking at the truck's cameras to identify the problem and resolution.  The operator tells the truck to ignore the obstacle by disabling the obstacle detection for that particular obstacle (the tumbleweed).  After this, the operator tells the Last Mile dispatching to continue with the truck using the same spot.  So the Last Mile Dispatching sends this message to the truck:


```json
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-23T09:30:10.435Z",
  "OccupyPlaceV2":
  {
    "VehicleId": "be87fb7e-9eb6-11ed-a8fc-0242ac120002",
    "LastMileId":"23456756-aa34-5742-9b66-08a5d4294f34",
    "RequestId": "b82291f2-f97d-45cf-bb5c-601a1dbd2642",
    "Queues": [
        {
            "PlaceId": "e7b8a9f4-3c4e-4d8b-9b2e-8f9d6c3e7a1f",
            "Pose": {
                "Latitude": 49.1768531,
                "Longitude": -123.071610,
                "Elevation": 22.69,
                "Heading": 74
            },
            "PlaceIO": "PullThrough",
            "Origin": "Front",
            "Sequence": 1
        }
    ],
    "Spot": {
        "PlaceId": "c20f2649-fefe-49b3-a6af-db5f337d1006",
        "Pose": {
            "Latitude": 49.176854,
            "Longitude": -123.0718,
            "Elevation": 22.69,
            "Heading": 68
        },
        "Tolerance": {
            "Linear": 0.5,
            "Angular": 15
        },
        "PlaceIO": "BackIn",
        "Origin": "Load",
        "Task": "Load"
    }
  }
}
```

Now the truck resumes spotting normally and gets loaded.
