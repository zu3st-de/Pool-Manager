# 📑 Pool-Manager - Projektindex

Willkommen bei Pool-Manager! Hier findest du einen Überblick über alle Dateien und deren Zweck.

---

## 🗂️ Projektstruktur

```
Pool-Manager/
│
├── README.md                          ← START HIER! Schnelle Einführung
│
├── blueprints/
│   ├── automation/
│   │   ├── pool_pump_automation.yaml          🔴 HAUPTAUTOMATION
│   │   ├── pool_chemistry_automation.yaml      🟡 Optional: Chemie
│   │   └── pool_weather_integration.yaml       🟡 Optional: Wetter
│   │
│   └── templates/
│       ├── configuration_example.yaml         🔵 Helper & Sensoren-Vorlage
│       └── dashboard_example.yaml             🟢 Dashboard-Vorlage
│
└── documentation/
    ├── ANLEITUNG.md                   📖 Ausführliche Doku (150+ Seiten)
    ├── INSTALLATION.md                📖 Schritt-für-Schritt Setup
    ├── CHECKLISTE.md                  ✅ Konfigurations-Checkliste
    └── INDEX.md                       📑 Diese Datei
```

---

## 🎯 Wo fange ich an?

### Für Anfänger: START HIER ↓

1. **[README.md](README.md)** (5 min)
   - Überblick über alle Funktionen
   - Quick Start (5 Minuten)
   - Beispiel-Konfigurationen

2. **[INSTALLATION.md](documentation/INSTALLATION.md)** (30-45 min)
   - Schritt-für-Schritt Anleitung
   - Screenshot-freundlich
   - Häufige Fehler & Lösungen

3. **[CHECKLISTE.md](documentation/CHECKLISTE.md)** (15-30 min)
   - Konfiguration überprüfen
   - Tests durchführen
   - Deine Werte notieren

### Für erfahrene Nutzer: DIREKTSTART ↓

1. Importiere die Blueprints aus `blueprints/automation/`
2. Kopiere Helper aus `blueprints/templates/configuration_example.yaml`
3. Erstelle Automationen nach deinen Bedürfnissen
4. Teste mit [CHECKLISTE.md](documentation/CHECKLISTE.md)

### Für spezifische Fragen: ↓

| Frage | Gehe zu |
|-------|---------|
| "Wie installiere ich alles?" | [INSTALLATION.md](documentation/INSTALLATION.md) |
| "Wie konfiguriere ich X?" | [ANLEITUNG.md](documentation/ANLEITUNG.md) |
| "Ist alles richtig eingestellt?" | [CHECKLISTE.md](documentation/CHECKLISTE.md) |
| "Was kann Pool-Manager?" | [README.md](README.md) |
| "Fehler beim Setup?" | [INSTALLATION.md](documentation/INSTALLATION.md) - Fehlerbehebung |

---

## 📄 Datei-Beschreibungen

### 🔴 ERFORDERLICHE DATEIEN

#### `pool_pump_automation.yaml`
**Was:** Die Hauptautomation für Pumpen-Steuerung
**Größe:** ~400 Zeilen
**Enthält:**
- Zeitfenster pro Wochentag
- Temperatur-Steuerung
- PV-Überschuss-Integration
- Badezeit & Extra-Laufzeit Modi

**Wann brauchen:** IMMER (Kern-Funktionalität)

### 🟡 OPTIONALE DATEIEN

#### `pool_chemistry_automation.yaml`
**Was:** Chemie-Überwachung mit Empfehlungen
**Größe:** ~350 Zeilen
**Enthält:**
- Chlor-Wert-Überwachung
- PH-Kontrolle
- Multitab-Dosierer-Steuerung
- Automatische Benachrichtigungen

**Wann brauchen:** Wenn du Sensoren für Chlor/PH hast

#### `pool_weather_integration.yaml`
**Was:** Wetter-basierte Steuerung
**Größe:** ~300 Zeilen
**Enthält:**
- Regenwarnung
- Unwetter-Abschaltung
- Sonnenschein-Nutzung
- Temperaturvorhersage

**Wann brauchen:** Wenn du einen Wetterdienst hast

### 🔵 KONFIGURATIONSVORLAGEN

#### `configuration_example.yaml`
**Was:** Vorlage für alle notwendigen Helper
**Größe:** ~250 Zeilen
**Enthält:**
- Input-Boolean (Badezeit, Pause)
- Input-Number (Timer, Einstellungen)
- Input-Button (Schnell-Aktionen)
- Input-Select (Modi)
- Template-Sensoren (berechnete Werte)
- Support-Automationen (Timer-Countdown)

