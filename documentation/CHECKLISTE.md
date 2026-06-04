# Pool-Manager - Konfigurations-Checkliste

Verwende diese Checkliste, um sicherzustellen, dass dein Pool-Manager System vollständig und korrekt konfiguriert ist.

---

## 📋 Vor der Installation

### Vorbereitung
- [ ] Home Assistant Version überprüft (≥ 2024.1)
- [ ] Admin-Zugriff verfügbar
- [ ] Home Assistant neu gestartet kürzlich
- [ ] Backup der aktuellen Konfiguration erstellt

### Hardware bereit
- [ ] Pumpen-Schalter bereits in Home Assistant konfiguriert
- [ ] Entity-ID des Schalters notiert: `_______________`
- [ ] Optional: Sensoren physikalisch installiert und getestet

---

## 🔧 Installation

### Blueprint-Dateien
- [ ] `pool_pump_automation.yaml` in `config/blueprints/automation/` kopiert
- [ ] `pool_chemistry_automation.yaml` in `config/blueprints/automation/` kopiert
- [ ] `pool_weather_integration.yaml` in `config/blueprints/automation/` kopiert
- [ ] Home Assistant neu gestartet
- [ ] Blueprints in UI sichtbar (**Einstellungen** → **Automationen & Szenen** → **Blueprints**)

### Helper erstellt
- [ ] `input_boolean.pool_bath_time_active`
- [ ] `input_boolean.pool_automation_paused`
- [ ] `input_number.pool_bath_time_remaining`
- [ ] `input_number.pool_extra_runtime_remaining`
- [ ] `input_number.pool_pump_min_runtime`
- [ ] `input_number.pool_target_temperature`
- [ ] `input_button.pool_bath_time`
- [ ] `input_button.pool_extra_runtime`
- [ ] `input_button.pool_maintenance_mode`
- [ ] `input_select.pool_pump_mode`

---

## ⚙️ Basis-Konfiguration

### Pumpen-Automation Automation

**Automation erstellt:** ☐

#### Erforderliche Parameter

| Parameter | Wert | Notizen |
|-----------|------|--------|
| Pumpen-Schalter | | z.B. switch.pool_pump |
| Poolgröße (L) | | z.B. 50000 |
| Pumpendurchsatz (L/h) | | z.B. 10000 |

#### Zeitfenster - Montag
- [ ] Startzeit: `________` (z.B. 06:00)
- [ ] Endzeit: `________` (z.B. 22:00)

#### Zeitfenster - Dienstag
- [ ] Startzeit: `________`
- [ ] Endzeit: `________`

#### Zeitfenster - Mittwoch
- [ ] Startzeit: `________`
- [ ] Endzeit: `________`

#### Zeitfenster - Donnerstag
- [ ] Startzeit: `________`
- [ ] Endzeit: `________`

#### Zeitfenster - Freitag
- [ ] Startzeit: `________`
- [ ] Endzeit: `________`

#### Zeitfenster - Samstag
- [ ] Startzeit: `________`
- [ ] Endzeit: `________`

#### Zeitfenster - Sonntag
- [ ] Startzeit: `________`
- [ ] Endzeit: `________`

#### Badezeit
- [ ] Button-Entity: `input_button.pool_bath_time`
- [ ] Dauer eingestellt: `________` Minuten (default: 120)

#### Extra-Laufzeit
- [ ] Button-Entity: `input_button.pool_extra_runtime`
- [ ] Dauer eingestellt: `________` Minuten (default: 60)

---

## 🌡️ Optional: Temperatur-Steuerung

### Aktivierung
- [ ] Temperatur-Steuerung in Automation **AKTIVIERT**
- [ ] Wasser-Temperatur-Sensor ausgewählt: `______________`
- [ ] Sensor testet korrekt (zeigt Wert in °C)

### Schwellwerte (Empfohlen)
| Schwellwert | Standard | Dein Wert | Notizen |
|-------------|----------|-----------|--------|
| Minimale Temp. | 18°C | | Pumpe AUS unter dieser Temp. |
| Maximale Temp. | 26°C | | Pumpe AUS über dieser Temp. |

---

## ☀️ Optional: PV-Überschuss-Steuerung

### Aktivierung
- [ ] PV-Überschuss-Steuerung in Automation **AKTIVIERT**
- [ ] PV-Überschuss-Sensor ausgewählt: `______________`
- [ ] Sensor testet korrekt (zeigt Wert in W)

### Parameter
| Parameter | Standard | Dein Wert | Notizen |
|-----------|----------|-----------|---------|
| PV-Schwellwert | 2000W | | Ab dieser Leistung läuft Pumpe zusätzlich |

### Integration überprüft
- [ ] PV-System in Home Assistant konfiguriert
- [ ] Entity-ID des Überschuss-Sensors bekannt
- [ ] Sensoren liefern realistische Werte

---

## 🧪 Optional: Chemie-Management

### Aktivierung
- [ ] Chemie-Automation erstellt (Pool Chemie-Management)
- [ ] Automation in: **Einstellungen** → **Automationen & Szenen** sichtbar

