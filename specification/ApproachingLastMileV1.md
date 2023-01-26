# ApproachingLastMileV1

|key|value|format
|---|-----|-----|
| "VehicleId" | VehicleID | UUID |
| "FromWayId" | WayID | integer |

## To Equipment Use Case:
A truck is determined by AHS to be the next vehicle to enter the primary queue, as it’s getting close to the entrance of a loading area and wants to know if it needs to queue and where to spot at the shovel.  It supplies the Ingress WayId as some large area may have more than a single entrance.  Yes this is redundant in most cases, but also serves as a validation layer instead of the Spot service assuming where the truck is coming from.




# Example

```
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2023-01-23T09:30:10.435Z",
  " ApproachingLastMileV1": 
  {
	"VehicleId": "52121756-ff73-4257-b59b-6a96708a5d42",
	"FromWayId": 731854,
	"ToType": "Equipment",
	"EquipmentId": "c20f2649-fefe-49b3-a6af-db5f337d1006"
  }
}


```