**Verwendung:**
- Kopiere Abschnitte in deine `configuration.yaml`
- ODER nutze als Include-File
- ODER erstelle Helper manuell über UI

#### `dashboard_example.yaml`
**Was:** Vorkonfiguriertes Dashboard
**Größe:** ~350 Zeilen
**Enthält:**
- Übersicht-Tab (Pumpe, Status, Schnell-Aktionen)
- Chemie-Tab (Chlor/PH-Gauges, Empfehlungen)
- Wetter-Tab (Vorhersage, UV-Index)
- Statistiken-Tab (Laufzeit, Daten)
- Einstellungen-Tab (Konfiguration)

**Verwendung:**
- Erstelle neues Dashboard
- Bearbeite als YAML
- Kopiere & Paste den Inhalt

### 📖 DOKUMENTATION

#### `ANLEITUNG.md`
**Was:** Umfassende Benutzerhandbuch
**Größe:** ~200 Seiten Text
**Enthält:**
- Feature-Übersicht
- Installationsschritte
- Detaillierte Konfigurationsanleitung
- Verwendungsbeispiele
- Fehlerbehandlung
- Tipps & Tricks

**Wann lesen:** Nach Installation, für Deep-Dive

#### `INSTALLATION.md`
**Was:** Schritt-für-Schritt Setup-Guide
**Größe:** ~100 Seiten Text
**Enthält:**
- Vorbereitung prüfen
- Blueprint-Installation (Methode A & B)
- Helper einrichten
- Automationen erstellen
- Dashboard aufbauen
- Testen & Feinabstimmung
- Häufige Fehler

**Wann lesen:** ZU BEGINN (einfach den Schritten folgen)

#### `CHECKLISTE.md`
**Was:** Verifiable Konfigurations-Checkliste
**Größe:** ~50 Seiten Text
**Enthält:**
- Pre-Installation Checklist
- Installations-Schritte zum Abhaken
- Konfiguration pro Modul
- Tests zum Durchlaufen
- Feinabstimmung
- Notfall-Hilfe
- Dokumentations-Template

**Wann nutzen:** WÄHREND & NACH Installation

#### `README.md`
**Was:** Projekt-Übersicht & Marketing
**Größe:** ~30 Seiten Text
**Enthält:**
- Feature-Übersicht
- Quick-Start
- Typische Setups
- Dashboard-Preview
- Links & Support

**Wann lesen:** ZU BEGINN (für Überblick)

#### `INDEX.md` (Diese Datei)
**Was:** Navigation durch alle Dateien
**Enthält:**
- Projekt-Struktur
- Datei-Beschreibungen
- Lese-Reihenfolge
- Schnelle Referenz

---

## 🚀 Empfohlene Lese-Reihenfolge

### Szenario A: Erstmaliges Setup (Anfänger)

```
1. README.md (5 min) ← Überblick
   ↓
2. INSTALLATION.md (45 min) ← Schritt-für-Schritt
   ↓
3. CHECKLISTE.md (30 min) ← Testen & Verifizieren
   ↓
4. ANLEITUNG.md (optional) ← Spezialthemen später
```

### Szenario B: Schnelles Setup (Erfahrene)

```
1. README.md - Quick Start (5 min)
   ↓
2. Blueprints importieren + Helper + Automation
   ↓
3. CHECKLISTE.md - Tests (15 min)
   ↓
4. ANLEITUNG.md - nur bei Problemen
```

### Szenario C: Spezifische Feature (z.B. Chemie)

```
1. README.md - Was ist möglich? (5 min)
   ↓
2. ANLEITUNG.md - Relevanter Abschnitt (15-30 min)
   ↓
3. CHECKLISTE.md - Relevant Zeilen (10 min)
   ↓
4. Blueprint importieren + Setup
```

### Szenario D: Fehlersuche

```
1. CHECKLISTE.md - "Notfall-Checklist" (5 min)
   ↓
2. INSTALLATION.md - "Häufige Fehler" (10 min)
   ↓
3. ANLEITUNG.md - Relevanter Abschnitt (15-30 min)
   ↓
4. Logs prüfen: Home Assistant → Einstellungen → Protokolle
```

---

## 📊 Feature-Matrix