### Chlor-Sensor

**Sensor vorhanden:** ☐

| Parameter | Beispiel | Dein Wert |
|-----------|----------|-----------|
| Entity-ID | sensor.pool_chlorine | |
| Einheit | ppm | |
| Min. Zielwert | 1.0 ppm | |
| Max. Zielwert | 3.0 ppm | |
| Kritisch niedrig | 0.5 ppm | |

### PH-Sensor

**Sensor vorhanden:** ☐

| Parameter | Beispiel | Dein Wert |
|-----------|----------|-----------|
| Entity-ID | sensor.pool_ph | |
| Min. Zielwert | 7.0 | |
| Max. Zielwert | 7.6 | |

### Multitab-Dosierer

**Dosierer vorhanden:** ☐

- [ ] Entity-ID: `______________` (input_number)
- [ ] Bereich: 0-100% konfiguriert
- [ ] Automation kann Dosierer steuern

### Benachrichtigungen

- [ ] Benachrichtigungs-Service aktiviert: `notify.______________`
- [ ] Test-Benachrichtigung gesendet und empfangen

---

## 🌦️ Optional: Wetter-Integration

### Aktivierung
- [ ] Wetter-Automation erstellt (Pool Wetterdienst-Integration)
- [ ] Wetterdienst in Home Assistant konfiguriert

### Wetter-Entity
- [ ] Entity-ID: `weather._______________`
- [ ] Wetterdienst testet korrekt
- [ ] Aktuelle Wetterbedingungen sichtbar

### Regen-Sensor (Optional)
- [ ] Sensor vorhanden: ☐
- [ ] Entity-ID: `______________`
- [ ] Schwellwert: `________` mm/h (default: 2.0)

### UV-Index (Optional)
- [ ] Sensor vorhanden: ☐
- [ ] Entity-ID: `______________`
- [ ] Werte zwischen 0-12

### Benachrichtigungen
- [ ] Service: `notify.______________`
- [ ] Test-Benachrichtigung bei Unwetter gesendet

---

## 📊 Dashboard

### Dashboard erstellt
- [ ] Neues Dashboard "Pool-Manager" erstellt
- [ ] YAML-Inhalt eingefügt

### Dashboard-Tabs
- [ ] ✅ Übersicht-Tab (Pumpe, Status, Quick-Actions)
- [ ] ✅ Chemie-Tab (optional, aber sichtbar)
- [ ] ✅ Wetter-Tab (optional, aber sichtbar)
- [ ] ✅ Statistiken-Tab (Laufzeit, Daten)
- [ ] ✅ Einstellungen-Tab (Konfiguration)

### Funktionalität überprüft
- [ ] Pumpen-Schalter im Dashboard sichtbar und funktionsfähig
- [ ] Badezeit-Button funktioniert
- [ ] Extra-Laufzeit-Button funktioniert
- [ ] Sensoren-Daten korrekt angezeigt

---

## 🧪 Tests durchführen

### Test 1: Manuelle Pumpen-Steuerung
- [ ] Pumpe über Dashboard ein-/ausschalten möglich
- [ ] Schalter antwortet sofort
- [ ] Physische Pumpe folgt dem Befehl

### Test 2: Badezeit-Modus
- [ ] Badezeit-Button drückt Automation in Pause
- [ ] `input_boolean.pool_bath_time_active` wird ON
- [ ] Counter zählt runterzählt
- [ ] Nach Ablauf: Automation reaktiviert

### Test 3: Extra-Laufzeit
- [ ] Extra-Laufzeit-Button aktiviert Pumpe zusätzlich
- [ ] Timer zählt herunter
- [ ] Nach Ablauf: Pumpe folgt normalem Plan

### Test 4: Zeitfenster (Testweise ausführen)
- [ ] Automation manuell ausgelöst
- [ ] Pumpe startet zum richtigen Zeitfenster
- [ ] Pumpe stoppt zum konfigurierten Ende
- [ ] Protokoll zeigt keine Fehler

### Test 5: Temperatur-Steuerung (Falls aktiv)
- [ ] Sensor liefert Werte
- [ ] Pumpe reagiert auf Temperatur-Änderungen
- [ ] Schwellwerte werden eingehalten

### Test 6: PV-Überschuss (Falls aktiv)
- [ ] Sensor zeigt PV-Leistung
- [ ] Pumpe startet bei Überschuss > Schwellwert
- [ ] Pumpe stoppt wenn Überschuss < Schwellwert

### Test 7: Chemie-Empfehlungen (Falls aktiv)
- [ ] Sensoren liefern Werte
- [ ] Benachrichtigungen werden gesendet
- [ ] Empfehlungen mathematisch korrekt

### Test 8: Wetter-Integration (Falls aktiv)
- [ ] Wetterdaten aktualisieren sich
- [ ] Benachrichtigungen bei Regen/Gewitter
- [ ] Pumpe reagiert auf Unwetter

