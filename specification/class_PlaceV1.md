# Place

A `Place` is a virtual class containing all the shared attributes of these children classes:
- `Spot`
- `Primary Queue`
- `Staging Queue`


## Place attributes
|key |value |format | Description|
|---|-----|-----|---|
| `"TimeCreation"` | timestamp | ISOxx | Timestamp when this place (spot/queue) was created |
|`"PlaceId"`|| integer_64| Unique ID for the entire Spot Service that identifies a Place (Spot, primary queue, staging queue, …)  All places share the same ID space, this is to minimize the risk of bugs due to managing different ID spaces for the different child classes. further 2^64 is wide enough to accommodate all the places.|
|`"Latitude"`||float| position on earth in LLE using WGS84.  NOTE: LLE can change if the place is re-positioned by the operator.  The float will typically require 7 decimals to reach centimeter level of resolution. |
|`"Longitude"`||float||
|`"Elevation"`||float||
|`"Heading"`||integer|Compass heading in degrees of the front of the spot.  Where the vehicle's front should point when spotted|
|`"PlaceIO"`||enum| How to Ingress & Egress that place See enum. |
|`"Origin"`||enum|How to interpret the point served by the spot service in reference to the truck’s origin frame of reference|
|`"DynamicPathId "`||integer<br>`nullable`|Optionally, but expected, to be set by the AHS when a dynamic path that leads to this place is computed in the free space of the area. Semantic null=”Not Set” = Initial state, 0=”Unreachable”,  >0 = greater than 0 means valid path computed in the free space of the area.|
|`"ServiceMaxUtilization"`||integer<br>`nullable`| The number of times this place can be used before it needs to be “reset” by an external event. Can be set to INFINITE via “null”.  Null means that there is no Max Utilization set.  Dispatching can use this to know how many future trucks can be sent there. Useful for loading and dumping, especially paddock (set=1).  Setting this value (writing), even to the same value, will reset the Service Count below.  NOTE: Writing to this value is not the proper way to reset the counter, there is an API to reset it back to zero.|
|`"PlaceState"`||enum| {Closed, Open, Pending, PendingDelete}  This state is User controlled through user requests. Pending = Pending close and waiting that the place is freed before going into closed.  PendingDelete: same but for delete.|
|`"ChangeSequence"`|||a sequence number that is incremented each time this place (and derivations) changes |
|`"ServicingVehicleGUID"`||integer<br>`nullable`|Equipment that has reserved or is there at the moment. Note: for queues the spot server only places the first vehicle here. null= No vehicle utilizing this resource|
|`"ServiceCount"`||||
