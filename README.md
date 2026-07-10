# ha-facade-cover-control

Home Assistant blueprint for **sun-aware and heat-aware facade cover control**.

This blueprint manages covers/shutters by facade or exposure. It is designed for homes where each facade receives sun at different times and where covers should behave differently during hot days.

## Features

- Control multiple covers as one facade group.
- Block morning opening on hot days based on the daily forecast maximum temperature.
- Reopen after the sun no longer hits the facade.
- Optional integrated solar window using `sun.sun` azimuth/elevation, so a separate direct-sun binary sensor is not required.
- Optional partial reopening after a hot day sun block.
- Per-day opening times.
- Optional workday, holiday, vacation, and absence/security sensors.
- Evening closing based on sunset with configurable monthly offsets.
- Local Home Assistant automation; no cloud dependency besides your configured weather provider.

## Blueprint

Blueprint file:

```text
blueprints/automation/facade_cover_control.yaml
```

## Basic idea

Normal day:

```text
open at the configured time, respecting sunrise if enabled
close at sunset + monthly offset
```

Hot day:

```text
keep covers closed in the morning
wait until the facade is no longer in direct sun
then reopen partially or fully depending on configuration
close at sunset + monthly offset
```

## Import into Home Assistant

Click the button below to import the blueprint directly from GitHub:

[![Open your Home Assistant instance and import the blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://raw.githubusercontent.com/WadohS/ha-facade-cover-control/main/blueprints/automation/facade_cover_control.yaml)

Manual import URL:

```text
https://raw.githubusercontent.com/WadohS/ha-facade-cover-control/main/blueprints/automation/facade_cover_control.yaml
```

You can also copy the blueprint file manually to:

```text
/config/blueprints/automation/facade_cover_control.yaml
```

Then create a new automation from the blueprint in Home Assistant.

## Recommended first test

Start with one facade only, for example kitchen/dining room:

```text
cover.volet_cuisine
cover.volet_salle_a_manger
```

Use either a facade direct-sun binary sensor such as:

```text
binary_sensor.facade_est_sud_sun_direct
```

or use the integrated solar window. Example for a South-East facade whose perpendicular wall direction is 145°:

```yaml
sun_detection_mode: solar_window
facade_azimuth: 145
solar_window_before: 65   # 145 - 65 = 80° window start
solar_window_after: 75    # 145 + 75 = 220° window end
solar_elevation_min: 3
```

Disable any other automation controlling the same covers during the test to avoid conflicts.

## Status

Current version: `0.1.2`

This is an early version. Test on a limited set of covers before deploying widely.

## Documentation

- [Configuration guide — English](docs/configuration.en.md)
- [Roadmap / ideas — English](docs/roadmap.en.md)
- [Guide de configuration — Français](docs/configuration.fr.md)
- [Feuille de route / idées — Français](docs/roadmap.fr.md)
- [README français](README.fr.md)

## License

MIT
