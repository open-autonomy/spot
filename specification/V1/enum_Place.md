# enum Place

Here is a list of all the enumerations we use  
- [enum Place](#enum-place)
  - [PlaceIO enumeration](#placeio-enumeration)
  - [Task enumeration](#task-enumeration)
  - [Origin enumeration](#origin-enumeration)
  - [PlaceState enumeration](#placestate-enumeration)
  - [SpotState enumeration](#spotstate-enumeration)
  - [QueueState enumeration](#queuestate-enumeration)
  - [EntityType enumeration](#entitytype-enumeration)

<br>

## PlaceIO enumeration
PlaceIO is used to specify how the vehicle is expected to enter and exit the spot.

|Enumeration | Description|
|---|-----|
|`"PullThrough"` |Drive forward in and out |
|`"BackIn"` | Reverse into it, forward to exit|
|`"BackOut"` | Forward into it, Reverse out of it|
|`"BackThrough"` | Reverse into it, Reverse out of it|

<br>

## Task enumeration
Task is used to specify what the vehicle is expected to do after it has spotted.  Queue points can not posses a task.

|Enumeration | Description|
|---|---|
|`"Load"` |Owned by an equipment (shovel, excavator, Loader) |
|`"DumpCrusher"` |Owned by an equipment (crusher, requires start(/stop) dumping) |
|`"DumpPaddock"` |Owned by an area |
|`"DumpOverEdge"` |Owned by an area |
|`"Fuel"` |Owned by an equipment (fuel truck or fuel island) |
|`"Park"` |Owned by an area |
|`"Wait"` |Owned by an area or equipment. (requires kickout to stop wait) |

<br>

## Origin enumeration
What part of the truck should be positioned / aligned with the designed spot.

|Enumeration | Description|
|---|-----|
|`"Front"` | Front center of the "bumper"|
|`"Load"` |Where the bucket of the first load should be centered in the tray|
|`"Axle"` |The middle of the rear axle (closest axle to the rear of the truck)|
|`"Rear"` |Self-explanatory, typically the rear most part of the tray.|
|`"Pile"` |After dumping, it is where the center of the dirt pile should be.|

> NOTE Any Origin can be used for any type of spot.  But typically these are the normal combinations

These are TYPICAL spot types configurations at a mine site.  But any configuration is possible with any spot type.

|Origin	|Load	|Dump <br>Crusher	|Dump <br>Paddock	|Dump <br>OverEdge	|Fuel	|Park	|Wait|
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Front|:x:	|:x:	|:x:	|:x:	| :white_check_mark:	|:white_check_mark:	|:white_check_mark:|
|Load	|:white_check_mark:|:x:|:x:|:x:|:x:|:x:|:x:|
|Axle|:x:|:white_check_mark:|:x:|:white_check_mark:|:white_check_mark:|:white_check_mark:|:white_check_mark:|
|Rear |:x:|:x:|:x:|:x:|:x:|:x:|:x:|
|Pile	|:x:|:x:|:white_check_mark:|:x:|:x:|:x:|:x:|

<br>

## PlaceState enumeration   

|Enumeration | Description|
|---|-----|
|`"Closed"` |user decided that they don’t want vehicles here at the moment. temporarily, but they intend to re-open later|
|`"Open"` |user wants vehicles to come here now |
|`"PendingClosed"` |user no longer wants vehicles here, but it’s being used right now|
|`"PendingDelete"` |user will no longer use this spot, but it’s being used right now|

<br>

## SpotState enumeration         

|Enumeration | Description|
|---|-----|
|`"Available"`| This spot is available but is not the next one to be dispensed by the /spot service, there is another spot servicing this area that will be used before.  A dispatcher could send a truck to that particular spot that is available.  This is useful to park a particular truck in a particular maintenance spot and have someone dispatched to that location without having to hunt for that truck.|
|`"Next"`| This is a spot that is open and available and it will be the next one to be dispensed by the /spot service to a truck requesting *Any* open spot in that destination.  There will only be one NEXT spot per [Parent&type] that is servicing a destination.  e.g. This means that if there are over the edge spots and paddock spots in one area, then there will be 2 spots in the Next substate, one of the over the edge type and one of the paddock dumping type. When multiple spots are open to serve a single source, then it’s useful to know which one will be used next.|
|`"Reserved"`|A vehicle has requested exclusive usage of this spot and the /spot server has granted it to that vehicle.  But the vehicle is on its way there and has not stopped at that location yet.  This also indicates that no other vehicle can use that spot.|
|`"Full"`|A vehicle is immobilized in that spot and is doing executing its task there.  When the truck’s task is to interact with another vehicle, Full also indicates to the other vehicle (like a loading equipment) that the truck is ready and won’t move until it is told to do so.  The spot will remain full until the truck releases this resource.  This also indicates that no other vehicle can use that spot or will be successful at reserving it.|
|`"Exhausted"`|This is an optional state that is used to automatically “close” a spot until it gets “replenished” by an external agent.  This is useful for over the edge dumping where after a set number of dumps, you’d want a dozer to go cleanup, push material and renew the berm (windrow).  The same method could be used for paddock dumping and set a single usage for each dumping point, until a dozer flattens the new lift.  It could also be used by a shovel operator to constrain the number of trucks that the FMS will be dispatched to it before a long tram.  This is a user controlled way to tell the overall system (FMS, AHS, Spot) to expect a delay before the re-opening of that spot and use other spots in the meantime, without having a person continuously glued to a screen and closing spots just in time after a truck has used it for a set amount of time.|



<br>

## QueueState enumeration         

|Enumeration | Description|
|---|---|
|``"Available"``| There is still space in this queue|
|``"Full"``| The queue is full, there are at least as many trucks queued there than the configured capacity of the queue.  Full is normal for Staging queues, but if a primary queue is full, then the trucks are not dispatched optimally from the FMS or the shovel might be over trucked.|

<br>

## EntityType enumeration

|Enumeration | Description|
|---|---|
|`"Equipment"`| A vehilce, typically defined in the `FleetDefinition` message|
|`"Area"`| A closed WayID representing an area that is defined in the map provided by the map service|
|`"Road"`| A open WayID representing a road segment that is defined in the map provided by the map service|
|`"Place"`| A spot or queue representing a point of interest for trucks in the mime.  This element is provided by the spot service|
