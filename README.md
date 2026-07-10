# ha-climate-cover-control

<p align="center">
  <strong>Climate Cover Control</strong><br>
  Home Assistant blueprint for sun-aware and heat-aware facade cover/shutter automation.
</p>

<p align="center">
  <a href="README.fr.md"><strong>🇫🇷 Français</strong></a>
  &nbsp;·&nbsp;
  <a href="README.en.md"><strong>🇬🇧 English</strong></a>
</p>

<p align="center">
  <a href="https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://raw.githubusercontent.com/WadohS/ha-climate-cover-control/main/blueprints/automation/climate_cover_control_fr.yaml">
    <img alt="Importer le blueprint français" src="https://my.home-assistant.io/badges/blueprint_import.svg">
  </a>
  <a href="https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://raw.githubusercontent.com/WadohS/ha-climate-cover-control/main/blueprints/automation/climate_cover_control_en.yaml">
    <img alt="Import English blueprint" src="https://my.home-assistant.io/badges/blueprint_import.svg">
  </a>
</p>

---

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

## Documentation

| Français | English |
|---|---|
| [Documentation](README.fr.md) | [Documentation](README.en.md) |
| [Guide de configuration](docs/configuration.fr.md) | [Configuration guide](docs/configuration.en.md) |
| [Feuille de route](docs/roadmap.fr.md) | [Roadmap](docs/roadmap.en.md) |

## License

MIT