| Feature | Datei | Anfänger | Profi | Optional |
|---------|-------|----------|-------|----------|
| Zeitfenster | pool_pump_automation.yaml | ✅ | ✅ | ❌ |
| Temperatur | pool_pump_automation.yaml | ⭐ | ✅ | ✅ |
| PV-Überschuss | pool_pump_automation.yaml | ⭐ | ✅ | ✅ |
| Badezeit-Button | pool_pump_automation.yaml | ✅ | ✅ | ❌ |
| Extra-Laufzeit | pool_pump_automation.yaml | ✅ | ✅ | ❌ |
| Chlor-Management | pool_chemistry_automation.yaml | ⭐ | ✅ | ✅ |
| PH-Management | pool_chemistry_automation.yaml | ⭐ | ✅ | ✅ |
| Multitab-Dosierer | pool_chemistry_automation.yaml | ⭐ | ✅ | ✅ |
| Regen-Warnung | pool_weather_integration.yaml | ⭐ | ✅ | ✅ |
| Unwetter-Schutz | pool_weather_integration.yaml | ⭐ | ✅ | ✅ |
| UV-Index Nutzung | pool_weather_integration.yaml | ⭐ | ✅ | ✅ |
| Dashboard | dashboard_example.yaml | ✅ | ✅ | ❌ |

**Legende:**
- ✅ = Erforderlich / Immer empfohlen
- ⭐ = Optional aber empfohlen
- ❌ = Kann übersprungen werden

---

## 🔍 Schnell-Referenz

### Ich möchte...

| Aufgabe | Lese | Datei |
|--------|------|-------|
| ...alles installieren | INSTALLATION.md | Alle |
| ...verstehen wie es funktioniert | ANLEITUNG.md | pool_pump_automation.yaml |
| ...Chemie-Features nutzen | ANLEITUNG.md (Chemie-Abschnitt) | pool_chemistry_automation.yaml |
| ...Wetter integrieren | ANLEITUNG.md (Wetter-Abschnitt) | pool_weather_integration.yaml |
| ...Fehler beheben | INSTALLATION.md (Fehler) + Logs | Relevant |
| ...Helper konfigurieren | CHECKLISTE.md | configuration_example.yaml |
| ...Dashboard anpassen | ANLEITUNG.md (Dashboard) | dashboard_example.yaml |
| ...mit PV starten | ANLEITUNG.md (PV-Überschuss) | pool_pump_automation.yaml |
| ...Automation testen | CHECKLISTE.md (Tests) | Pool Automationen |
| ...mein Setup dokumentieren | CHECKLISTE.md (Deine Daten) | Dein Pool-Setup |

---

## 📞 Support-Pfad

**Problem?** → Hier ist die Lösung:

```
Schritt 1: Suche in dieser Datei (INDEX.md)
           ↓
Schritt 2: Lies das relevante Kapitel in ANLEITUNG.md
           ↓
Schritt 3: Folge den Schritten in INSTALLATION.md
           ↓
Schritt 4: Überprüfe mit CHECKLISTE.md
           ↓
Schritt 5: Prüfe Home Assistant Logs:
           Einstellungen → Protokolle
           ↓
Schritt 6: Teste manuell im Home Assistant Frontend
```

---

## 💾 Wichtige Dateien zum Speichern

Nach dem Setup solltest du folgende Dateien speichern/backen:

- [ ] `configuration.yaml` (mit deiner Pool-Konfiguration)
- [ ] `blueprints/automation/*.yaml` (die 3 Blueprint-Dateien)
- [ ] Deine Werte aus CHECKLISTE.md
- [ ] Screenshots deines Dashboards
- [ ] Liste deiner Sensor-Entity-IDs

---

## 🎯 Nächste Schritte

1. **Jetzt:** Lese [README.md](README.md) (5 min)
2. **Dann:** Folge [INSTALLATION.md](documentation/INSTALLATION.md) Schritt-für-Schritt
3. **Dann:** Nutze [CHECKLISTE.md](documentation/CHECKLISTE.md) zum Testen
4. **Optional:** Lies [ANLEITUNG.md](documentation/ANLEITUNG.md) für Spezialthemen
5. **Genießen:** Automatischer Pool-Manager! 🏊‍♂️

---

**Version:** 1.0  
**Zuletzt aktualisiert:** Juni 2024  
**Projekt:** Pool-Manager für Home Assistant  
**Lizenz:** MIT