### Test 9: Protokoll überprüft
- [ ] **Einstellungen** → **Protokolle** geöffnet
- [ ] Keine Fehler für Pool-Automationen
- [ ] Debug-Einträge vorhanden (wenn gewünscht)

---

## 🔧 Feinabstimmung

### Zeitfenster-Optimierung
- [ ] Beobachte Pumpenlaufzeiten über 1 Woche
- [ ] Anpassungen notiert: `_______________________`
- [ ] Ggf. Startzeiten früher/später verlegt

### Temperatur-Schwellwerte
- [ ] Beobachte Temperaturverlauf
- [ ] Min-Temperatur angepasst: `________` °C
- [ ] Max-Temperatur angepasst: `________` °C

### PV-Schwellwert
- [ ] Beobachte PV-Überschuss über 1-2 Wochen
- [ ] Schwellwert angepasst: `________` W
- [ ] Balance zwischen Laufzeit und Nutzen gefunden

### Chemie-Grenzwerte
- [ ] Chlor-Zielwerte angepasst: `______` - `______` ppm
- [ ] PH-Zielwerte angepasst: `______` - `______`
- [ ] Benachrichtigungs-Häufigkeit optimiert

---

## 📈 Betrieb überprüfen

### Wöchentliche Überprüfung
- [ ] Pumpe läuft nach Plan
- [ ] Keine unerwarteten Abschaltungen
- [ ] Sensoren zeigen realistische Werte
- [ ] Benachrichtigungen hilfreich und nicht zu häufig

### Monatliche Überprüfung
- [ ] Sensor-Kalibrierung überprüft
- [ ] Physische Ausrüstung inspiziert
- [ ] Laufzeit-Statistiken überprüft
- [ ] Optimierungen überlegt

### Saisonale Überprüfung
- [ ] Sommerbetrieb: Sind längere Laufzeiten nötig?
- [ ] Winterbetrieb: Können Zeiten verkürzt werden?
- [ ] Übergang: Schwellwerte angepasst?

---

## 🆘 Notfall-Checklist

### Wenn Pumpe nicht läuft
1. [ ] Home Assistant online? Protokolle prüfen
2. [ ] Schalter manuell funktioniert? Schalter-Fehler?
3. [ ] Entity-ID korrekt? Status-Übersicht prüfen
4. [ ] Automation aktiv? Automation deaktiviert?
5. [ ] Badezeit-Modus aktiv? Deaktiviere ihn
6. [ ] Zeitfenster aktuell? Manuelle Zeit prüfen

### Wenn Pumpe nicht stoppt
1. [ ] Automation überprüfen - Stop-Trigger aktiv?
2. [ ] Schalter manuell ausschaltbar? Schalter-Fehler?
3. [ ] Extra-Laufzeit aktiv? Timer prüfen
4. [ ] Manuelle Automation blockiert? Prüfe Automationen

### Wenn Sensoren zeigen "unknown"
1. [ ] Sensoren in Übersicht sichtbar? Entity-IDs prüfen
2. [ ] Sensor-Integration aktiv? (MQTT, ESPHome, etc.)
3. [ ] Sensor-Werte mit ihrer App/Interface überprüfbar?
4. [ ] Integration konfiguriert? Dokumentation konsultieren

---

## ✅ Abschluss-Checkliste

### Alles funktioniert
- [ ] Basis-Automation läuft zuverlässig
- [ ] Dashboard ist hilfreich und vollständig
- [ ] Alle Tests erfolgreich abgeschlossen
- [ ] Konfiguration dokumentiert

### Dokumentation
- [ ] Deine Konfigurationswerte notiert (siehe unten)
- [ ] Sensor-Entity-IDs dokumentiert
- [ ] Schwellwerte aufgeschrieben
- [ ] Telefon-/Kontaktdaten hinterlegt für Notfälle

### Backup & Sicherheit
- [ ] Home Assistant Backup erstellt
- [ ] Configuration gebackupt
- [ ] Support-Kontakte verfügbar
- [ ] Passwörter sicher gespeichert

---

## 📝 Deine Konfigurationsdaten

Speichere hier deine wichtigsten Parameter:

```
POOL-DATEN:
Poolgröße: _________________ L
Pumpendurchsatz: _________________ L/h
Maximale Laufzeit pro Tag: _________________ h

ZEITFENSTER:
Mo-Fr: __________ bis __________
Sa-So: __________ bis __________

TEMPERATUR:
Min: __________ °C
Max: __________ °C
Aktuell: __________ °C

CHEMIE:
Chlor Ziel: __________ - __________ ppm
PH Ziel: __________ - __________
Kritisch Chlor: __________ ppm

PV-SYSTEM:
Schwellwert: __________ W
Aktuelle Leistung: __________ W

KONTAKTE:
Techniker: __________
Elektriker: __________
Emergency: __________
```

---

**Herzlichen Glückwunsch! Dein Pool-Manager ist vollständig konfiguriert! 🎉**

Bei Fragen oder Problemen:
- Konsultiere: ANLEITUNG.md
- Überprüfe: Home Assistant Protokolle
- Teste: Manuelle Ausführung der Automationen
