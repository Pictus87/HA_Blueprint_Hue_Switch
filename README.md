# Hue Dimmer & Wall Switch Blueprints

Home Assistant automation blueprints for the **Philips Hue Dimmer Switch** (4 buttons) and the **Philips Hue Wall Switch Module** (2 paddles, Left/Right) — connected via the official Hue integration (event entities, requires Home Assistant **2024.8.0+**).

Each button/paddle lets you assign a free action per event type (`initial_press`, `repeat`, `short_release`, `long_press`, `long_release`) — dimming, covers, time-based logic, whatever you need. Each button/paddle also has an optional **Scene Cycler**: tap to advance to the next scene, and after a short pause (2.5s) the counter resets back to scene 1.

## Requirements

- Home Assistant 2024.8.0 or newer
- The official Philips Hue integration (`event`-domain entities — not a Zigbee2MQTT/ZHA/deCONZ pairing)
- For the Scene Cycler: one `input_number` helper per button/paddle that uses it (Settings → Devices & Services → Helpers → Add Helper → Number; Minimum 0, Maximum = number of your scenes, Step 1)

> ⚠️ `short_release` arrives noticeably delayed — this is caused by the Hue Bridge's own rate limit (~1 event per device per second over its external EventStream API) and can't be sped up while staying on the Hue Bridge integration.

---

## 🎛️ Hue Dimmer Switch (4 buttons)

Buttons: On, Brighter, Darker, Off.

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FPictus87%2FHA_Blueprint_Hue_Dimmer-Switch%2Fblob%2Fmain%2Fblueprints%2Fhue_dimmer_remote.yaml)

File: [`blueprints/hue_dimmer_remote.yaml`](blueprints/hue_dimmer_remote.yaml)

### Setup

1. Click the import button above (opens your Home Assistant instance and pre-fills the blueprint import) — or import manually under Settings → Automations & Scenes → Blueprints → Import Blueprint → paste the file URL.
2. Create a new automation from the "Hue Dimmer Remote" blueprint.
3. For each of the 4 buttons, pick the matching event entity (`event.xxx_button_1` … `_button_4` — the exact name depends on your device, check Developer Tools → States).
4. For each button, fill in the actions for whichever event types you care about. Leave fields you don't need empty.
5. Optional: fill in the Scene Cycler per button (scene list + its `input_number` helper). Once the scene list is filled in, that button's "Action on: short_release" is ignored.

---

## 🎛️ Hue Wall Switch Module (2 paddles)

Paddles: Left, Right.

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FPictus87%2FHA_Blueprint_Hue_Dimmer-Switch%2Fblob%2Fmain%2Fblueprints%2Fhue_wall_switch.yaml)

File: [`blueprints/hue_wall_switch.yaml`](blueprints/hue_wall_switch.yaml)

### Setup

Same steps as the Dimmer Switch above, just with 2 sections (Left/Right) instead of 4.

> ⚠️ This blueprint assumes the Wall Switch Module reports the same `event_type` values as the Dimmer Switch. Before relying on `repeat`/`long_press`/`long_release`, check Developer Tools → States to confirm which values your paddle's event entity actually reports while held.

---

## Repo layout

```
.
├── README.md
└── blueprints/
    ├── hue_dimmer_remote.yaml
    └── hue_wall_switch.yaml
```

If you place the files elsewhere in the repo, update the `blueprint_url` parameter in both import buttons above to match.
