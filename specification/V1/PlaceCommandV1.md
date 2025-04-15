# PlaceCommandV1
This message is used to request a copy of all the `Places` currently defined in the system.  This message may not be useful for most client applications.  It could be used if part of the client application has lost its context but kept the connection open and wants to rebuilt its cache of all the places again.

> NOTE: When a new connection is established to the spot service, the Spot service will automatically send a `PlaceAllV1` message without the client requesting it.  This message can re-initiate this message without having to disconnect.  Further, the spot service will automatically send differential updates to the `Places` as they are modified so if the client application keeps its cache updated, it should not need to use this message.


|Sender| Triggered by | Triggers|
|---|---|---|
| `client`| Client wants to re-sync its cache of `Places`| `PlaceAllV1` message |


## Message attributes

|key |value |format | Description|
|---|:---:|:---:|---|
| `"VehicleId"` | VehicleID | UUID | The UUID identifying the vehicle defined in the fleet definition. |
| `"Method"` | ``"GET"`` | string | Identifies the command. Currently only `"GET"` is supported.|
| `"Scope"` | path | string| Currently there are only 2 path supported : `"/"` and `"/summary"`.|

When the path ends with the reserved word `/summary`, then the service will only return the **meta data** of the path of the Spot "database".
In the future, we plan on providing the ability to specify a lower scope as the path to retrieve a subset of the "database" like `"/pit1/"` or `"/pit1/eastParking/"`.


## Use case 1
The client application wants to retrieve meta data only to make sure it won't overwhelm it and that it can reserve the right amoutn of resources before the flow of Places begins.  This will trigger the spot service to issue a `PlaceSummaryV1` messages.

```json
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2023-01-23T09:30:10.831Z ",

  "PlaceCommandV1":
  {
     "Method":"GET",
     "Scope": "/summary"
  }
}

```


## Use case 2
The client application wants to receive a copy of all the Places in service.  This will trigger the spot service to issue a series of `PlaceAllV1` messages.

```json
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2023-01-23T09:30:10.831Z ",

  "PlaceCommandV1":
  {
     "Method":"GET",
     "Scope": "/"
  }
}

```
