# 🏊 Pool-Manager für Home Assistant

Ein umfassendes Blueprint-System zur vollautomatischen Verwaltung einer Poolpumpe mit optionalen Chemie- und Wetter-Features.

## ✨ Kernfeatures

### 🎯 Pump-Automation
- ✅ **Flexible Zeitfenster** pro Wochentag (z.B. Mo-Fr: 6-22 Uhr, Sa-So: 5-23 Uhr)
- ✅ **Automatische Laufzeit-Berechnung** basierend auf Poolgröße, Durchsatz und Temperatur
- ✅ **Intelligente Umwälzungs-Steuerung**: 1-3 Umwälzungen pro Tag je nach Wassertemperatur
- ✅ **PV-Überschuss Nutzung**: Zusätzliche Filtration bei verfügbarem Solarstrom
- ✅ **Badezeit-Modus**: Button zum Deaktivieren der Automation (z.B. 2h Badepause)
- ✅ **Extra-Laufzeit**: Button für zusätzliche Pumpenlaufzeit (z.B. Stoßfiltration)

### 🧪 Chemie-Management (Optional)
- ✅ **Chlor-Überwachung**: Automatische Empfehlungen für **Chlor-Granulat** (in Gramm)
- ✅ **Teststreifen-Eingabe**: Manuelle Eingabe bis zum BLE-Sensor
- ✅ **BLE-YC01 Integration**: Automatische Messung (Chlor, PH, TDS, EC, ORP, Temperatur)
- ✅ **PH-Kontrolle**: Berechnung von PH+ oder PH- Mengen
- ✅ **Benachrichtigungen**: Alerts bei Abweichungen

### 🌦️ Wetter-Integration (Optional)
- ✅ **Regenwarnung**: Erkennung von Niederschlag
- ✅ **Unwetter-Schutz**: Abschaltung bei Gewitter
- ✅ **Sonnenschein-Nutzung**: Erhöhte Filtration bei hohem UV-Index
- ✅ **Temperaturvorhersage**: Planung basierend auf Wetter

---

## 📦 Projektstruktur

```
Pool-Manager/
├── blueprints/
│   ├── automation/
│   │   ├── pool_pump_automation.yaml          # Hauptautomation
│   │   ├── pool_chemistry_automation.yaml     # Chemie-Module
│   │   └── pool_weather_integration.yaml      # Wetter-Integration
│   └── templates/
│       ├── configuration_example.yaml         # Helper & Sensoren
│       └── dashboard_example.yaml             # LoVeLace Dashboard
├── documentation/
│   ├── ANLEITUNG.md                          # Ausführliche Dokumentation
│   ├── INSTALLATION.md                       # Schritt-für-Schritt Setup
│   └── CHECKLISTE.md                         # Konfigurations-Checkliste
└── README.md
```

---

## 🚀 Quick Start (5 Minuten)

### 1. Blueprints hochladen
```
Home Assistant > Einstellungen > Automationen & Szenen > Blueprints > Importieren
```
Kopiere den Inhalt der YAML-Dateien aus `blueprints/automation/`

### 2. Helper konfigurieren
```
Kopiere den Inhalt von configuration_example.yaml 
in deine configuration.yaml oder ein Include-File
```

### 3. Automation erstellen
```
Einstellungen > Automationen & Szenen > Neue Automation
Wähle "Pool Pumpen-Automation" Blueprint
Konfiguriere deine Pumpe und Zeitfenster
```

### 4. Dashboard anpassen
```
Einstellungen > Dashboards > Neu
Bearbeite als YAML und kopiere dashboard_example.yaml Inhalt
```

### 5. Genießen 🎉
Die Pumpe startet jetzt automatisch nach Plan!

---

## ⚙️ Minimale Anforderungen

- Home Assistant 2024.1 oder neuer
- 1 Pumpen-Switch (für Grundfunktion)
- Optional: Temperatur-, Chlor-, PH-Sensoren
- Optional: PV-Überschuss-Sensor
- Optional: Wetterdienst-Integration

---

## 🎨 Dashboard Highlights

Das System beinhaltet ein vorkonfiguriertes Dashboard mit:

| Tab | Inhalt |
|-----|--------|
| **Übersicht** | Pumpen-Status, Schnell-Aktionen |
| **Chemie** | Chlor/PH-Gauges, Empfehlungen |
| **Wetter** | Vorhersage, UV-Index, Regen |
| **Statistiken** | Laufzeit, Umwälzrate |
| **Einstellungen** | Konfiguration, Modi |

---

## 🔧 Typische Konfiguration

