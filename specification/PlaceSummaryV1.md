# PlaceSummaryV1
This message is sent by the spot service in response to a client sending a `PlaceCommandV1`.  This summary is sent alone or will be followed by `PlaceAllV1` if the scope does not end with `/summary`. It contains the meta data of the `Place` database and does not contain any `Place` objects.
When the path ends with `/summary`.  The Spot service will send this message.


|Sender| Triggered by | Triggers|
|---|---|---|
| `spot`| `PlaceCommandV1` | nothing |

<br>

## Message attributes
|key |value |format | Description|
|---|:---:|:---:|---|
|`"Timestamp"`|Last change|| Timestamp of when the last time there was a change in the spot database|
|`"ScopeList"`|path|ArrayOf <br>[`string`]| A list of all the scpoes defined in the spot service.  In version 1 this is solely `"/"`.|
|`"ChangeSet"`| counter| uint_64| A sequence number that is incremented each time a Place is modified.  This means per transaction, not per attribute edited. So if in one atomic edition 3 fields are modified at the same time, than that counts as a single change. |
|`"CountSpot"`| count | uint_64| The number of spots in the system|
|`"CountPrimaryQueue"`| count | uint_64| The number of primary queues in the system|
|`"CountStagingQueue"`| count | uint_64| The number of staging queues in the system|
|`"DocumentCount"`| count| uint_64| The number of `PlaceAllV1` messages (pages) that the service will send if it would send `PlaceAllV1` right now|


## Use Case:
A client application want to determine what are all the subset scopes it can request.  Or a client application wants to reserve the right amount of resources before it


## Example 1
This example of current implementation with a single scopes.
```json
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2023-01-24T09:30:10.948Z",

  "PlaceSummaryV1":
  {
    "Timestamp": "2023-01-23T21:35:27.226Z",
    "ScopeList": [ "/" ],
    "ChangeSet":82318740,
    "CountSpot": 36541,
    "CountPrimaryQueue": 3853,
    "CountStagingQueue": 632,
    "DocumentCount": 5
  }
}
```



## Example 2
This example would be for a future version that would support multiple scopes.
```json
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2023-01-24T09:30:10.948Z",

  "PlaceSummaryV1":
  {
    "Timestamp": "2023-01-23T21:35:27.226Z",
    "ScopeList": [ "/", "/pit1/", "/pit2/"],
    "ChangeSet":82318740,
    "CountSpot": 36541,
    "CountPrimaryQueue": 3853,
    "CountStagingQueue": 632,
    "DocumentCount": 5
  }
}
```
