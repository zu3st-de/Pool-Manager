# Pool-Manager für Home Assistant - Dokumentation

## 📋 Übersicht

Das **Pool-Manager Blueprint-System** ist eine umfassende Lösung zur automatisierten Verwaltung einer Poolpumpe mit optionalen Chemie- und Wetterintegration.

### Hauptfunktionen

✅ **Pumpen-Automation**
- Zeitfenster pro Wochentag
- Temperaturbasierte Steuerung (optional)
- PV-Überschuss-Nutzung (optional)
- Manuelle Override-Modi (Badezeit, Extra-Laufzeit)

✅ **Chemie-Management** (Optional)
- Chlor-Wert-Überwachung mit Empfehlungen
- PH-Wert-Überwachung und Berechnung
- Multitab-Dosierer-Steuerung
- Automatische Benachrichtigungen

✅ **Wetter-Integration** (Optional)
- Regenwarnung
- Sonnenschein-basierte Extralaufzeiten
- Unwetter-Abschaltung
- Temperaturvorhersage

---

## 🚀 Installation

### Schritt 1: Blueprint-Dateien installieren

1. Öffne deine Home Assistant Instanz
2. Gehe zu **Einstellungen** → **Automationen & Szenen** → **Blueprints**
3. Klicke auf "Blueprints importieren"
4. Kopiere den Inhalt der `pool_pump_automation.yaml` Datei
5. Wiederhole für alle anderen Blueprint-Dateien

Alternativ: Kopiere alle `*.yaml` Dateien aus dem `blueprints/automation/` Ordner direkt in dein Home Assistant `config/blueprints/automation/` Verzeichnis.

### Schritt 2: Helper konfigurieren

1. Kopiere den Inhalt aus `configuration_example.yaml`
2. Wähle einen der folgenden Wege:
   - **Option A**: Füge den Code zu `configuration.yaml` hinzu
   - **Option B**: Erstelle eine neue Datei `pool_manager.yaml` und referenziere sie in `configuration.yaml`:
     ```yaml
     input_boolean: !include pool_manager/input_boolean.yaml
     input_number: !include pool_manager/input_number.yaml
     input_button: !include pool_manager/input_button.yaml
     automation: !include pool_manager/automations.yaml
     template: !include pool_manager/templates.yaml
     ```

### Schritt 3: Automationen erstellen

1. Gehe zu **Einstellungen** → **Automationen & Szenen**
2. Klicke auf **Neue Automation erstellen**
3. Wähle "Aus Blueprint erstellen"
4. Wähle "Pool Pumpen-Automation" aus
5. Konfiguriere die Parameter (siehe unten)

---

## ⚙️ Konfiguration

### Pool Pumpen-Automation - Erforderliche Parameter

| Parameter | Beschreibung | Beispiel |
|-----------|-------------|---------|
| **Pumpen-Schalter** | Der Switch der Poolpumpe | `switch.pool_pump` |
| **Poolgröße** | Volumen in Litern | `50000` L |
| **Pumpendurchsatz** | Durchsatz in L/h | `10000` L/h |

### Zeitfenster-Konfiguration

Die Pumpe kann für jeden Wochentag unterschiedliche Zeiten haben:

```
Montag - Freitag:    06:00 - 22:00
Samstag - Sonntag:   05:00 - 23:00
```

**Tipps:**
- Längere Laufzeiten an Wochenenden für Badebetrieb
- Frühere Starts bei niedrigen Temperaturen
- Überlappung mit PV-Produktion für Überschussnutzung

### Temperatur-Steuerung (Optional)

Aktivieren unter **Einstellungen** → **Eingabeoptionen** → "Temperatur-basierte Steuerung"

| Parameter | Standard | Bereich |
|-----------|----------|--------|
| Minimale Temperatur | 18°C | 10-25°C |
| Maximale Temperatur | 26°C | 20-35°C |

**Funktion:**
- ≤ 18°C: Pumpe AUS (zu kalt zum Filtern)
- 18-26°C: Pumpe AN (optimal)
- ≥ 26°C: Kann zu Algenbildung führen

### PV-Überschuss (Optional)

Benötigt einen Sensor für verfügbaren Überschuss in Watt.

| Parameter | Standard | Beschreibung |
|-----------|----------|-------------|
| Schwellwert | 2000 W | Ab dieser Leistung läuft die Pumpe zusätzlich |

**Beispiel für Integration:**

