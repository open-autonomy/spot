# SetDynamicPathIdV1
This is the method for an AHS to tell **IF** and **HOW** it can reach a Place in an open area.


|Sender| Triggered by | Triggers|
|---|---|---|
| `AHS` | a Place changed and it has no DynamicPath associated with it | AHS to compute if it can theoritically drive there |

<br>

## Message properties
|key |value |format | Description|
|---|:---:|:---:|---|
|`"StartPlaceId"`| PlaceId|uint_64| place where the DynamicPathId starts from|
|`"EndPlaceId"`| PlaceId|uint_64| place where the DynamicPathId ends|
|`"DynamicPathId"`|`null`<br> 0 <br> PathId |uint_64| A Path ID that can be used to retrieved the path geometry from the Path service. `0` has a reserved meaning.  0 means that the AHS can't compute a theoritical path to the place and that the place needs to change if you want a truck to successfully reach it. |

> The Path service is part of the Open-Autonomy family of protocols.  Please reference the Path service documentation for the techincal details.

> Discussion: We could change the message to be an array of Start-End structures{} as is't quite possible that there will be more than one path to save when a spot is created and there are more than one Primary queues in the area or that multiple staging quesues are used.  But I don't think the added complexity of an array outweighs the benefit of the simplicity of a single path per message.  For instance at boot up an AHS could decide to calcualte and save all the dynamic paths in the mine and that might require some sort of paging if there are too many.



## Use Case:
This message is used when an event triggers it in a very special case under specific circumstances.

## Example
AHS has computed a successful path intent and conveys this to the spot service.
```json
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2023-01-24T09:30:10.948Z",

  "SetDynamicPathIdV1":
  {
    "StartPlaceId": 731889,
    "EndPlaceId": 731257,
    "DynamicPathId": 58311
  }
}
```

AHS was unsuccessful at finding a theoretical path to reach the place.
```json
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2023-01-24T09:30:10.948Z",

  "SetDynamicPathIdV1":
  {
    "StartPlaceId": 731889,
    "EndPlaceId": 731257,
    "DynamicPathId": 0
  }
}
```

<br>
AHS wants to remove an intended path or delete an "I can't reach the place".  

> Note that not setting the `"DynamicPathId"` in the JSON will have the same behavior in most , if not all, libraries.

```json
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2023-01-24T09:30:10.948Z",

  "SetDynamicPathIdV1":
  {
    "StartPlaceId": 731889,
    "EndPlaceId": 731257,
    "DynamicPathId": null
  }
}
```
