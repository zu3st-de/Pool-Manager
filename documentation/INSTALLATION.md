# Pool-Manager - Installations-Anleitung

Schritt-für-Schritt Anleitung zum Einrichten des Pool-Manager Systems in Home Assistant.

---

## Schritt 1: Vorbereitung (5 Min.)

### Was du haben solltest:
- [ ] Home Assistant Installation (2024.1 oder neuer)
- [ ] Admin-Zugriff auf Home Assistant
- [ ] Einen Pumpen-Schalter (Switch) bereits in Home Assistant konfiguriert
- [ ] Optional: Sensoren (Temperatur, Chlor, PH, etc.)

### Prüfe deine Home Assistant Version:
1. Gehe zu **Einstellungen** → **Über**
2. Notiere die Versionsnummer (mindestens 2024.1 nötig)

---

## Schritt 2: Blueprint-Dateien importieren (10 Min.)

### Methode A: Direkte Datei-Installation (Empfohlen)

1. Öffne dein Home Assistant Dateisystem (z.B. via SFTP oder Samba)
2. Navigiere zu: `config/blueprints/automation/`
3. Kopiere alle `.yaml` Dateien aus `blueprints/automation/` dorthin:
   - `pool_pump_automation.yaml`
   - `pool_chemistry_automation.yaml`
   - `pool_weather_integration.yaml`

4. Starte Home Assistant neu: **Einstellungen** → **System** → **Neustarten**

### Methode B: Über UI importieren

1. Gehe zu **Einstellungen** → **Automationen & Szenen** → **Blueprints**
2. Klicke auf "Blueprints importieren"
3. Wähle die Datei `pool_pump_automation.yaml` und klicke "Importieren"
4. Wiederhole für die anderen Dateien

---

## Schritt 3: Helper konfigurieren (15 Min.)

### Option 1: Über YAML-Editor (Empfohlen für Power-User)

1. Öffne deine `configuration.yaml` Datei
2. Kopiere den relevanten Inhalt aus `configuration_example.yaml`:
   ```yaml
   input_boolean:
     pool_bath_time_active:
       name: Badezeit Aktiv
       icon: mdi:swim
       initial: false
   # ... weitere Konfiguration
   ```

3. Speichere und gehe zu **Entwicklerwerkzeuge** → **YAML** → **Konfiguration neu laden**

### Option 2: Über UI erstellen

Wenn du lieber die UI nutzt:

1. **Einstellungen** → **Geräte und Dienste** → **Helfer**
2. Klicke auf "Helfer erstellen" → "Logischer Schalter"
3. Erstelle folgende Helfer:

```
Name: pool_bath_time_active
Beschreibung: Badezeit Aktiv
```

Wiederhole für:
- `pool_automation_paused`
- `pool_extra_runtime_remaining`
- `pool_bath_time_remaining`
- `pool_pump_min_runtime`
- `pool_target_temperature`

---

## Schritt 4: Automationen erstellen (20 Min.)

### 4a. Pumpen-Automation

1. Gehe zu **Einstellungen** → **Automationen & Szenen**
2. Klicke auf **"Neue Automation erstellen"** → **"Aus Blueprint erstellen"**
3. Suche und wähle **"Pool Pumpen-Automation"**
4. Konfiguriere die Einstellungen:

| Feld | Beispiel | Deine Auswahl |
|------|----------|---------------|
| Pumpen-Schalter | `switch.pool_pump` | |
| Poolgröße (L) | 50000 | |
| Pumpendurchsatz (L/h) | 10000 | |
| Montag Start | 06:00 | |
| Montag Ende | 22:00 | |
| (... für alle Wochentage) | | |

5. Klicke **"Speichern"**

### 4b. Wetterdienst-Integration (Optional)

