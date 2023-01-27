# enum Place

Here is a list of all the enumerations we use  
- [`PlaceIO`](#placeio-enumeration)
- [`Task`](#task-enumeration)
- [`Origin`](#origin-enumeration)
- [`PlaceState`](#placestate-enumeration)
- [`SpotState`](#spotstate-enumeration)
- [`QueueState`](#queuestate-enumeration)

<br>

## PlaceIO enumeration
PlaceIO is used to specify how the vehicle is expected to enter and exit the spot.

|Enumeration | Description|
|---|-----|
|``"PullThrough"`` |Drive forward in and out |
|``"BackIn"`` | Reverse into it, forward to exit|
|``"BackOut"`` | Forward into it, Reverse out of it|
|``"BackThrough"`` | Reverse into it, Reverse out of it|

<br>

## Task enumeration
Task is used to specify what the vehicle is expected to do after it has spotted.  Queue points can not posses a task.

|Enumeration | Description|
|---|-----|
|``"Load"`` |Owned by an equipment (shovel, excavator, Loader) |
|``"DumpCrusher"`` |Owned by an equipment (crusher, requires start(/stop) dumping) |
|``"DumpPaddock"`` |Owned by an area |
|``"DumpOverEdge"`` |Owned by an area |
|``"Fuel"`` |Owned by an equipment (fuel truck or fuel island) |
|``"Park"`` |Owned by an area |
|``"Wait"`` |Owned by an area or equipment. (requires kickout to stop wait) |

<br>

## Origin enumeration
What part of the truck should be positioned / aligned with the designed spot.

|Enumeration | Description|
|---|-----|
|``"Front"`` | Front center of the "bumper"|
|``"Load"`` |Where the bucket of the first load should be centered in the tray|
|``"Axle"`` |The middle of the rear axle (closest axle to the rear of the truck)|
|``"Rear"`` |Self-explanatory, typically the rear most part of the tray.|
|``"Pile"`` |After dumping, it is where the center of the dirt pile should be.|

> NOTE Any Origin can be used for any type of spot.  But typically these are the normal combinations

These are TYPICAL spot types configurations at a mine site.  But any configuration is possible with any spot type.

|Origin	|Load	|Dump Crusher	|Dump Paddock	|Dump OverEdge	|Fuel	|Park	|Wait|
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
|``"Closed"`` |user decided that they don’t want vehicles here at the moment. temporarily, but they intend to re-open later|
|``"Open"`` |user wants vehicles to come here now |
|``"PendingClosed"`` |user no longer wants vehicles here, but it’s being used right now|
|``"PendingDelete"`` |user will no longer use this spot, but it’s being used right now|

<br>

## SpotState enumeration         

|Enumeration | Description|
|---|-----|
|``"Available"``||
|``"Next"``||
|``"Reserved"``||
|``"Full"``||
|``"Exhausted"``||



<br>

## QueueState enumeration         

|Enumeration | Description|
|---|-----|
|``"Available"``||
|``"Full"``||
