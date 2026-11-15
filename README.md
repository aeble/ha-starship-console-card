# Starship Console

![Screenshot](images/starship-console-card.png)

A console in the style of 90s science-fiction shows: black background, rounded colour bars, elbows, narrow capitals and a pseudo stardate.

The column of numbers at the top left shows real values as codes and scrolls one line per second: `PWR` power, `UT` uptime (sensor from the Uptime integration, or set `uptime:`), `NR` unavailable entities, `LGT` lights on, `EXT`/`RH` outdoors, `KSL` boiler, `GAS` gas today, `DL`/`UL` network, `BAT` low batteries, `LQ` link quality of all `*_linkquality` sensors (weakest first).

- Environment section with segment bars for every room.
- Lights as pill buttons.
- Engineering with heating, gas and power, red alert bar while the burner runs.
- Sensors with outdoor readings and network throughput, communications with departures, people, contacts and batteries.

The card pays homage to a design style. It contains no names, logos or terms from any particular show.

## Configuration

All cards share the same keys. Anything missing is hidden or shown as "–".

```yaml
type: custom:starship-console-card
title: Home
outside_temperature: sensor.outdoor_temperature
outside_humidity: sensor.outdoor_humidity
weather: weather.forecast_home        # condition, and fallback for the outdoor temperature
flame: binary_sensor.burner_flame     # heating, optional
boiler: sensor.boiler_temperature
hot_water: sensor.hot_water_temperature
gas_today: sensor.gas_today
gas_cost: sensor.gas_cost_today
power: sensor.house_power             # W
download: sensor.wan_download         # optional, converted to Mbit/s whatever the unit
upload: sensor.wan_upload
battery_threshold: 20                 # battery sensors below this are reported
idle_timeout: 300                     # idle screen after this many seconds without touch (omit = off)
idle_wake:                            # optional, keeps the dashboard awake while active
  - binary_sensor.kitchen_presence
rooms:
  - name: Living room
    temperature: sensor.living_room_temperature
    humidity: sensor.living_room_humidity
    co2: sensor.living_room_co2           # optional, ppm; yellow from 1000, red from 1200
    pm25: sensor.living_room_pm25         # optional, µg/m³; yellow from 15, red from 35
lights:
  - name: Kitchen
    entity: light.kitchen
departure:                            # optional, sensor holding the timestamp of the next departure
  name: Bus 12 › Centre
  entity: sensor.next_departure
  delay: sensor.delay
contacts:                             # optional, windows and doors
  - name: Bathroom window
    entity: binary_sensor.bathroom_window
```

The idle screen puts a black layer with a faint clock over the dashboard; a tap wakes it without switching anything underneath. A browser cannot dim the tablet's backlight itself. While idle, all animations pause and the card redraws only every 30 seconds, which helps older tablets.

People come from all `person.*` entities, low batteries from all sensors with `device_class: battery`.

The text shown on the cards is German.

## Installation

Via HACS as a custom repository (type Dashboard), or by hand: copy `starship-console-card.js` to `/config/www/` and add `/local/starship-console-card.js` as a JavaScript module under Settings → Dashboards → Resources.

A discreet signature sits in the bottom right corner; `signature: false` hides it.

The card is meant for a panel view (`panel: true`) so it fills the screen.

```yaml
views:
  - title: Home
    panel: true
    cards:
      - type: custom:starship-console-card
        # configuration as above
```

The fonts are embedded in the card file; the card loads nothing from external servers.

## License

Apache License 2.0, copyright 2026 Axel Eble. If you redistribute or modify the card, include `LICENSE` and `NOTICE`.


---

## Deutsch

### Raumschiff-Konsole

Konsole im Stil von Science-Fiction-Serien der 90er: schwarzer Grund, abgerundete Farbbalken, Ellbogen, schmale Versalien und eine Pseudo-Sternzeit.

Die Zahlenkolonne links oben zeigt echte Werte als Codes und rollt jede Sekunde eine Zeile weiter: `PWR` Leistung, `UT` Uptime (Sensor der Integration „Uptime“, sonst `uptime:` angeben), `NR` nicht erreichbare Entitäten, `LGT` eingeschaltete Lampen, `EXT`/`RH` außen, `KSL` Kessel, `GAS` Gas heute, `DL`/`UL` Netz, `BAT` schwache Batterien, `LQ` Linkqualität aller `*_linkquality`-Sensoren (schwächste zuerst).

