# ApproachingLastMileV2

This message is sent by AHS to enter a specific truck into the last mile dispatching service when the truck is getting close to an open area.

|Sender| Triggered by | Triggers|
|---|---|---|
|`AHS` | Truck approaching the end of haul road <br>and wanting to enter the last mile dispatching | `OccupyPlace` message|

**Note**: the top-level message should contain the `EquipmentId` which is the EquipmentId of the vehicle that owns the ApproachingLastMileV2 message.

## Message attributes

||key |value |format | Description|
|---|---|:---:|:---:|---|
|| `"LastMileId"` | DispatchingId | UUID | A unique ID for this dispatching that will remain the same throught the process of dispatching the truck to the spot and until the truck is released from the Last Mile dispatching  or reports quitting last mile. |
|| `"FromWayId"` | WayID | integer <br/> `nullable` | The ID of the road segment the vehicle will use to enter the open area.  The source of this ID is from the Map, provided by the map service. If the vehicle is inside the target open area (including where this area contains the target equipment or place) this field should be null. |
|| ``"ToType"`` |  oneOf : [`"Equipment"`, `"Area"`, `"Place"`] | enum [`EntityType`](enum_Place.md#entitytype-enumeration) | Identifies the type of entity the vehicle wants to reach. This will be used to identify which of the 3 attributes holds the destination entity ID in the JSON.  At the time of this writing, a Road is not supported.|
|OneOf:||||
|| `"EquipmentId"` | EquipmentId | UUID | Asks the last mile dispatching to find the next available spot served by the vehicle identified by the UUID.  Typically a loading spot at a shovel or digger. This field is only required when the `"ToType"` is `"Equipment"`. |
||`"AreaId"`| WayId| integer| Asks the last mile dispatching to find the next available spot in the Area.  Typically a dumping spot or a parking spot.  This field is only required when the `"ToType"` is `"Area"`. |
||`"PlaceId"`| PlaceId | UUID | Asks the last mile dispatching to send to a specific spot. Typically a parking bay or a fueling bay. This field is only required when the `"ToType"` is `"Place"`. |


## To Equipment Use Case:
AHS has determined that this autonomous truck is getting close to the entrance of a loading area and wants to know if the truck needs to queue and where to spot at the shovel.  AHS supplies the Ingress WayId the truck will use to enter the area. Large areas may have more than a single entrance.  This is always required even if it's redundant when an area has a single entrance, it will serve as a validation for the spot service.

### To Equipment Message Example
```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2023-01-23T09:30:10.435Z",
  "EquipmentId": "52121756-ff73-4257-b59b-6a96708a5d42",
  "ApproachingLastMileV2":
  {
    "LastMileId":"23456756-aa34-5742-9b66-08a5d4294f34",
    "FromWayId": 731854,
    "ToType": "Equipment",
    "EquipmentId": "c20f2649-fefe-49b3-a6af-db5f337d1006"
  }
}
```

## To Area Use case:
The truck was dispatched to an over the edge dump (or paddock) in an open area and is determined by AHS to be the next vehicle to enter the primary queue.  To avoid slowing down as the truck as it is approaching the area, it wants to get its final last mile dispatching to know which queue and spot to use.  In this case the truck does not interact with another heavy vehicle so the truck must “auto-kickout” once it’s done dumping.

### To Area Message Example
```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2018-10-31T09:30:10.435Z",
  "EquipmentId": "e4de3723-a315-4506-b4e9-537088a0eabf",
  "ApproachingLastMileV2":
  {
    "LastMileId":"23456756-aa34-5742-9b66-08a5d4294f34",
    "FromWayId": 731854,
    "ToType": "Area",
    "AreaId": 731889
  }
}
```

> NOTE: We may want to change the auto-kickout to a kickout specified by the Spot service to prevent traffic deadlock, but typically traffic deadlock is not a problem for exiting the dumping area.  Doing this would also make the messaging pattern symmetric for loading and dumping, but symmetry alone does not feel like a good excuse to implement this.


## To Place Use case:
A dispatcher is requested by maintenance to park a specific truck (that has an alarm) into a specific maintenance bay (#03) in the maintenance parking.  The FMS has sent a route and a specific PlaceID to the AHS instead of just a general “parking area”.  So maintenance knows that the truck will be exactly in maintenance bay #03 and won’t have to search in the parking lot.

### To Spot Message Example
```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2018-10-31T09:30:10.435Z",
  "EquipmentId": "e4de3723-a315-4506-b4e9-537088a0eabf",
  "ApproachingLastMileV2":
  {
    "LastMileId":"23456756-aa34-5742-9b66-08a5d4294f34",
    "FromWayId": 731854,
    "ToType": "Place",
    "PlaceId": "6f7e3723-a315-4506-b4e9-537088a0eabf"
  }
}
```
