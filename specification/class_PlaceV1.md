# Place

A `Place` is a virtual class containing all the shared attributes of these children classes:
- [`Spot`](#spotv1)
- [`Primary Queue`]()
- [`Staging Queue`]()


## Place attributes
|key |value |format | Description|
|---|-----|-----|---|
| `"TimeCreation"` | timestamp | ISOxx | Timestamp when this place (spot/queue) was created |
|`"PlaceId"`| unique id | integer_64| Unique ID for the entire Spot Service that identifies a Place (Spot, primary queue, staging queue, …)  All places share the same ID space, this is to minimize the risk of bugs due to managing different ID spaces for the different child classes. further 2^64 is wide enough to accommodate all the places.|
|`"Latitude"`| &#177;90 degree |float| position on earth in LLE using WGS84.  NOTE: LLE can change if the place is re-positioned by the operator.  The float will typically require 7 decimals to reach centimeter level of resolution. |
|`"Longitude"`|&#177;180 degree|float||
|`"Elevation"`|meter above see level|float||
|`"Heading"`| 0-359 degrees|integer|Compass heading in degrees of the front of the spot.  Where the vehicle's front should point when spotted|
|`"PlaceIO"`||enum| How to Ingress & Egress that place See enum. |
|`"Origin"`||enum|How to interpret the point served by the spot service in reference to the truck’s origin frame of reference|
|`"DynamicPathId"`||integer<br>`nullable`|Optionally, but expected, to be set by the AHS when a dynamic path that leads to this place is computed in the free space of the area. Semantic null=”Not Set” = Initial state, 0=”Unreachable”,  >0 = greater than 0 means valid path computed in the free space of the area.|
|`"ServiceMaxUtilization"`||integer<br>`nullable`| The number of times this place can be used before it needs to be “reset” by an external event. Can be set to INFINITE via “null”.  Null means that there is no Max Utilization set.  Dispatching can use this to know how many future trucks can be sent there. Useful for loading and dumping, especially paddock (set=1).  Setting this value (writing), even to the same value, will reset the Service Count below.  NOTE: Writing to this value is not the proper way to reset the counter, there is an API to reset it back to zero.|
|`"PlaceState"`||enum| {Closed, Open, Pending, PendingDelete}  This state is User controlled through user requests. Pending = Pending close and waiting that the place is freed before going into closed.  PendingDelete: same but for delete.|
|`"ChangeSequence"`|||a sequence number that is incremented each time this place (and derivations) changes |
|`"ServicingVehicleGUID"`||integer<br>`nullable`|Equipment that has reserved or is there at the moment. Note: for queues the spot server only places the first vehicle here. null= No vehicle utilizing this resource|
|`"ServiceCount"`||||



<br><br>

# SpotV1

`SpotV1` is a position in the mine where a miner want a vehicle to go to and do something.  It is a child class of `Place`. It is used in messaging between the Spot service and its clients.


## object attributes
|key |value |format | Description|
|---|-----|-----|---|
|`"Place"`|see `"Place"`|see `"Place"`|see `"Place"`|
|`"Action"`||||
|`"ServiceChain":[]`||||
|`""`||||
|`""`||||
|`""`||||

```json
"SpotV1":
	[
		{
      "TimeCreation":"2018-10-31T09:30:10.43.512Z",
      "PlaceId":731854,
      "Latitude":49.176854,
      "Longitude":-123.0718,
      "Elevation":22.69,
      "Heading":68,
      "PlaceIO":"BackIn",
      "Origin":"Load",
      "DynamicPathId":8456,
      "ServiceMaxUtilization":null,
      "PlaceState": "Closed",
      "ChangeSequence": 3452,
      "ServicingVehicleGUID": null,
      "ServiceCount":0,
      "Action":"Load",
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
