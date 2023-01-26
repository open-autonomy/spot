# PlaceAllV1

This message is send by the spot service at connection time or on request by the client.

|Sender| Triggered by|
|---|---|
|`Spot` | New client connection or <br> `PlaceCommandV1` message |

## Message attributes

|key |value |format | Description|
|---|-----|-----|---|
| `"ChangeSet"` | Change SequenceNumber | integer| A number that increments each time there is a modification to the DB |
| ``"DocumentSequence"`` | Current page | integer | The current page in the sequence of pages being sent to the client|
|``"DocumentCount"`` | Total pages | integer | The number of pages required to send the entire DB.  The spot service will cut the data in a series of pages if the data gets beyond the capacity of a page. |


## Usage
The spot service will chunk the whole database into several pages when there are more than XXX (configurable, say 500 for now) places.  The number of places and pages can be known beforehand by requesting the GetPlaceSummary.
```JSON
{
"Protocol": "Open-Autonomy",
"Version": 1,
"Timestamp": "2018-10-31T09:30:10.511Z",

"PlaceAllV1": {
"ChangeSet": 82318740,
"DocumentSequence": 1,
"DocumentCount": 1,		
	"SpotV1":
	[
		{
		"TimeCreation":"2018-10-31T09:30:10.43.512Z",
		"PlaceId":731854,
		"PlaceState": "Closed",
		"Latitude":49.176854,
		"Longitude":-123.0718,
		"Elevation":22.69,
		"Heading":68,
		"Action":"Load",
		"PlaceIO":"BackIn",
		"Origin":"Load",
		"DynamicPathId":8456,
		"ServiceMaxUtilization":null,
		"ServicingVehicleGUID": null,
		"ServiceCount":0,
		"ServiceChain":
		[
			{
			"LinkWayId": 945723,
			"LinkQueuePrimary": 30354,
			"LinkQueueStage":[ 30476 ]
			},
			{
			"LinkWayId": 945361,
			"LinkQueuePrimary": 30352,
			"LinkQueueStage":[ 30475,30477]
			}	   
		],
		"OwnerWayId":745932,
		"OwnerGUID":"2248d535-3daf-4a86-b1e1-4951a22beec6"
  }
	],
	"StagingQueueSpotV1":
	[
		{
		"TimeCreation":"2018-10-31T09:30:10.43.512Z",
		"PlaceId":731854,
		"Latitude":49.176854,
		"Longitude":-123.0718,
		"Elevation":22.69,
		"Heading":68,
		"Action":"Load",
		"PlaceIO":"PullThrough",
		"Origin":"Load",
		"DynamicPathId":8456,
		"ServiceMaxUtilization":null,
		"ServicingVehicleGUID":"00000000-0000-0000-0000-000000000000",
		"ServicingVehicleGUID": null,
		"ServiceCount":0,
		"Capacity":1,
		"CapacityUsed":1,
		"QueueState":"Full",
		"ParentQueueId":[73185],
		"OwnerWayId":745932,
		"OwnerGUID":"2248d535-3daf-4a86-b1e1-4951a22beec6"
		}
	],
	"PrimaryQueueSpotV1":
	[
	  {
            "TimeCreation": "2021-01-20T09:30:10.43.511Z",
            "PlaceId" : 731856,
            "PlaceState": "Opened",
            "Latitude" :49.176853,
            "Longitude" :-123.0716,
            "Elevation" : 22.69,
            "Heading" : 74,
            "Action": "Wait",
            "PlaceIO": "PullThrough",
            "Origin":"Front",   
            "DynamicPathId": 56982,
            "ServiceMaxUtilization": null,
            "PlaceState": "Available",
            "ChangeSequence": 9,
            "ServicingVehicleGUID": "2248d535-3daf-4a86-b1e1-4951a22beec6",
            "ServiceCount": 26,
            "Capacity": 4,
            "CapacityUsed": 1,
            "QueueState": "Open",
            "LinkWayId": 111
	}
	]
}
}

```
