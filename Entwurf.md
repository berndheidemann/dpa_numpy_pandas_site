# Didaktischer Entwurf: NumPy & Pandas Lernsituation

## 1. Analyse des Ist-Zustands

### 1.1 Vorlage-Struktur (vorlage_docs)

Die bestehende Vorlage für Python-Grundlagen zeigt eine gut strukturierte didaktische Aufbereitung:

| Merkmal | Beschreibung |
|---------|--------------|
| **Arbeitsblätter** | 7 aufeinander aufbauende Themen (AB 00–06) |
| **Infoblätter** | 9 begleitende Nachschlagewerke |
| **Format** | MkDocs mit Material-Theme |
| **Didaktik** | Lernziele, Admonitions, Aufgaben mit Checkboxen, Selbstkontrolle |
| **Praxisbezug** | Datenanalyse-Szenarien (FiDPA-gerecht) |

**Stärken der Vorlage:**
- Klare Lernzielformulierungen
- Verlinkung zwischen Arbeits- und Infoblättern
- Admonitions (note, tip, warning, success, question)
- Aufgaben mit Checkboxen zur Fortschrittskontrolle
- Zusammenfassungen und Selbstkontrollfragen
- Code-Beispiele mit Ausgaben
- Praxisnahe Szenarien (Datenbereinigung, Statistik)

---

### 1.2 Vorhandenes NumPy-Material

**Lernsituation 01: NYC Yellow Taxi Trip-Daten**
- Gut strukturierte Einführung in NumPy
- Themen: CSV laden, Indexierung, Slicing, Statistik, Filtern, Vektorisierung
- Visualisierungen vorhanden (8 Bilder)
- Praxisnaher Datensatz mit realem Bezug
- Reflexionsfragen enthalten

**Lernsituation 02: Studenten-Alkoholkonsum**
- Nur grobe Fragestellungen ohne Struktur
- Datensätze: student-mat.csv, student-por.csv
- Fokus auf explorative Datenanalyse
- Keine ausgearbeiteten Aufgaben

**Analyse:** Das NumPy-Material ist inhaltlich solide, aber strukturell noch nicht an die Vorlage angepasst. Es fehlen:
- Einführendes Infoblatt zu NumPy
- Strukturierte Lernzielformulierungen
- Verlinkungen und Admonitions
- Selbstkontrollfragen

---

### 1.3 Vorhandenes Pandas-Material

**Lernsituation 01: MBA-Entscheidungsdaten**
- Umfangreiche Aufgabensammlung (8 Hauptkapitel + Bonus)
- Themen: DataFrame-Grundlagen, iloc/loc, Boolean Indexing, GroupBy, map/apply, String-Operationen
- Best Practices und Ressourcen-Links
- Hilfestellungen zu jeder Aufgabe

**Lernsituation 02: Global Shark Attacks**
- Nur Datensatzbeschreibung vorhanden
- Keine ausgearbeiteten Aufgaben
- Guter Datensatz für fortgeschrittene Analysen

**Analyse:** Das Pandas-Material ist inhaltlich gut, aber:
- Keine Infoblätter zu Pandas-Konzepten
- Keine strukturierte Verlinkung
- Shark-Attack-Lernsituation nicht ausgearbeitet

---

## 2. Didaktisches Konzept

### 2.1 Zielgruppe und Lernprogression

**Zielgruppe:** Fachinformatiker/in für Daten- und Prozessanalyse (FiDPA)

**Voraussetzungen:** 
- Python-Grundlagen (aus vorlage_docs)
- Listen und Dictionaries
- Grundlegende Statistik-Kenntnisse

**Lernprogression:**

```mermaid
graph LR
    A[Python Grundlagen] --> B[NumPy Basics]
    B --> C[NumPy Fortgeschritten]
    C --> D[Pandas Basics]
    D --> E[Pandas Fortgeschritten]
    E --> F[Abschlussprojekt]
```

---

### 2.2 Geplante Struktur

#### Arbeitsblätter (chronologisch zu bearbeiten)

