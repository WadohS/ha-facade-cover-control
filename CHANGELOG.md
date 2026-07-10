# Changelog

## 0.1.2

- Add integrated solar-window sun detection using `sun.sun` azimuth/elevation.
- Make the direct-sun binary sensor optional when solar-window detection is used.
- Add facade-centered configuration: `facade_azimuth`, `solar_window_before`, `solar_window_after`, and `solar_elevation_min`.
- Document that before/after values are degrees subtracted from / added to the facade perpendicular direction.

## 0.1.1

- Add optional `input_text` status helper inspired by CCA.
- Add configurable heat-protection months (default May–September), so high forecast temperatures are ignored outside the warm season for winter solar gains.
- Store daily open, hot-reopen and close actions in the helper to improve persistence across reloads/restarts.
- Use templated `input_text.set_value` data for optional helper writes, avoiding malformed automation saves when no helper is selected.
- Include periodic tick in hot-day after-sun reopening logic so reopening is not missed if the sun sensor changed while the automation was unavailable.

## 0.1.0

Initial blueprint draft.

Features:

- Facade-based cover control.
- Hot day morning opening block based on daily weather forecast maximum temperature.
- Reopen after direct sun leaves the facade.
- Optional partial reopening.
- Per-day opening schedule.
- Optional workday, holiday, vacation and absence sensors.
- Monthly sunset close offsets.
