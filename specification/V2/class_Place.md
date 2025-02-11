# Objects included in messages
Last mile assignments are made up of a sequence of places that the vehicle must visit to complete its task.  These places are either spots or queues.  A spot is the terminus of the assignment and is the last place that the AHT needs to interact with to complete its assigned task.  A queue is a place that the vehicle must visit in a specific sequence before visiting the spot.  The vehicle must visit the queues in the order of the sequence number prior to visiting the spot.

- [`Place`](#place)
- [`Queue`](#queue)
- [`Spot`](#spot)


# Place
A `Place` is a virtual class containing all the shared attributes of these children classes:
- spot
- queues

You will never encounter a place object in the messages, but other concrete classes will reference this definition.

| Field         | Type     | Description                                      |
|---------------|----------|--------------------------------------------------|
| PlaceId       | UUID   | Unique identifier for the place            |
| Pose          | Object   | Contains the position details of the place |
| Pose.Latitude | Float    | Latitude coordinate of the place                 |
| Pose.Longitude| Float    | Longitude coordinate of the place                |
| Pose.Elevation| Float    | Elevation of the place in meters                 |
| Pose.Heading  | Integer  | Heading direction in degrees                     |
| Tolerance.Linear | Float | OPTIONAL accepted linear tolerance in meters between specified position and the true position of the specified origin of the vehicle. |
| Tolerance.Angular | Float | OPTIONAL accepted angular tolerance in degrees between specified heading and the true heading of the vehicle.  |
| PlaceIO       |Enum [`PlaceIO`](enum_Place.md#placeio-enumeration)| Input-Output operation of the place     |
| Origin        |Enum [`Origin`](enum_Place.md#origin-enumeration)| Origin of the place                        |

### Queue
A Queue is a superset of Place that also describes the sequence number in which the queue point should be visited. Vehicles will visit the queues in the order of the sequence number prior to visiting the spot.

| Field         | Type     | Description                                      |
|---------------|----------|--------------------------------------------------|
| include "[Place](#Place)" | N/A      | Reference "[Place](#Place)"          |
| Sequence      | Integer  | Sequence number of the queue point                |



### Spot
A spot is a superset of a place that also describes the task that needs to be performed at the spot. The Place Also represents the terminus of the assignment, and is the last place that the AHT needs to interact with to complete its assigned task.

| Field         | Type     | Description                                      |
|---------------|----------|--------------------------------------------------|
| include "[Place](#Place)" | N/A      | Reference "[Place](#Place)"          |
| Task          | String   | Task to be performed at the spot                 |