| Nr. | Thema | Datensatz | Beschreibung |
|-----|-------|-----------|--------------|
| **NP-01** | NumPy Einführung | (synthetisch) | Arrays erstellen, Eigenschaften, Grundoperationen |
| **NP-02** | Indexierung & Slicing | taxi_tripdata.csv | Datenextraktion, mehrdimensionale Arrays |
| **NP-03** | Statistik & Aggregation | taxi_tripdata.csv | mean, median, std, sum, Achsenoperationen |
| **NP-04** | Filtern & Vektorisierung | taxi_tripdata.csv | Boolean Indexing, Broadcasting |
| **NP-05** | NumPy Vertiefung | student-mat.csv | Komplexe Analysen, Anomalieerkennung |
| **PD-01** | Pandas Einführung | games.csv | DataFrame-Grundlagen, Series, Exploration |
| **PD-02** | Datenzugriff & Selektion | mba_decisions.csv | iloc, loc, Boolean Indexing |
| **PD-03** | Gruppierung & Aggregation | mba_decisions.csv | groupby, agg, Pivot-Tabellen |
| **PD-04** | Datentransformation | mba_decisions.csv | map, apply, String-Operationen |
| **PD-05** | Pandas Vertiefung | shark_attacks.csv | Datenbereinigung, komplexe Analysen |
| **NP-PD-06** | Abschlussprojekt | Kombiniert | Vollständige Analyse-Pipeline |

#### Infoblätter (als Nachschlagewerk)

| Thema | Inhalte |
|-------|---------|
| **NumPy Grundlagen** | Installation, Arrays, Datentypen, Dimensionen, Erstellen |
| **NumPy Indexierung** | Slicing, Fancy Indexing, Boolean Indexing, Views vs. Copies |
| **NumPy Funktionen** | Mathematische Operationen, Statistik, Achsen, Universal Functions |
| **NumPy Broadcasting** | Regeln, Beispiele, Performance |
| **Pandas Grundlagen** | DataFrame, Series, Index, Installation |
| **Pandas Datenzugriff** | iloc, loc, at, iat, Slicing, Boolean Indexing |
| **Pandas Aggregation** | groupby, agg, pivot_table, crosstab |
| **Pandas Transformation** | map, apply, applymap, String-Methoden |
| **Datenbereinigung** | Fehlende Werte, Duplikate, Datentypen, Outlier |

---

## 3. Detailplanung der Arbeitsblätter

### 3.1 NumPy-Arbeitsblätter

#### NP-01: NumPy Einführung
**Lernziele:**
- Verstehen, warum NumPy für Datenanalyse wichtig ist
- Arrays erstellen (np.array, np.zeros, np.ones, np.arange, np.linspace)
- Array-Eigenschaften verstehen (shape, dtype, ndim, size)
- Einfache Operationen durchführen

**Aufgaben:**
1. Listen vs. NumPy-Arrays vergleichen (Performance-Test)
2. Verschiedene Array-Erstellungsmethoden anwenden
3. Mehrdimensionale Arrays erstellen (1D, 2D, 3D)
4. Array-Eigenschaften inspizieren
5. Element-weise Operationen durchführen

**Praxisbezug:** Messwerte eines Temperatursensors

---

#### NP-02: Indexierung & Slicing
**Lernziele:**
- 1D- und 2D-Slicing beherrschen
- Negative Indizes nutzen
- Schrittweiten anwenden
- Spezifische Daten aus großen Arrays extrahieren

**Aufgaben:** (basierend auf Lernsituation 01)
1. Spalte `trip_distance` extrahieren
2. Mehrere Spalten gleichzeitig auswählen
3. Zeilen- und Spaltenbereiche kombinieren
4. Jede n-te Zeile extrahieren
5. Daten umkehren und transponieren

**Praxisbezug:** NYC Taxi Trip-Daten

---

#### NP-03: Statistik & Aggregation
**Lernziele:**
- Statistische Funktionen anwenden (mean, median, std, var)
- Aggregationen über Achsen verstehen
- Extremwerte finden (min, max, argmin, argmax)
- Summen und Produkte berechnen

**Aufgaben:**
1. Durchschnittliche Fahrstrecke berechnen
2. Median und Standardabweichung der Fahrpreise
3. Umsatzsumme berechnen
4. Zeilen- vs. Spaltenweise Aggregation
5. Fahrt mit höchstem Trinkgeld finden

**Praxisbezug:** NYC Taxi Trip-Daten

---

#### NP-04: Filtern & Vektorisierung
**Lernziele:**
- Boolean Indexing verstehen und anwenden
- Mehrere Bedingungen kombinieren (&, |, ~)
- Vektorisierte Berechnungen durchführen
- where() und select() nutzen

**Aufgaben:**
1. Fahrten mit mehr als 5 Meilen filtern
2. Preisbereich 10-20$ filtern
3. Kombinierte Bedingungen (Passagiere & Strecke)
4. Fahrpreis pro Meile berechnen (vektorisiert)
5. Preise um 10% erhöhen

**Praxisbezug:** NYC Taxi Trip-Daten

---

#### NP-05: NumPy Vertiefung
**Lernziele:**
- Komplexe Analysen selbstständig durchführen
- Anomalien/Ausreißer identifizieren
- Daten sortieren und gruppieren
- Ergebnisse speichern

