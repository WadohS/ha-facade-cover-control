# Configuration guide

## Concept

Create one automation instance per facade/exposure.

For each instance, select:

- the covers on this facade;
- a binary sensor that is `on` when the facade is in direct sun;
- a weather entity that supports `weather.get_forecasts` with `type: daily`;
- opening times;
- hot day threshold;
- sunset close offsets.

## Direct sun sensor

The blueprint expects a binary sensor:

```text
on  = sun is hitting this facade
off = sun is no longer hitting this facade
```

You may create this sensor with Home Assistant templates using `sun.sun` azimuth/elevation, or with any other integration.

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

When the direct sun sensor turns `off`, the blueprint can reopen the covers:

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
