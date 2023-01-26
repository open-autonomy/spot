# SpotV1

`SpotV1` is an class representing a position in the mine where a miner want a vehicle to go to and do something.  It is a child class of `Place`. It is used in messaging between the Spot service and its clients.


## object attributes
|key |value |format | Description|
|---|-----|-----|---|

|`""`||||
|`""`||||
|`""`||||
|`""`||||
|`""`||||
|`""`||||

```json
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
		"OwnerGUID":"2248d535-3daf-4a86-b1e1-4951a22beec6",
    }
	]
```