**Aufgaben:** (basierend auf Lernsituation 02)
1. Studenten-Alkohol-Analyse
2. Korrelationen berechnen
3. Ausreißer identifizieren (> 2σ)
4. Daten in Kategorien einteilen
5. Ergebnisse als CSV speichern

**Praxisbezug:** Studenten-Alkoholkonsum-Studie

---

### 3.2 Pandas-Arbeitsblätter

#### PD-01: Pandas Einführung
**Lernziele:**
- DataFrame und Series verstehen
- CSV-Dateien laden
- Daten inspizieren (head, tail, info, describe, shape)
- Grundlegende Spaltenoperationen

**Aufgaben:**
1. games.csv laden und inspizieren
2. Spaltenauswahl und -umbenennung
3. Neue Spalten erstellen
4. Datentypen verstehen und konvertieren
5. Einfache Statistiken berechnen

**Praxisbezug:** Videospiel-Verkaufsdaten

---

#### PD-02: Datenzugriff & Selektion
**Lernziele:**
- Unterschied iloc vs. loc verstehen
- Boolean Indexing für komplexe Filter
- Mehrere Bedingungen kombinieren
- query()-Syntax als Alternative

**Aufgaben:** (basierend auf MBA-Lernsituation)
1. Zeilen/Spalten mit iloc extrahieren
2. Bedingte Auswahl mit loc
3. GPA ≥ 3.5 AND Networking ≥ 8 filtern
4. Online-MBA mit Entrepreneurial Interest
5. query()-Methode anwenden

**Praxisbezug:** MBA-Entscheidungsdaten

---

#### PD-03: Gruppierung & Aggregation
**Lernziele:**
- groupby()-Mechanismus verstehen
- Mehrere Aggregationsfunktionen anwenden
- Pivot-Tabellen erstellen
- Cross-Tabulation nutzen

**Aufgaben:**
1. Durchschnittsgehalt nach Funding Source
2. Höchster GRE/GMAT pro Major
3. Multi-Level-Gruppierung (Gender + Location)
4. Pivot-Tabelle: Gehalt nach Gender × Major
5. Eigene Aggregationsfunktionen definieren

**Praxisbezug:** MBA-Entscheidungsdaten

---

#### PD-04: Datentransformation
**Lernziele:**
- map() für Wertemapping
- apply() für Funktionsanwendung
- String-Methoden für Textdaten
- Lambda-Funktionen effektiv einsetzen

**Aufgaben:**
1. Ja/Nein zu 1/0 konvertieren
2. Gehalt um 5% erhöhen (bedingt)
3. Kategorien erstellen (Low/Medium/High)
4. String-Spalten zu Kleinbuchstaben
5. Text durchsuchen und extrahieren

**Praxisbezug:** MBA-Entscheidungsdaten

---

#### PD-05: Pandas Vertiefung
**Lernziele:**
- Umgang mit fehlenden Werten
- Datenbereinigung durchführen
- Komplexe Analysen kombinieren
- Ergebnisse exportieren

**Aufgaben:** (neue Lernsituation)
1. Shark Attack Daten laden und inspizieren
2. Fehlende Werte analysieren und behandeln
3. Zeitliche Trends analysieren (Jahr, Monat)
4. Geografische Verteilung analysieren
5. Aktivitäten vs. Fatalität untersuchen
6. Top 10 gefährlichste Locations ermitteln

**Praxisbezug:** Global Shark Attacks

---

#### NP-PD-06: Abschlussprojekt
**Lernziele:**
- Gesamte Datenanalyse-Pipeline durchführen
- NumPy und Pandas kombinieren
- Ergebnisse interpretieren und dokumentieren
- Visualisierungen erstellen (optional: matplotlib)

**Projektoptionen:**
1. Vollständige Taxi-Trip-Analyse mit Business-Insights
2. MBA-Entscheidungsmodell mit Korrelationsanalyse
3. Eigenes Projekt mit selbstgewähltem Datensatz

---

## 4. Infoblätter-Detailplanung

### 4.1 NumPy Grundlagen
```markdown
- Was ist NumPy und warum?
- Installation (pip install numpy)
- Arrays erstellen
  - np.array(), np.zeros(), np.ones()
  - np.arange(), np.linspace()
  - np.random.rand(), np.random.randint()
- Array-Eigenschaften
  - shape, dtype, ndim, size, itemsize
- Datentypen (int32, float64, etc.)
- Reshaping (reshape, flatten, ravel)
```