Wenn du eine PV-Anlage mit einem Hybrid-Wechselrichter hast:
```yaml
sensor:
  - platform: mqtt
    name: "PV Überschuss"
    state_topic: "solaredge/excess_power"
    unit_of_measurement: "W"
```

---

## 🏊 Verwendung

### Normale Pumpenlaufzeit

Die Pumpe läuft automatisch nach den konfigurierten Zeitfenstern.

**Aktuelle Einstellung zeigen:** Dashboard → Übersicht → Pumpe

### Badezeit-Modus

**Aktivierung:** Drücke den "🏊 Badezeit" Button

**Was passiert:**
- Automation wird für 2 Stunden (konfigurierbar) deaktiviert
- Pumpe kann manuell gesteuert werden
- Nach Ablauf: Automation lädt neu

**Dauer anpassen:** Dashboard → Einstellungen → "Badezeit-Dauer"

### Extra-Laufzeit

**Aktivierung:** Drücke den "⏱️ Extra-Laufzeit" Button

**Was passiert:**
- Pumpe läuft zusätzlich zu normalen Zeiten
- Läuft 1 Stunde (konfigurierbar)
- Ideal für PV-Überschüsse oder Stoßfiltration

---

## 🧪 Chemie-Management

### Aktivierung

1. Gehe zu **Einstellungen** → **Automationen & Szenen**
2. Erstelle neue Automation aus "Pool Chemie-Management" Blueprint
3. Konfiguriere die Sensoren (siehe unten)

### Erforderliche Sensoren

#### Chlor-Sensor
- **Einheit:** ppm (mg/L)
- **Zielbereich:** 1.0 - 3.0 ppm
- **Kritisch:** < 0.5 ppm
- **Beispiel-Integration:**
  ```yaml
  sensor:
    - platform: mqtt
      name: "Pool Chlor"
      state_topic: "pool/sensors/chlorine"
      unit_of_measurement: "ppm"
  ```

#### PH-Sensor
- **Einheit:** PH-Wert
- **Zielbereich:** 7.0 - 7.6
- **Zu sauer:** < 7.0 → PH+ zugeben
- **Zu basisch:** > 7.6 → PH- zugeben

### Automatische Empfehlungen

Das System berechnet automatisch:

**Für Chlor:**
- Menge Chlor-Lösung in ml basierend auf Poolgröße
- Beispiel: 50.000L Pool mit 0,5 ppm Deficit = ~50ml Chlor-Lösung

**Für PH:**
- ml PH+ oder PH- basierend auf Abweichung
- Beispiel: 50.000L Pool mit PH 8.0 = ~100ml PH-

### Multitab-Dosierer-Integration

Falls du einen motorisierten Multitab-Dosierer hast:

```yaml
input_number:
  pool_multitab_position:
    name: Multitab-Position
    unit_of_measurement: "%"
    min: 0
    max: 100
    step: 5
```

Bei kritischem Chlor-Mangel stellt das System ihn auf 100% offen.

---

## 🌦️ Wetterdienst-Integration

### Voraussetzungen

1. Wetterdienst in Home Assistant konfiguriert:
   - OpenWeatherMap
   - Weather.com
   - Lokale Wetterstation

2. Optional: Regen-Sensor (z.B. von Funkwetterstation)

### Funktionen

#### Regenwarnung
- System erkennt aktiven Regen
- Benachrichtigungen an Nutzer
- Optionale Pumpen-Anpassung

#### Unwetter-Abschaltung
- Erkennt Gewitter/Unwetter
- Schaltet Pumpe automatisch ab
- Schützt die Ausrüstung

#### Sonnenschein-Nutzung
- UV-Index Überwachung
- Erhöhte Filtration bei hoher Sonne
- Nutzt PV-Überschuss automatisch

#### Temperaturvorhersage
- Vorhersage für kommende Tage
- Warnt vor extremer Hitze
- Plant Filterlaufzeiten voraus

### Integration einrichten

1. Gehe zu **Einstellungen** → **Geräte und Dienste** → **Integrationen**
2. Suche nach deinem Wetterdienst (z.B. "OpenWeatherMap")
3. Installiere und konfiguriere
4. Notiere die Entity-ID (z.B. `weather.home`)

---

## 📊 Dashboard verwenden

Das System beinhaltet ein vorkonfiguriertes Dashboard mit:

### 📱 Übersicht-Tab
- Pumpen-Status
- Aktuelle Werte (Temperatur, Chlor, PH)
- Schnell-Aktionen (Badezeit, Extra-Laufzeit)

