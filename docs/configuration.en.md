# Configuration guide

## Concept

Create one automation instance per facade/exposure.

For each instance, select:

- the covers on this facade;
- either a binary sensor that is `on` when the facade is in direct sun, or the integrated solar window settings;
- a weather entity that supports `weather.get_forecasts` with `type: daily`;
- opening times;
- hot day threshold;
- sunset close offsets.

## Optional status helper

The blueprint can use an optional `input_text` as a small persistent status file, inspired by CCA.

Create one helper per facade/automation instance, for example:

```text
input_text.facade_est_sud_cover_status
```

The helper stores JSON with the actions already performed today, such as normal opening, hot-day reopening and evening closing. This helps the automation avoid repeated actions after reloads or restarts.

If no helper is selected, the blueprint still works, but with less persistent state.

The helper also stores the forecast maximum temperature used for the last action (`forecast_max`) and whether the day was considered hot (`hot_day`). This makes debugging wrong weather entities easier.

For heat decisions, prefer a weather entity whose daily forecast temperature is the real daily maximum. In this setup, `weather.tacoignieres` / Météo-France is usually a better candidate than a weather entity whose visible temperature is only the current temperature.

## Direct sun detection

The blueprint supports three direct-sun detection modes:

```text
Direct sensor only
Solar window only
Direct sensor or solar window
```

### Direct sensor

Use this mode if you already have a binary sensor:

```text
on  = sun is hitting this facade
off = sun is no longer hitting this facade
```

You may create this sensor with Home Assistant templates using `sun.sun` azimuth/elevation, or with any other integration. The sensor is optional if you use the integrated solar window.

### Solar window

Solar window mode uses `sun.sun` azimuth and elevation directly. It avoids having to create one binary sensor per facade.

Configure:

```yaml
facade_azimuth: 145
solar_window_before: 65
solar_window_after: 75
solar_elevation_min: 3
```

`facade_azimuth` is the direction perpendicular to the facade/wall, in degrees from North. It is the center of the window, not the start/end.

`solar_window_before` means degrees to subtract from the facade azimuth.

`solar_window_after` means degrees to add to the facade azimuth.

Example:

```text
facade_azimuth = 145°
solar_window_before = 65°  -> window starts at 80°
solar_window_after  = 75°  -> window ends at 220°
```

In that example, the facade is considered sunny when:

```text
sun azimuth is between 80° and 220°
and sun elevation is >= 3°
```

For east-facing or south-east-facing facades that heat early in the morning, increase `solar_window_before` so covers do not reopen too early while morning sun is still strong.

## Hot day logic

The blueprint calls:

```yaml
weather.get_forecasts
```

with:

```yaml
type: daily
```

It reads the first forecast item temperature as today's maximum temperature.

If:

```text
forecast max >= heat threshold
```

then the normal morning opening is skipped.

When the selected sun detection says the facade is no longer sunny, the blueprint can reopen the covers:

- not at all;
- partially;
- fully.

## Opening schedule

You can define an opening time per weekday.

Optional sensors can override this:

1. absence/security sensor;
2. vacation sensor;
3. holiday sensor;
4. workday sensor;
5. per-day schedule.

## Closing schedule

Closing is based on:

```text
sunset + monthly offset
```

Each month has its own configurable offset in minutes.

Example:

```text
winter: close before sunset
summer: close after sunset
```

## First deployment recommendation

Start with one facade and two covers. Disable any other automation controlling the same covers during testing.