### 4.2 NumPy Indexierung
```markdown
- Grundlagen der Indizierung (0-basiert)
- 1D-Slicing [start:stop:step]
- 2D-Slicing [row, col]
- Negative Indizes
- Fancy Indexing (mit Listen)
- Boolean Indexing
- Views vs. Copies (wichtig!)
- Visualisierungen der Slicing-Konzepte
```

### 4.3 NumPy Funktionen
```markdown
- Mathematische Operationen
  - +, -, *, /, **, %
  - np.sqrt(), np.exp(), np.log()
- Statistische Funktionen
  - mean, median, std, var
  - min, max, sum, prod
  - argmin, argmax
- Achsen-Parameter (axis=0, axis=1)
- Universal Functions (ufuncs)
- Vergleichsoperatoren
```

### 4.4 NumPy Broadcasting
```markdown
- Was ist Broadcasting?
- Broadcasting-Regeln
- Beispiele mit Visualisierungen
- Performance-Vorteile
- Häufige Fehler vermeiden
```

### 4.5 Pandas Grundlagen
```markdown
- Was ist Pandas?
- Series vs. DataFrame
- Daten laden (read_csv, read_excel)
- Dateninspektion
  - head(), tail(), info(), describe()
  - shape, columns, dtypes, index
- Spaltenauswahl und -erstellung
- Datentypen konvertieren
```

### 4.6 Pandas Datenzugriff
```markdown
- iloc (integer-based)
- loc (label-based)
- at und iat (einzelne Werte)
- Boolean Indexing
- query()-Syntax
- Vergleich der Methoden
- SettingWithCopyWarning vermeiden
```

### 4.7 Pandas Aggregation
```markdown
- groupby()-Grundlagen
- Aggregationsfunktionen
  - sum, mean, count, min, max
  - agg() für mehrere Funktionen
- Named Aggregation
- pivot_table()
- crosstab()
- Multi-Level-Gruppierung
```

### 4.8 Pandas Transformation
```markdown
- map() für Series
- apply() für DataFrames
- applymap() (deprecated: map für DataFrames)
- Lambda-Funktionen
- String-Methoden (.str accessor)
  - contains, startswith, split
  - lower, upper, strip
  - replace, extract
```

### 4.9 Datenbereinigung
```markdown
- Fehlende Werte
  - isna(), notna()
  - fillna(), dropna()
  - Interpolation
- Duplikate
  - duplicated(), drop_duplicates()
- Datentypen korrigieren
- Ausreißer erkennen und behandeln
- Konsistenz prüfen
```

---

## 5. Umsetzungsplan

### Phase 1: Infrastruktur (1 Tag)
- [ ] docs-Ordner für NumPy/Pandas erstellen
- [ ] mkdocs.yml anpassen
- [ ] Datensätze in assets/files kopieren
- [ ] Bilder in assets/images organisieren

### Phase 2: Infoblätter (3-4 Tage)
- [ ] NumPy Grundlagen
- [ ] NumPy Indexierung
- [ ] NumPy Funktionen
- [ ] NumPy Broadcasting
- [ ] Pandas Grundlagen
- [ ] Pandas Datenzugriff
- [ ] Pandas Aggregation
- [ ] Pandas Transformation
- [ ] Datenbereinigung

### Phase 3: NumPy-Arbeitsblätter (3-4 Tage)
- [ ] NP-01: Einführung
- [ ] NP-02: Indexierung & Slicing
- [ ] NP-03: Statistik & Aggregation
- [ ] NP-04: Filtern & Vektorisierung
- [ ] NP-05: Vertiefung

### Phase 4: Pandas-Arbeitsblätter (4-5 Tage)
- [ ] PD-01: Einführung
- [ ] PD-02: Datenzugriff & Selektion
- [ ] PD-03: Gruppierung & Aggregation
- [ ] PD-04: Datentransformation
- [ ] PD-05: Vertiefung

### Phase 5: Abschluss (1-2 Tage)
- [ ] NP-PD-06: Abschlussprojekt
- [ ] Index-Seite erstellen
- [ ] Querverweise prüfen
- [ ] Gesamttest durchführen

---

## 6. Designentscheidungen

### 6.1 Admonitions-Konvention

| Typ | Verwendung |
|-----|------------|
| `!!! note` | Begleitende Infoblatt-Verlinkung |
| `!!! info` | Syntax-Erklärungen, Definitionen |
| `!!! tip` | Hilfreiche Tipps, Shortcuts |
| `!!! warning` | Häufige Fehler, Fallstricke |
| `!!! success` | Zusammenfassungen, Lernziele erreicht |
| `??? question` | Selbstkontrollfragen (ausklappbar) |
| `!!! example` | Code-Beispiele mit Ausgabe |

