# SpotServiceV1
The first message sent by the `spot` service.  It identifies that the client is talking to the right service and identified the highest protocol version it supports.

> NOTE: Security is supplied at the infrastructure level, not at the application level.  The Authentication and authorization has already been done by the time the client reahes this point;  so it is considered safe to provide a lot of information on the service beyond this point.

> NOTE: The API version will be incremented when significant new functions are added to the protocol or if backwards compatibility is broken.  Bug fixes have no impact on the protocol version.

> NOTE: In the future, each message could have its onw key:value pair, where the value would be the version number supported by this implementation.  But we are KISSing it for now.

|Sender| Triggered by | Triggers|
|---|---|---|
| `spot`  | Client connection | Normally Nothing. <br>  Misconfiguration: disconnection of the client if it connected to the wrong service. |

<br>

## Message attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|`"API"`| Version| integer|The highest spot protocol version it supports|


## Use Case:
This message is used when an event triggers it in a very special case under specific circumstances.

## Example
This example would be for a future version that would support multiple scopes.
```json
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2023-01-24T09:30:10.948Z",

  "SpotServiceV1":
  {
	  "API": 1
  }

}
```
