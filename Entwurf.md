# Entwurf: Python-Lernsituation für Fachinformatiker Daten- und Prozessanalyse

## Kontext

Diese Lernsituation soll die bestehenden Java-Materialien (OOP-Konzepte) durch eine Python-Version ergänzen/ersetzen. Die Zielgruppe sind Fachinformatiker für Daten- und Prozessanalyse, daher sollten die Beispiele und Aufgaben einen starken Bezug zur Datenanalyse haben.

## Struktur

### Infoblätter (Wissensvermittlung)

| Nr. | Thema | Dateiname | Beschreibung |
|-----|-------|-----------|--------------|
| 1 | Python Grundlagen | `python-grundlagen.md` | Installation, IDE-Setup, Syntax, Variablen, Datentypen, Ein-/Ausgabe |
| 2 | Operatoren | `operatoren.md` | Arithmetische, Vergleichs-, Logische Operatoren, Typecasting |
| 3 | Verzweigungen | `verzweigungen.md` | if-elif-else, Ternärer Operator |
| 4 | Schleifen | `schleifen.md` | while, for, range(), enumerate(), zip(), break/continue |
| 5 | Listen | `listen.md` | Listen-Operationen, Slicing, List Comprehensions, 2D-Listen |
| 6 | Dictionaries | `dictionaries.md` | Key-Value-Paare, Methoden, Dict Comprehensions |
| 7 | Funktionen | `funktionen.md` | def, Parameter, *args/**kwargs, Lambda, Scope |
| 8 | OOP in Python | `oop-python.md` | Klassen, __init__, Vererbung, Polymorphismus, Properties |

### Arbeitsblätter (Übungen)

| Nr. | Thema | Dateiname | Aufgaben |
|-----|-------|-----------|----------|
| 00 | Vorbereitungen | `py-00-vorbereitungen.md` | Python-Installation, IDE-Setup, Hello World, erste Variablen |
| 01 | Verzweigungen | `py-01-verzweigungen.md` | Datenvalidierung, KPI-Bewertung, Datumsvalidierung |
| 02 | Schleifen | `py-02-schleifen.md` | Messwert-Erfassung, Log-Analyse, Batch-Verarbeitung |
| 03 | Listen | `py-03-listen.md` | Datenbereinigung, Statistik, Aktienkurse, 2D-Sensordaten |
| 04 | Dictionaries | `py-04-dictionaries.md` | Produktkatalog, Server-Monitoring, CSV-Analyse |
| 05 | OOP Grundlagen | `py-05-oop-grundlagen.md` | Measurement-Klasse, Vererbung DataSources, Properties |
| 06 | Pygame Projekt | `py-06-pygame-projekt.md` | Pong-Spiel mit Ball, Paddle, Score, Power-Ups |

## Berufsbezug für FiDPA

Alle Aufgaben sollten einen klaren Bezug zur Datenanalyse haben:

- **Datenqualität**: Validierung von Eingaben, Bereinigung von Messwerten
- **Statistik**: Durchschnitt, Median, Standardabweichung
- **Datenstrukturen**: Listen für Zeitreihen, Dictionaries für Lookup-Tabellen
- **Dateiverarbeitung**: CSV-Import/Export
- **Visualisierung**: Einfache Konsolenausgaben als Vorbereitung auf Matplotlib

## Beispiel-Szenarien für Aufgaben

1. **Messwert-Analyse**: Temperaturdaten von Sensoren auswerten
2. **Verkaufsdaten**: Umsätze aggregieren, Top-Produkte ermitteln
3. **Log-Analyse**: Server-Logs parsen und Anomalien erkennen
4. **KPI-Dashboard**: Kennzahlen berechnen und kategorisieren
5. **A/B-Test**: Conversion-Rates vergleichen

## Abschlussprojekt: Pygame

Das Pygame-Projekt (Pong) dient als Motivation und praktische Anwendung von OOP:
- Klassen für Spielobjekte (Ball, Paddle, Score)
- Vererbung für verschiedene Power-Up-Typen
- Event-Handling und Game-Loop
- Schrittweise Erweiterung des Spiels

## Nächste Schritte

1. [x] Infoblätter erstellen (8 Stück)
2. [x] Arbeitsblätter erstellen (7 Stück → jetzt 8: AB 00-07)
3. [x] ~~Pygame-Startercode vorbereiten~~ → Ersetzt durch Konsolen-Dashboard
4. [x] mkdocs.yml anpassen
5. [ ] Testen und Feedback einholen

---

# Analyse der Lernsituation (Stand: Aktuelle Version)

## ✅ Behobene Probleme

### 1. Dateibenennung – Infoblätter ~~nicht gefunden!~~ BEHOBEN

**Problem war:** Die `mkdocs.yml` verwies auf Infoblatt-Dateien mit `py-`-Präfix, die nicht existieren.

**Lösung:** mkdocs.yml und index.md wurden korrigiert, um die tatsächlichen Dateinamen zu verwenden.

### 2. index.md ~~verweist auf falsche Pfade~~ BEHOBEN

**Lösung:** Alle Links in index.md korrigiert.

### 3. games.csv Pfad ~~unklar~~ BEHOBEN

**Lösung:** 
- games.csv nach `docs/assets/files/games.csv` kopiert
- AB 04 Link aktualisiert auf `../assets/files/games.csv`

### 4. AB 05 OOP ~~Großer Komplexitätssprung~~ BEHOBEN

**Lösung:** AB 05 wurde aufgeteilt:
- **AB 05 – OOP Grundlagen:** Klassen, `__init__`, `__str__`, Klassenattribute, Komposition
- **AB 06 – OOP Vertiefung:** Vererbung, Properties, Polymorphismus, Pipelines

### 5. Pygame ~~weniger berufsbezogen~~ ERSETZT

**Lösung:** Pygame-Projekt wurde durch ein berufsrelevantes Abschlussprojekt ersetzt:
- **AB 07 – Konsolen-Daten-Dashboard:** Verkaufsdaten laden, filtern, sortieren, exportieren
- Kombiniert alle erlernten Konzepte
- Starker Bezug zur Datenanalyse

### 6. ~~Fehlendes Thema: Fehlerbehandlung (try-except)~~ BEHOBEN

**Lösung:** Neues Infoblatt `infoblaetter/exceptions.md` erstellt mit:
- Grundstruktur try-except-else-finally
- Häufige Exception-Typen
- Praktische Beispiele
- Eigene Exceptions

### 7. ~~Fehlendes Thema: String-Methoden~~ BEHOBEN

**Lösung:** Abschnitt "String-Methoden" im Infoblatt `python-grundlagen.md` hinzugefügt:
- `.upper()`, `.lower()`, `.strip()`, `.split()`, `.join()`
- `.replace()`, `.find()`, `.startswith()`, `.endswith()`

### 8. ~~Fehlendes Thema: Tuple~~ BEHOBEN

**Lösung:** Abschnitt "Tuple – Unveränderliche Listen" im Infoblatt `listen.md` hinzugefügt:
- Unterschied zu Listen
- Unpacking
- Als Dictionary-Schlüssel

---

## 🟡 Verbleibende Empfehlungen

### Optional (Nice-to-have)

- [ ] AB 03: Normalisierungsformel erklären
- [ ] Musterlösungen für komplexere Aufgaben
- [ ] Datei-Schreiben: Wird jetzt im Abschlussprojekt (AB 07) behandelt

---

## Aktuelle Struktur

### Arbeitsblätter (8 Stück)

| Nr. | Thema | Dateiname |
|-----|-------|-----------|
| 00 | Vorbereitungen | `py-00-vorbereitungen.md` |
| 01 | Verzweigungen | `py-01-verzweigungen.md` |
| 02 | Schleifen | `py-02-schleifen.md` |
| 03 | Listen | `py-03-listen.md` |
| 04 | Dictionaries | `py-04-dictionaries.md` |
| 05 | OOP Grundlagen | `py-05-oop-grundlagen.md` |
| 06 | OOP Vertiefung | `py-06-oop-vertiefung.md` |
| 07 | Abschlussprojekt | `py-07-abschlussprojekt.md` |

### Infoblätter (9 Stück)

| Thema | Dateiname |
|-------|-----------|
| Python Grundlagen | `python-grundlagen.md` |
| Operatoren | `operatoren.md` |
| Verzweigungen | `verzweigungen.md` |
| Schleifen | `schleifen.md` |
| Listen | `listen.md` |
| Dictionaries | `dictionaries.md` |
| Funktionen | `funktionen.md` |
| Exceptions | `exceptions.md` |
| OOP in Python | `oop-python.md` |

---

## 🟢 Zusammenfassung

Die Lernsituation ist **vollständig überarbeitet** und einsatzbereit:

- ✅ Alle Pfade in mkdocs.yml und index.md korrigiert
- ✅ OOP in zwei Arbeitsblätter aufgeteilt (bessere Progression)
- ✅ Pygame durch berufsbezogenes Konsolen-Dashboard ersetzt
- ✅ Fehlende Themen ergänzt (Exceptions, String-Methoden, Tuple)
- ✅ games.csv korrekt verlinkt