```yaml
# Kleine Familie (30.000L)
Poolgröße: 30.000 L
Pumpendurchsatz: 8.000 L/h
Laufzeit: 06:00 - 22:00
Temperatur: Optional
Kosten: ~50€ (nur Schalter + Raspberry Pi)

# Familie mit PV (50.000L)
Poolgröße: 50.000 L
Pumpendurchsatz: 10.000 L/h
Laufzeit: 06:00 - 22:00 + PV-Überschuss
Temperatur: Aktiv (Sensor ~30€)
PV-Sensor: Aktiv
Kosten: ~150€

# Premium Setup (80.000L + Chemie)
Poolgröße: 80.000 L
Pumpendurchsatz: 15.000 L/h
Laufzeit: Wochentag-abhängig
Temperatur: Aktiv
PV-Überschuss: Aktiv
Chlor-Sensor: Aktiv (~80€)
PH-Sensor: Aktiv (~80€)
Multitab-Dosierer: Motorisiert (~200€)
Wetterdienst: Aktiv
Kosten: ~500€+
```

---

## 📋 Was du brauchst

### Hardware
- [ ] Intelligenter Schalter für Pumpe (Z-Wave, WiFi, MQTT)
- [ ] Home Assistant Instanz (Raspberry Pi, NUC, etc.)
- [ ] Optional: Temperatur-Sensor (DS18B20, ~10€)
- [ ] Optional: Chlor/PH-Sensoren (WiFi/MQTT, ~150€)
- [ ] Optional: Wetterstation (Funk/WiFi, ~50€)

### Software
- [ ] Home Assistant aktuell (≥2024.1)
- [ ] MQTT Broker (falls MQTT-Sensoren used)
- [ ] Integrationen für deine Sensoren

### Dokumente
- [ ] Diese README
- [ ] ANLEITUNG.md (ausführliche Doku)
- [ ] INSTALLATION.md (Schritt-für-Schritt)
- [ ] CHECKLISTE.md (Konfigurations-Checklist)

---

## 💡 Beispiel-Nutzung

### Normale Woche
```
Mo-Fr: 06:00 - Pumpe start (Zeitfenster)
       22:00 - Pumpe stop (Zeitfenster)
       + Zusätzlich wenn PV > 2000W
       
Sa-So: 05:00 - Pumpe start
       23:00 - Pumpe stop
       + Badebetrieb mit Badezeit-Button
```

### Mit Badezeit-Modus
```
13:00 - Nutzer drückt "🏊 Badezeit"
        → Automation deaktiviert für 2h
        → Pumpe kann manuell gesteuert werden
        
15:00 - Badezeit vorbei
        → Automation reaktiviert
        → Pumpe läuft wieder nach Plan
```

### Mit Chemie-Integration
```
09:00 - Sensor zeigt Chlor: 0.8 ppm (TOO LOW)
        → System sendet: "Empfehlung: 50ml Chlor-Lösung"
        → Optional: Multitab-Dosierer wird geöffnet
        
16:00 - Sensor zeigt PH: 7.8 (TOO HIGH)
        → System sendet: "Empfehlung: 40ml PH-"
```

### Mit Wetter
```
Sonniger Tag:
- UV-Index 8 → Erhöhte Filtration
- PV Überschuss 3500W → Pumpe läuft zusätzlich
- Temp-Vorhersage 28°C → Warnung vor Algen

Gewittertag:
- Unwetter erkannt → Pumpe sofort aus
- Nachricht: "Schütze deine Ausrüstung!"
```

---

## 🆘 Häufige Probleme

| Problem | Lösung |
|---------|--------|
| Pumpe startet nicht | Prüfe Switch-Entity, Automation aktiviert? |
| Badezeit-Button funktioniert nicht | Prüfe `input_boolean.pool_bath_time_active` |
| Keine Benachrichtigungen | Teste notify-Service in Home Assistant |
| Sensoren zeigen "unknown" | Prüfe Entity-IDs, Sensor-Kalibrierung |
| YAML-Fehler | Nutze [YAML Validator](https://www.yamllint.com/) |

Detaillierte Lösungen: Siehe **ANLEITUNG.md**

---

## 📚 Weitere Ressourcen

- [Home Assistant Dokumentation](https://www.home-assistant.io/docs/)
- [Blueprint-Dokumentation](https://www.home-assistant.io/docs/automation/using_blueprints/)
- [Community Forum](https://community.home-assistant.io/)

---

## 📄 Lizenz

MIT License - Frei nutzbar, veränderbar, weitergabefähig.

---

## 🎯 Was kommt als nächstes?

Nach der Installation empfehlen wir:

1. **Liest die ANLEITUNG.md** - Detaillierte Erklärungen aller Features
2. **Folge INSTALLATION.md** - Schritt-für-Schritt Setup-Guide
3. **Nutze CHECKLISTE.md** - Stelle sicher, dass alles richtig ist
4. **Optimiere deine Konfiguration** - Passe Zeitfenster an deine Bedürfnisse an
5. **Erweitere optional** - Füge Chemie oder Wetter hinzu, wenn gewünscht

---

**Viel Spaß mit deinem Pool-Manager! 🏊‍♂️💧**

Bei Fragen oder Verbesserungsvorschlägen: Siehe Support in ANLEITUNG.md