### 🧪 Chemie-Tab
- Chlor- und PH-Wert-Gauges
- Automatische Empfehlungen
- Multitab-Dosierer-Position

### 🌦️ Wetter-Tab
- Wetter-Vorhersage
- UV-Index
- Regen-Sensor

### 📈 Statistiken-Tab
- Laufzeit heute
- Umwälzrate
- Nächste geplante Aktivierung

### ⚙️ Einstellungen-Tab
- Zieltemperatur
- Min. tägliche Laufzeit
- Pumpen-Modus

---

## 🔧 Fehlerbehebung

### Problem: Pumpe startet nicht automatisch

**Lösungen:**
1. Prüfe, ob der Schalter korrekt konfiguriert ist
2. Überprüfe die Automation in **Einstellungen** → **Automationen & Szenen**
3. Prüfe die Trigger-Zeiten (korrekte Uhrzeit eingestellt?)
4. Schaue in die Log-Datei: **Einstellungen** → **Protokolle** → **Automation**

### Problem: Badezeit funktioniert nicht

**Lösungen:**
1. Prüfe, ob `input_boolean.pool_bath_time_active` existiert
2. Button muss in der Automation als Entity konfiguriert sein
3. Überprüfe im Dashboard, ob Wert runterzählt

### Problem: Chemie-Empfehlungen funktionieren nicht

**Lösungen:**
1. Prüfe die Entity-IDs der Sensoren
2. Stelle sicher, dass Sensoren Werte zurückgeben (nicht "unknown")
3. Überprüfe die Automation auf Fehler

### Problem: Keine Benachrichtigungen

**Lösungen:**
1. Stelle sicher, dass Benachrichtigungs-Service konfiguriert ist
2. Teste mit: **Einstellungen** → **Geräte und Dienste** → **Services**
3. Suche nach "notify" und teste den Service

---

## 📝 Beispiel-Szenarien

### Szenario 1: Kleine Familie, kein PV, keine Chemie

```yaml
# Nur Basis-Automation
Poolgröße: 30.000 L
Pumpendurchsatz: 8.000 L/h
Zeitfenster: 08:00-22:00 täglich
Temperatur: optional
```

**Voraussetzungen:**
- 1 Pumpen-Schalter
- Badezeit & Extra-Laufzeit Buttons

### Szenario 2: Mittelgroßer Pool mit PV-Nutzung

```yaml
Poolgröße: 50.000 L
Pumpendurchsatz: 10.000 L/h
Zeitfenster: 06:00-22:00
Temperatur-Sensor: aktiv
PV-Überschuss: aktiv (Schwellwert 2000W)
```

**Zusätzlich:**
- Temperatur-Sensor
- PV-Überschuss-Sensor
- Smart Meter Integration

### Szenario 3: Premium Setup mit Chemie & Wetter

```yaml
Poolgröße: 80.000 L
Pumpendurchsatz: 15.000 L/h
Zeitfenster: unterschiedlich pro Wochentag
Temperatur: aktiv
PV-Überschuss: aktiv
Chlor-Sensor: aktiv
PH-Sensor: aktiv
Multitab-Dosierer: motorisiert
Wetterdienst: aktiv
```

**Komponenten:**
- Intelligente Pumpen-Steuerung
- Chemie-Sensoren
- Motorisierter Dosierer
- Funk-Wetterstation

---

## 🔐 Sicherheit & Datenschutz

### Empfehlungen:

1. **Sensoren schützen:** Montiere Sensoren vor direkter Sonne/Regen
2. **Kalibrierung:** Kalibriere Sensoren monatlich
3. **Backups:** Erstelle regelmäßig Backups deiner Home Assistant Konfiguration
4. **Authentifizierung:** Nutze starke Passwörter für Remote-Zugriff
5. **Wartung:** Überprüfe Pumpenstatus regelmäßig

---

## 📞 Support & Weitere Infos

### Nützliche Links:

- [Home Assistant Blueprints Doku](https://www.home-assistant.io/docs/automation/using_blueprints/)
- [Home Assistant Automation](https://www.home-assistant.io/docs/automation/)
- [MQTT Integration für Sensoren](https://www.home-assistant.io/integrations/mqtt/)

### Bei Problemen:

1. Überprüfe die Home Assistant Logs
2. Validiere die YAML-Syntax
3. Stelle sicher, dass alle Entities existieren
4. Teste die Automation manuell

---

**Version:** 1.0  
**Zuletzt aktualisiert:** Juni 2024  
**Lizenz:** MIT
