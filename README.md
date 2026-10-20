## HasanRasouliUberTask
## Testing the Ride-Hailing Application

### 1. Register a Rider

```bash
curl -X POST http://localhost/register \
  -H "Content-Type: application/json" \
  -d '{"name": "Carlos", "type": "rider"}'
```

Expected output:

```json
{
  "message": "Rider registered successfully",
  "user_id": 5
}
```

**Note:** The actual `user_id` depends on the existing database records.

### 2. Request a Ride

Use the `user_id` returned by the registration request. Driver simulation is configured for Tehran.

```bash
curl -X POST http://localhost/request_ride \
  -H "Content-Type: application/json" \
  -d '{"rider_id": 5, "latitude": 35.6892, "longitude": 51.3890}'
```

Expected response:

```json
{
  "message": "Ride request received. Searching for a driver.",
  "trip_id": 7
}
```

**Note:** The actual `trip_id` depends on the database state.

### 3. Monitor Driver Assignment

```bash
docker compose logs -f matcher
```

Expected log messages:

```text
[x] Received ride request...
[*] Found nearest driver: Driver-Alpha
[*] Successfully assigned driver Driver-Alpha (ID: 1) to trip ...
```

Press `Ctrl+C` to stop following the logs.

### 4. Check Trip Status

Use the actual `trip_id` returned by the ride request.

```bash
curl http://localhost/trip/7
```

Expected final response:

```json
{
  "driver_id": 1,
  "status": "assigned"
}
```

The driver ID and trip ID may vary depending on the database state. If the trip is initially still `requested`, wait a few seconds and check again.