1. **Neue Automation erstellen** → **"Aus Blueprint erstellen"**
2. Suche und wähle **"Pool Wetterdienst-Integration"**
3. Konfiguriere:
   - Wetter-Entity: `weather.home` (oder dein Wetterdienst)
   - Regen-Sensor: (falls vorhanden)
   - UV-Index-Sensor: (falls vorhanden)

### 4c. Chemie-Management (Optional)

1. **Neue Automation erstellen** → **"Aus Blueprint erstellen"**
2. Suche und wähle **"Pool Chemie-Management"**
3. Konfiguriere die Sensoren (falls vorhanden):
   - Chlor-Sensor: `sensor.pool_chlorine`
   - PH-Sensor: `sensor.pool_ph`

---

## Schritt 5: Dashboard einrichten (10 Min.)

### Erstelle ein neues Dashboard:

1. Gehe zu **Dashboards**
2. Klicke auf **"Erstelle neues Dashboard"**
3. Nenne es: `Pool-Manager`
4. Klicke auf **"Erstellen"**

### Dashboard mit Vorlagen füllen:

1. Klicke auf das Bleistift-Icon (Bearbeiten)
2. Klicke auf **"3 Punkte" → "YAML bearbeiten"**
3. Kopiere den Inhalt aus `dashboard_example.yaml`
4. Klicke **"Speichern"**

### Schnell-Test:

- Pumpen-Switch sollte sichtbar sein
- Buttons (Badezeit, Extra-Laufzeit) sollten vorhanden sein
- Gauges für Temperatur, Chlor, PH sollten (wenn Sensoren vorhanden) Daten zeigen

---

## Schritt 6: Sensoren hinzufügen (Optional, 30 Min.)

### Temperatur-Sensor (DS18B20 über ESPHome)

Wenn du einen Temperatur-Sensor hast:

1. Integriere den Sensor in Home Assistant (ESPHome, MQTT, etc.)
2. Notiere die Entity-ID: z.B. `sensor.pool_temperature`
3. Bearbeite die "Pool Pumpen-Automation" Automation
4. Aktiviere "Temperatur-basierte Steuerung"
5. Wähle den Sensor

### Chlor & PH Sensoren

Beispiel für MQTT-Sensoren:

```yaml
# configuration.yaml
mqtt:
  sensor:
    - name: "Pool Chlor"
      state_topic: "pool/chlorine"
      unit_of_measurement: "ppm"
      unique_id: pool_chlorine_sensor
      
    - name: "Pool PH"
      state_topic: "pool/ph"
      unique_id: pool_ph_sensor
```

Nach Hinzufügung: Restart Home Assistant

### PV-Überschuss-Sensor

Falls du eine PV-Anlage mit Hybrid-Wechselrichter hast:

```yaml
# Beispiel für SolarEdge
sensor:
  - platform: solaredge
    name: "PV Überschuss"
    resource: "https://api.solaredge.com/site/..."
    api_key: "YOUR_API_KEY"
```

---

## Schritt 7: Testen (5 Min.)

### Test 1: Manuelle Aktivierung

1. Öffne dein Pool-Dashboard
2. Klicke den **Pumpen-Schalter** → sollte Pumpe einschalten

### Test 2: Badezeit-Button

1. Klicke **"🏊 Badezeit"** Button
2. Überprüfe im Dashboard:
   - "Badezeit Aktiv" sollte `on` sein
   - "Verbleibende Zeit" sollte zählen

### Test 3: Automation-Trigger

1. Gehe zu **Einstellungen** → **Automationen & Szenen**
2. Öffne "Pool Pumpen-Automation"
3. Klicke auf **"Testweise ausführen"**
4. Beobachte, ob Pumpe aktiviert wird

### Test 4: Protokoll prüfen

1. Gehe zu **Einstellungen** → **Protokolle**
2. Suche nach "Pool" - sollten Debug-Meldungen erscheinen

---

## Schritt 8: Feinabstimmung (10 Min.)

### Zeitfenster anpassen

