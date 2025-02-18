# Message Headers

This document records the header attributes that will be at the top-level of the spot messages.

## Message headers
| key | value | format | required | description |
| --- |:---:|:---:|:---:| --- |
| `"Protocol"` | `"Open-Autonomy"` | string | True | The protocol of the messages. |
| `"Version"` | 2 | integer | True | The version of the protocol. |
| `"Timestamp"` | DateTime | ISO 8601 | True | The date-time of when the message is sent in ISO 8601 format. |
| `"EquipmentId"` | EquipmentId | UUID | False | The UUID identifying the vehicle defined in the fleet definition, indicating the equipment that the message is intended for or from. This shall be in all messages except for ServiceIdentification |
| `"CorrelationId"` | CorrelationId | UUID | False | The UUID identifying the relations between messages. `CorrelationId` shall be included for messages of request-response nature.

### Message Headers Examples
Message headers for ApproachingLastMileV2 message
```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2018-10-31T09:30:10.435Z",
  "EquipmentId": "e4de3723-a315-4506-b4e9-537088a0eabf",
  "ApproachingLastMileV2": {...}
}
```

Message headers for LeavePlaceV2 message
```JSON
{
  "Protocol":"Open-Autonomy",
  "Version": 2,
  "Timestamp": "2018-10-31T09:30:10.435Z",
  "EquipmentId": "e4de3723-a315-4506-b4e9-537088a0eabf",
  "CorrelationId": "ee46a0c7-ec8f-464c-be5c-ae72dd3d5ede",
  "LeavePlaceV2": {...}
}
```
