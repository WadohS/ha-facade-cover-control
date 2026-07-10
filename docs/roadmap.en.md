# Roadmap / ideas

## Near term

### Debug / logbook mode

Add an optional debug/logbook mode to make real-world testing easier.

Ideas:

- `enable_logbook: true`
- log forecast max, hot-day decision, selected opening time, close offset, facade sun state;
- log skipped actions, for example:
  - morning opening skipped because `hot_day=true`;
  - hot-day reopen skipped because facade still sunny;
  - evening close skipped because already closed;
- optionally write the same debug data to the status helper.

Example messages:

```text
Hot day detected: forecast_max=35.0, threshold=25.0. Morning opening skipped.
Facade sun cleared. Reopening covers to 40%.
Closing covers: sunset offset for July is +45 min.
```

### Weather / forecast flexibility

The current blueprint uses `weather.get_forecasts` with `type: daily` and reads the first forecast item's `temperature` as today's maximum.

Future options:

- allow a dedicated external sensor for today's max temperature;
- support multiple weather providers with different forecast semantics;
- add a `forecast_temperature_mode` option:
  - `daily_forecast_temperature`
  - `external_sensor`
- store the selected source and value in the status helper for easier debugging.

This would help users whose weather entity exposes current temperature more clearly than forecast max, or whose daily forecast format differs.

### Integrated solar window

Implemented in `0.1.2`: allow use of `sun.sun` azimuth/elevation directly instead of requiring a direct-sun binary sensor.

Goal: make the blueprint easier to use without creating a separate template binary sensor for every facade.

Inputs:

- `sun_detection_mode`: `direct_sensor`, `solar_window`, or `direct_sensor_or_solar_window`
- `facade_azimuth`: perpendicular direction of the facade/wall in degrees from North
- `solar_window_before`: degrees subtracted from facade azimuth
- `solar_window_after`: degrees added to facade azimuth
- `solar_elevation_min`

Behavior:

```text
facade_sunny =
  direct sensor is on
  OR sun azimuth/elevation is inside the configured facade window
```

This should remain optional because some users may already have precise direct-sun sensors that include shadows/obstacles better than pure azimuth.

## Later

### Dry-run mode

Add a mode that logs what would happen without moving covers.

### Manual override handling

Use the status helper to avoid reopening after a user manually closes covers.

### Night ventilation

Optional, disabled by default. Could partially open selected covers at night for cooling, guarded by home/absence/weather conditions.