1. Bearbeite die Automation "Pool Pumpen-Automation"
2. Passe die Zeiten an deine Bedürfnisse an:
   - **Früher Start:** Bei niedrigen Temperaturen (z.B. 05:00)
   - **Später Ende:** Bei viel Badebetrieb (z.B. 23:00)
   - **Weekend:** Meist längere Zeiten

### Temperatur-Schwellwerte anpassen

1. Im Dashboard: **Einstellungen**
2. Passe "Zieltemperatur" an (normal: 24°C)
3. Pumpe läuft automatisch:
   - UNTER 18°C: AUS
   - 18-26°C: AN
   - ÜBER 26°C: AUS (Algengefahr)

### PV-Schwellwert anpassen

1. Bei "Pool Wetterdienst-Integration" Automation
2. Schwellwert für Überschuss (Standard: 2000W)
   - Höher = Weniger oft zusätzliche Laufzeit
   - Niedriger = Häufiger zusätzliche Laufzeit

---

## Checklisten nach Installation

### Basis-Setup ✅
- [ ] Blueprint-Dateien importiert
- [ ] Helper erstellt
- [ ] Pumpen-Automation erstellt
- [ ] Dashboard eingerichtet
- [ ] Manuelle Tests erfolgreich

### Optional - Temperatur ✅
- [ ] Sensor hinzugefügt
- [ ] Entity-ID in Automation eingetragen
- [ ] Schwellwerte angepasst

### Optional - Chemie ✅
- [ ] Chlor-Sensor integriert
- [ ] PH-Sensor integriert
- [ ] Chemie-Automation erstellt
- [ ] Benachrichtigungen aktiviert

### Optional - Wetter ✅
- [ ] Wetterdienst konfiguriert
- [ ] Wetter-Automation erstellt
- [ ] Regen-Sensor (falls vorhanden) hinzugefügt

### Optimierung ✅
- [ ] Zeitfenster deinen Bedürfnissen angepasst
- [ ] Schwellwerte kalibriert
- [ ] Notfall-Kontakte eingerichtet
- [ ] Backups aktiviert

---

## Häufige Fehler & Lösungen

### Fehler: "Blueprint nicht gefunden"
**Lösung:** 
- Starte Home Assistant neu
- Prüfe Dateipfad: `config/blueprints/automation/`
- Überprüfe YAML-Syntax

### Fehler: "Switch nicht verfügbar"
**Lösung:**
- Prüfe Entity-ID: **Einstellungen** → **Geräte & Dienste**
- Stelle sicher, dass der Schalter konfiguriert ist
- Teste Schalter manuell

### Fehler: "Trigger wird nicht ausgelöst"
**Lösung:**
- Überprüfe Zeiten (24h Format)
- Prüfe Automation in **Protokolle**
- Test: **"Testweise ausführen"**

### Fehler: "YAML-Fehler bei Reload"
**Lösung:**
- Öffne [yamllint.com](https://www.yamllint.com)
- Kopiere deine Konfiguration → überprüfe Fehler
- Achte auf Einrückung (2 Spaces)

---

## Nächste Schritte

1. **Kalibrierung:** Überprüfe nach einer Woche, ob Laufzeiten zu deinen Bedürfnissen passen
2. **Sensoren:** Erweitere optional mit Chemie oder Wettersensoren
3. **Automatisierungen:** Nutze "Extra-Laufzeit" für Stoßfiltration bei Bedarf
4. **Dokumentation:** Halte deine Schwellwerte und Tests dokumentiert

---

## Support

- **Lädt nicht:** Home Assistant Protokolle prüfen
- **Automation triggert nicht:** Teste manuell, prüfe Zeiten
- **Sensoren zeigen "unknown":** Überprüfe Entity-IDs und Sensor-Status
- **Andere Probleme:** Siehe ANLEITUNG.md

---

**Glückwunsch! Du hast deinen Pool-Manager installiert! 🎉**

Genießen Sie die automatische Poolzerwaltung!
