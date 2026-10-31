# Kellerlüftung nach absoluter Feuchte · Cellar ventilation by absolute humidity

[![Blueprint importieren / Import blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fcars1011%2Fha-kellerlueftung%2Fblob%2Fmain%2Fblueprints%2Fautomation%2Fkellerlueftung.yaml)

🇩🇪 [Deutsch](#-deutsch) · 🇬🇧 [English](#-english)

---

## 🇩🇪 Deutsch

Ein Home-Assistant-Blueprint, der einen Keller über motorisierte Fenster entfeuchtet. Er vergleicht die **absolute Luftfeuchte** (g/m³) im Keller mit der Außenluft. Gelüftet wird nur, wenn die Außenluft tatsächlich weniger Wasser enthält, also wenn Lüften den Keller wirklich trockener macht.

### Warum absolute Feuchte?

Die relative Luftfeuchte hängt von der Temperatur ab. Warme Sommerluft mit 50 % relativer Feuchte kann deutlich mehr Wasser enthalten als kühle Kellerluft mit 70 %. Lüftet man dann, kondensiert die Feuchtigkeit an den kalten Kellerwänden, und der Keller wird nasser statt trockener. Die absolute Feuchte vermeidet diesen Fehler.

### Funktionen

- **Zustandsbasiert, ohne Wartezeiten:** Alle 5 Minuten, beim Start von Home Assistant und bei jeder Änderung der Feuchtewerte wird neu entschieden. Neustarts oder Reloads lassen die Fenster nicht offen hängen.
- **Hysterese:** getrennte Schwellen für Öffnen und Schließen, damit die Fenster nicht ständig auf- und zugehen.
- **Frostschutz:** Unter einer Mindesttemperatur außen oder im Keller bleiben die Fenster zu.
- **Sicherheit bei Sensorausfall:** Meldet ein Sensor `unavailable` oder `unknown`, werden die Fenster geschlossen.
- **Optionaler Regensensor.**
- Unterstützt **`switch`- und `cover`-Entitäten** und beliebig viele Fenster.

### Voraussetzungen

- Home Assistant **2024.10** oder neuer
- Je ein Sensor für **absolute Feuchte** (g/m³) im Keller und außen, z. B. aus der Integration [Thermal Comfort](https://github.com/dolezsa/thermal_comfort)
- Je ein Temperatursensor im Keller und außen
- Fensterantriebe, die ihren Zustand zuverlässig melden (`on`/`off` bzw. `open`/`closed`)

### Installation

**Variante 1:** Auf den Button „Blueprint importieren“ oben klicken.

**Variante 2:** In Home Assistant unter *Einstellungen → Automationen & Szenen → Blueprints → Blueprint importieren* diese URL einfügen:

```
https://github.com/cars1011/ha-kellerlueftung/blob/main/blueprints/automation/kellerlueftung.yaml
```

**Variante 3:** Die Datei `blueprints/automation/kellerlueftung.yaml` nach `config/blueprints/automation/kellerlueftung/` kopieren und Blueprints neu laden.

Anschließend eine Automation aus dem Blueprint erstellen und die Sensoren und Fenster auswählen.

### Einstellungen

| Einstellung | Standard | Bedeutung |
|---|---|---|
| Öffnen ab Differenz | 1,0 g/m³ | Um so viel muss der Keller feuchter sein als außen, damit geöffnet wird |
| Schließen unter Differenz | 0,3 g/m³ | Fällt der Unterschied darunter, wird geschlossen (muss kleiner als der Öffnen-Wert sein) |
| Mindest-Außentemperatur | 3 °C | Darunter bleiben die Fenster zu |
| Mindest-Kellertemperatur | 10 °C | Darunter wird nicht weiter gelüftet |
| Regensensor | – | Optional; bei `on` werden die Fenster geschlossen |

### Logik

| Situation | Aktion |
|---|---|
| Keller ≥ *Öffnen-Differenz* feuchter, warm genug, kein Regen | Fenster öffnen |
| Differenz ≤ *Schließen-Differenz* | Fenster schließen |
| Zu kalt (außen oder Keller) | Fenster schließen |
| Regen | Fenster schließen |
| Sensor nicht verfügbar | Fenster schließen |
| Dazwischen | Zustand bleibt unverändert |

### Hinweise

- Fensterantriebe, die nur per **Impuls** angesteuert werden und keinen echten Zustand melden, funktionieren nicht zuverlässig. Die Automation muss erkennen können, ob ein Fenster offen ist.
- Wird für einen anderen Zweck bereits eine Automation auf dieselben Fenster angewendet, sollte diese deaktiviert werden. Sonst arbeiten beide gegeneinander.

---

## 🇬🇧 English

A Home Assistant blueprint that dehumidifies a cellar using motorised windows. It compares the **absolute humidity** (g/m³) in the cellar with the outdoor air. It only ventilates when the outdoor air actually holds less water, i.e. when ventilating really makes the cellar drier.

### Why absolute humidity?

Relative humidity depends on temperature. Warm summer air at 50 % relative humidity can hold considerably more water than cool cellar air at 70 %. Ventilating in that situation makes moisture condense on the cold cellar walls, so the cellar gets wetter instead of drier. Absolute humidity avoids this mistake.

### Features

- **State-based, no delays:** It re-evaluates every 5 minutes, when Home Assistant starts, and whenever the humidity values change. Restarts or reloads never leave the windows stuck open.
- **Hysteresis:** separate thresholds for opening and closing so the windows don't keep flapping.
- **Frost protection:** Below a minimum outdoor or cellar temperature the windows stay closed.
- **Fail-safe on sensor outage:** If a sensor reports `unavailable` or `unknown`, the windows are closed.
- **Optional rain sensor.**
- Supports **`switch` and `cover` entities** and any number of windows.

### Requirements

- Home Assistant **2024.10** or newer
- One **absolute humidity** sensor (g/m³) each for the cellar and outdoors, e.g. from the [Thermal Comfort](https://github.com/dolezsa/thermal_comfort) integration
- One temperature sensor each for the cellar and outdoors
- Window actuators that reliably report their state (`on`/`off` or `open`/`closed`)

### Installation

**Option 1:** Click the "Import blueprint" button at the top.

**Option 2:** In Home Assistant go to *Settings → Automations & Scenes → Blueprints → Import Blueprint* and paste this URL:

```
https://github.com/cars1011/ha-kellerlueftung/blob/main/blueprints/automation/kellerlueftung.yaml
```

**Option 3:** Copy `blueprints/automation/kellerlueftung.yaml` to `config/blueprints/automation/kellerlueftung/` and reload blueprints.

Then create an automation from the blueprint and select your sensors and windows.

### Settings

| Setting | Default | Meaning |
|---|---|---|
| Open at difference | 1.0 g/m³ | The cellar must be this much more humid than outdoors to open |
| Close below difference | 0.3 g/m³ | If the difference drops below this, the windows close (must be lower than the open value) |
| Minimum outdoor temperature | 3 °C | Below this the windows stay closed |
| Minimum cellar temperature | 10 °C | Below this ventilation stops |
| Rain sensor | – | Optional; windows close while it is `on` |

### Logic

| Situation | Action |
|---|---|
| Cellar ≥ *open difference* more humid, warm enough, no rain | Open windows |
| Difference ≤ *close difference* | Close windows |
| Too cold (outdoors or cellar) | Close windows |
| Rain | Close windows |
| Sensor unavailable | Close windows |
| In between | State stays unchanged |

### Notes

- Window actuators driven only by a **pulse** that don't report a real state won't work reliably. The automation needs to know whether a window is open.
- If another automation already controls the same windows, disable it. Otherwise both will fight each other.

---

## Danksagung · Credits

Inspiriert vom Blueprint „GSW: Smart Ventilation Control“ von Iakovos Venieris ([Greece Sky and Weather](https://www.youtube.com/@GreeceSkyandWeather)).
Inspired by the "GSW: Smart Ventilation Control" blueprint by Iakovos Venieris ([Greece Sky and Weather](https://www.youtube.com/@GreeceSkyandWeather)).

## Lizenz · License

[MIT](LICENSE)
