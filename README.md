# Hue Dimmer & Wall Switch Blueprints

Home Assistant Automation-Blueprints für den **Philips Hue Dimmer Switch** (4 Tasten) und das **Philips Hue Wall Switch Module** (2 Paddles, Links/Rechts) — angebunden über die offizielle Hue-Integration (Event-Entities, benötigt Home Assistant **2024.8.0+**).

Jeder Taste/Paddle lässt sich frei eine Aktion pro Event-Typ zuweisen (`initial_press`, `repeat`, `short_release`, `long_press`, `long_release`) — Dimmen, Rollos, Zeitlogik, was auch immer du brauchst. Optional gibt es pro Taste/Paddle einen **Scene Cycler**: Antippen schaltet zur nächsten Szene, nach einer kurzen Pause (2.5s) springt der Zähler wieder auf Szene 1 zurück.

## Voraussetzungen

- Home Assistant 2024.8.0 oder neuer
- Philips Hue Integration (offiziell, `event`-Domain-Entities — keine Zigbee2MQTT/ZHA/deCONZ-Kopplung)
- Für den Scene Cycler: pro Taste/Paddle, die ihn nutzt, ein `input_number`-Helper (Einstellungen → Geräte & Dienste → Helfer → Helfer hinzufügen → Zahl; Minimum 0, Maximum = Anzahl deiner Szenen, Schrittweite 1)

> ⚠️ `short_release` kommt spürbar verzögert an — das liegt am Rate-Limit der Hue-Bridge selbst (~1 Event pro Gerät pro Sekunde über die externe EventStream-API) und lässt sich innerhalb der Hue-Bridge-Integration nicht beschleunigen.

---

## 🎛️ Hue Dimmer Switch (4 Tasten)

Tasten: On, Brighter, Darker, Off.

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FPictus87%2FHA_Blueprint_Hue_Dimmer-Switch%2Fblob%2Fmain%2Fblueprints%2Fhue_dimmer_remote.yaml)

Datei: [`blueprints/hue_dimmer_remote.yaml`](blueprints/hue_dimmer_remote.yaml)

### Einrichtung

1. Import-Button oben klicken (öffnet deine Home Assistant Instanz und schlägt den Blueprint-Import vor) — oder manuell unter Einstellungen → Automatisierungen & Szenen → Blueprints → Blueprint importieren → URL der Datei einfügen.
2. Neue Automatisierung aus dem Blueprint "Hue Dimmer Remote" erstellen.
3. Für jede der 4 Tasten die passende Event-Entity auswählen (`event.xxx_button_1` … `_button_4` — Name hängt von deinem Gerät ab, siehe Entwicklerwerkzeuge → Zustände).
4. Pro Taste die gewünschten Aktionen für die Event-Typen eintragen, die dich interessieren. Nicht benötigte Felder einfach leer lassen.
5. Optional: Scene Cycler pro Taste befüllen (Szenenliste + zugehöriger `input_number`-Helper). Ist die Szenenliste gefüllt, wird die "Action on: short_release" für diese Taste ignoriert.

---

## 🎛️ Hue Wall Switch Module (2 Paddles)

Paddles: Left, Right.

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FPictus87%2FHA_Blueprint_Hue_Dimmer-Switch%2Fblob%2Fmain%2Fblueprints%2Fhue_wall_switch.yaml)

Datei: [`blueprints/hue_wall_switch.yaml`](blueprints/hue_wall_switch.yaml)

### Einrichtung

Gleicher Ablauf wie beim Dimmer Switch, nur mit 2 statt 4 Sektionen (Left/Right).

> ⚠️ Dieser Blueprint geht davon aus, dass das Wall Switch Module dieselben `event_type`-Werte liefert wie der Dimmer Switch. Vor dem produktiven Einsatz von `repeat`/`long_press`/`long_release` kurz in Entwicklerwerkzeuge → Zustände prüfen, welche Werte deine Event-Entity beim Halten tatsächlich meldet.

---

## Aufbau in diesem Repo

```
.
├── README.md
└── blueprints/
    ├── hue_dimmer_remote.yaml
    └── hue_wall_switch.yaml
```

Falls du die Dateien an anderer Stelle im Repo ablegst, müssen die `blueprint_url`-Parameter in den beiden Import-Buttons oben entsprechend angepasst werden.
