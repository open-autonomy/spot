# PlaceCommandV1

|key|value|
|---|-----|
| "Method" | "GET" |
| "Scope" | "/summary" |

Returns the **meta data** of the entire Spot "database".  
In the future, we plan on providing the ability to specify a lower scope to retrieve a subset of the DB. 
> e.g. /Pit1/summary



# Example

```
{
  "Protocol":"Open-Autonomy",
  "Version": 1,
  "Timestamp": "2023-01-23T09:30:10.831Z ",

  "PlaceCommandV1": 
  {
     "Method":"GET",
     "Scope": "/summary"
  }
}

```