### 6.2 Aufgaben-Format

```markdown
### Aufgabe X – Titel

Kontextbeschreibung für die Aufgabe.

- [ ] **Teilaufgabe 1:** Beschreibung
- [ ] **Teilaufgabe 2:** Beschreibung
- [ ] **Teilaufgabe 3:** Beschreibung

!!! tip "Hinweis"
    Hilfestellung für die Aufgabe

Beispielausgabe:
```python
>>> code
erwartete_ausgabe
```
```

### 6.3 Code-Beispiel-Format

```python
import numpy as np

# Kommentar zur Erklärung
array = np.array([1, 2, 3, 4, 5])
print(array)        # [1 2 3 4 5]
print(array.shape)  # (5,)
```

---

## 7. Qualitätssicherung

### Checkliste pro Arbeitsblatt
- [ ] Lernziele klar formuliert
- [ ] Verlinkung zu Infoblättern
- [ ] Mindestens 5 Aufgaben mit Checkboxen
- [ ] Praxisbezug zu FiDPA
- [ ] Code-Beispiele mit Ausgaben
- [ ] Tipps/Warnungen wo nötig
- [ ] Zusammenfassung am Ende
- [ ] Selbstkontrollfragen

### Checkliste pro Infoblatt
- [ ] Strukturierte Überschriften
- [ ] Tabellen für Übersichten
- [ ] Code-Beispiele mit Ausgaben
- [ ] Visualisierungen wo hilfreich
- [ ] Selbstkontrollfragen am Ende

---

## 8. Datensatz-Übersicht

| Datensatz | Quelle | Verwendung | Größe |
|-----------|--------|------------|-------|
| taxi_tripdata.csv | old/numpy/LS01 | NumPy NP-02 bis NP-04 | ~5000 Zeilen |
| student-mat.csv | old/numpy/LS02 | NumPy NP-05 | ~400 Zeilen |
| student-por.csv | old/numpy/LS02 | NumPy NP-05 (optional) | ~650 Zeilen |
| games.csv | vorlage_docs/assets | Pandas PD-01 | ~16000 Zeilen |
| verkaufsdaten.csv | vorlage_docs/assets | Pandas (optional) | klein |
| mba_decisions.csv | old/pandas/LS01 | Pandas PD-02 bis PD-04 | ~1000 Zeilen |
| global_shark_attacks.csv | old/pandas/LS02 | Pandas PD-05 | ~6000 Zeilen |

---

## 9. Erwartete Ordnerstruktur

```
docs/
├── index.md
├── arbeitsblaetter/
│   ├── np-01-einfuehrung.md
│   ├── np-02-indexierung.md
│   ├── np-03-statistik.md
│   ├── np-04-filtern.md
│   ├── np-05-vertiefung.md
│   ├── pd-01-einfuehrung.md
│   ├── pd-02-datenzugriff.md
│   ├── pd-03-aggregation.md
│   ├── pd-04-transformation.md
│   ├── pd-05-vertiefung.md
│   └── np-pd-06-abschlussprojekt.md
├── infoblaetter/
│   ├── numpy-grundlagen.md
│   ├── numpy-indexierung.md
│   ├── numpy-funktionen.md
│   ├── numpy-broadcasting.md
│   ├── pandas-grundlagen.md
│   ├── pandas-datenzugriff.md
│   ├── pandas-aggregation.md
│   ├── pandas-transformation.md
│   └── datenbereinigung.md
└── assets/
    ├── files/
    │   ├── taxi_tripdata.csv
    │   ├── student-mat.csv
    │   ├── student-por.csv
    │   ├── games.csv
    │   ├── mba_decisions.csv
    │   └── global_shark_attacks.csv
    ├── images/
    │   ├── numpy/
    │   └── pandas/
    └── js/
        └── progress.js
```

---

## 10. Zusammenfassung

Dieser Entwurf bietet:

1. **Strukturierte Lernprogression** von NumPy-Basics über Pandas bis zum Abschlussprojekt
2. **11 Arbeitsblätter** mit insgesamt ~60 Aufgaben
3. **9 Infoblätter** als Nachschlagewerk
4. **Wiederverwendung** der vorhandenen Materialien und Datensätze
5. **Konsistentes Design** basierend auf der bewährten Vorlage
6. **Praxisbezug** für FiDPA mit realen Datensätzen
7. **Didaktische Elemente** wie Lernziele, Selbstkontrolle, Zusammenfassungen

Die Umsetzung sollte in ca. 2-3 Wochen möglich sein, wobei die Infoblätter parallel zu den Arbeitsblättern erstellt werden können.