- Umweltkontrolle mit Segmentbalken für jeden Raum.
- Beleuchtung als Pillentasten.
- Maschinenraum mit Heizung, Gas und Leistung, roter Alarmbalken bei laufendem Brenner.
- Sensoren mit Außenwerten und Netzdurchsatz, Kommunikation mit Abfahrten, Personen, Kontakten und Batterien.

Die Karte ist eine Hommage an einen Gestaltungsstil. Sie enthält keine Namen, Logos oder Begriffe aus einer bestimmten Serie.

#### Konfiguration

Alle Karten teilen sich dieselben Schlüssel. Was fehlt, wird ausgeblendet oder als „–“ angezeigt.

```yaml
type: custom:starship-console-card
title: Haus
outside_temperature: sensor.aussen_temperatur
outside_humidity: sensor.aussen_feuchte
weather: weather.forecast_home        # Zustand und Ersatz für die Außentemperatur
flame: binary_sensor.brenner_flamme   # Heizung, optional
boiler: sensor.kesseltemperatur
hot_water: sensor.warmwassertemperatur
gas_today: sensor.gas_heute
gas_cost: sensor.gaskosten_heute
power: sensor.hausleistung            # W
download: sensor.wan_download         # optional, Einheit wird nach Mbit/s umgerechnet
upload: sensor.wan_upload
battery_threshold: 20                 # Batteriesensoren darunter werden gemeldet
idle_timeout: 300                     # Ruhemodus nach so vielen Sekunden ohne Berührung (weglassen = aus)
idle_wake:                            # optional, hält das Dashboard wach, solange aktiv
  - binary_sensor.kueche_praesenz
rooms:
  - name: Wohnzimmer
    temperature: sensor.wohnzimmer_temperatur
    humidity: sensor.wohnzimmer_feuchte
    co2: sensor.wohnzimmer_co2            # optional, ppm; gelb ab 1000, rot ab 1200
    pm25: sensor.wohnzimmer_pm25          # optional, µg/m³; gelb ab 15, rot ab 35
lights:
  - name: Küche
    entity: light.kueche
departure:                            # optional, Sensor mit Zeitstempel der nächsten Abfahrt
  name: Linie 3 › Stadt
  entity: sensor.naechste_abfahrt
  delay: sensor.verspaetung
contacts:                             # optional, Fenster und Türen
  - name: Fenster Bad
    entity: binary_sensor.fenster_bad
```

Der Ruhemodus legt ein schwarzes Bild mit schwacher Uhr über das Dashboard; Antippen weckt es, ohne darunter etwas zu schalten. Die Helligkeit des Tablets selbst lässt sich aus dem Browser nicht steuern. Solange er aktiv ist, stehen alle Animationen still und die Karte zeichnet nur alle 30 Sekunden neu; das entlastet ältere Tablets.

Personen kommen automatisch aus allen `person.*`-Entitäten, schwache Batterien aus allen Sensoren mit `device_class: battery`.

#### Einbau

Über HACS als benutzerdefiniertes Repository (Typ Dashboard) oder von Hand: `starship-console-card.js` nach `/config/www/` kopieren und unter Einstellungen → Dashboards → Ressourcen als JavaScript-Modul `/local/starship-console-card.js` eintragen.

Unten rechts steht eine dezente Signatur; `signature: false` blendet sie aus.

Die Karte ist für eine Panel-Ansicht gedacht (`panel: true`), damit sie den ganzen Bildschirm füllt.

```yaml
views:
  - title: Haus
    panel: true
    cards:
      - type: custom:starship-console-card
        # Konfiguration wie oben
```

Die Schriften stecken in der Kartendatei; die Karte lädt nichts von fremden Servern.

#### Lizenz

Apache License 2.0, Copyright 2026 Axel Eble. Wer die Karte weitergibt oder verändert, liefert `LICENSE` und `NOTICE` mit.