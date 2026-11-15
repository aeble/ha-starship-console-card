# Raumschiff-Konsole

![Screenshot](images/starship-console-card.png)

Konsole im Stil von Science-Fiction-Serien der 90er: schwarzer Grund, abgerundete Farbbalken, Ellbogen, schmale Versalien und eine Pseudo-Sternzeit.

Die Zahlenkolonne links oben zeigt echte Werte als Codes und rollt jede Sekunde eine Zeile weiter: `PWR` Leistung, `UT` Uptime (Sensor der Integration „Uptime“, sonst `uptime:` angeben), `NR` nicht erreichbare Entitäten, `LGT` eingeschaltete Lampen, `EXT`/`RH` außen, `KSL` Kessel, `GAS` Gas heute, `DL`/`UL` Netz, `BAT` schwache Batterien, `LQ` Linkqualität aller `*_linkquality`-Sensoren (schwächste zuerst).

- Umweltkontrolle mit Segmentbalken für jeden Raum.
- Beleuchtung als Pillentasten.
- Maschinenraum mit Heizung, Gas und Leistung, roter Alarmbalken bei laufendem Brenner.
- Sensoren mit Außenwerten und Netzdurchsatz, Kommunikation mit Abfahrten, Personen, Kontakten und Batterien.

Die Karte ist eine Hommage an einen Gestaltungsstil. Sie enthält keine Namen, Logos oder Begriffe aus einer bestimmten Serie.

## Konfiguration

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

## Einbau

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

Schriften kommen von Google Fonts. Ohne Internetzugang fällt die Karte auf Systemschriften zurück.

## Lizenz

Apache License 2.0, Copyright 2026 Axel Eble. Wer die Karte weitergibt oder verändert, liefert `LICENSE` und `NOTICE` mit.
