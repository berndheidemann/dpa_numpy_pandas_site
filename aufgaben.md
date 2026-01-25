# Aufgaben-Kategorisierung: Pflicht vs. Optional

Diese Übersicht kategorisiert alle Aufgaben der Arbeitsblätter in:

- **🔴 Pflicht**: Essentiell für das Verständnis des Kernthemas
- **🟢 Optional**: Vertiefung, komplexere Anwendung, Challenge

---

## Gesamtübersicht

| Arbeitsblatt | Gesamt | Pflicht | Optional | Zeit (Pflicht) | Zeit (Gesamt) |
|--------------|--------|---------|----------|----------------|---------------|
| np-01-einfuehrung | 10 | 6 | 4 | 45-60 min | 90-120 min |
| np-02-indexierung | 9 | 6 | 3 | 45-60 min | 90-120 min |
| np-03-statistik | 9 | 7 | 2 | 50-70 min | 90-120 min |
| np-04-filtern | 9 | 6 | 3 | 45-60 min | 90-120 min |
| np-05-fallstudie | 7+Bonus | 5 | 2+Bonus | 60-90 min | 120-180 min |
| pd-01-einfuehrung | 9 | 7 | 2 | 50-70 min | 90-120 min |
| pd-02-datenzugriff | 10 | 7 | 3 | 50-70 min | 100-140 min |
| pd-03-aggregation | 10 | 7 | 3 | 50-70 min | 100-140 min |
| pd-04-transformation | 11 | 7 | 4 | 60-80 min | 120-150 min |
| pd-05-fallstudie | 9+Bonus | 6 | 3+Bonus | 70-100 min | 150-210 min |
| abschluss-projekt | 5 | 5 | 0 | 120-180 min | 180-240 min |
| **SUMME** | **98+** | **69** | **29+** | **~11-14 h** | **~18-25 h** |

---

## NumPy Arbeitsblätter

### np-01-einfuehrung.md

#### 🔴 Pflicht (6 Aufgaben)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 1 | NumPy einrichten | Grundvoraussetzung für alle weiteren Aufgaben |
| 2 | Performance-Vergleich | Erklärt den Hauptgrund für NumPy-Einsatz |
| 3 | Arrays erstellen | Fundamentale Funktionen: array, zeros, ones, arange, linspace, random |
| 4 | Array-Eigenschaften | Essentiell: shape, ndim, dtype, size verstehen |
| 5 | Reshaping | Kernkonzept für Datenstrukturierung |
| 6 | Arithmetische Operationen | Element-weise Operationen sind NumPy-Grundlage |

#### 🟢 Optional (4 Aufgaben)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 7 | Temperatursensor | Anwendungsübung, wiederholt Konzepte |
| 8 | Notenberechnung | Vertiefung axis-Parameter (kommt in np-03 intensiver) |
| 9 | Würfelsimulation | Challenge: komplexere Kombination |
| 10 | Körpergrößen-Analyse | Bonus mit Normalverteilung und Histogramm |

---

### np-02-indexierung.md

#### 🔴 Pflicht (6 Aufgaben)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 1 | Daten laden und untersuchen | Grundlage: CSV laden, Shape, NaN prüfen |
| 2 | 1D-Indexierung und Slicing | Kernkonzept: start:stop:step-Notation |
| 3 | Mehrere Spalten extrahieren | Wichtig für praktische Datenauswahl |
| 4 | 2D-Slicing | Essentiell für Matrix-Manipulation |
| 5 | Slicing an kleiner Matrix | Verständnissicherung durch kleine Beispiele |
| 6 | Views vs. Copies | Kritisches Konzept um Datenfehler zu vermeiden |

#### 🟢 Optional (3 Aufgaben)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 7 | Praktische Analysen | Anwendung auf Taxi-Daten, Wiederholung |
| 8 | Transponieren und Umformen | Bonus: erweiterte Umformung |
| 9 | Eigenständige Analysen (A-D) | Challenge: 4 komplexe Aufgaben ohne Hilfe |

---

### np-03-statistik.md

#### 🔴 Pflicht (7 Aufgaben)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 1 | Daten laden und vorbereiten | Setup für alle Folgeaufgaben |
| 2 | Lagemaße berechnen | Kernstatistik: mean, median, min, max |
| 3 | Streuungsmaße | Essentiell: std, var, Variationskoeffizient |
| 4 | Aggregationsfunktionen | sum, NaN-Handling |
| 5 | Der axis-Parameter | Kritisches Konzept für mehrdimensionale Statistik |
| 6 | Extremwerte finden | argmax, argmin für Positionsfindung |
| 7 | Perzentile und Quartile | Wichtig für Datenverteilungsanalyse |

#### 🟢 Optional (2 Aufgaben)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 8 | Statistik-Funktion erstellen | Vertiefung: eigene Funktion bauen |
| 9 | Eigenständige Analysen (A-E) | Challenge: 5 komplexe Aufgaben ohne Lösung |

---

### np-04-filtern.md

#### 🔴 Pflicht (6 Aufgaben)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 1 | Daten vorbereiten | Setup |
| 2 | Grundlagen Boolean Indexing | Kernkonzept: Bedingungen als Filter |
| 3 | Verschiedene Vergleichsoperatoren | Alle Operatoren kennenlernen |
| 4 | Mehrere Bedingungen kombinieren | Kritisch: &, |, ~ mit Klammern |
| 5 | Filter auf Datensätze anwenden | Masken auf gesamten DataFrame |
| 6 | Vektorisierte Berechnungen | Kernvorteil von NumPy |

#### 🟢 Optional (3 Aufgaben)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 7 | np.where() | Vertiefung, nice-to-have |
| 8 | Praktische Analysen | Komplexere Anwendung |
| 9 | Komplexe Praxisaufgaben (A-E) | Challenge: 5 Aufgaben ohne Lösung |

---

### np-05-fallstudie.md

#### 🔴 Pflicht (5 Aufgaben)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 1 | Daten laden und erkunden | Grundlegende Exploration |
| 2 | Deskriptive Statistik | Zentrale Statistiken berechnen |
| 3 | Alkoholkonsum analysieren | Kategorisierung und Verteilung |
| 4 | Zusammenhang Alkohol und Noten | Korrelation berechnen und interpretieren |
| 5 | Weitere Einflussfaktoren | Multi-Variablen-Analyse |

#### 🟢 Optional (2 + Bonus)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 6 | Komplexe Analyse | Vertiefung: Multifaktor, Notenentwicklung |
| 7 | Erkenntnisse dokumentieren | Zusammenfassung schreiben |
| Bonus A-C | Geschlechtervergleich, Risikoprofile, Vorhersage | Extra-Challenges |

---

## Pandas Arbeitsblätter

### pd-01-einfuehrung.md

#### 🔴 Pflicht (7 Aufgaben)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 1 | Pandas importieren, DataFrame erstellen | Grundvoraussetzung |
| 2 | CSV-Dateien laden | head, tail, sample, shape |
| 3 | Datensatz erkunden | info, describe, dtypes, isnull |
| 4 | Spalten auswählen | Series vs. DataFrame verstehen |
| 5 | Datentypen konvertieren | astype, category |
| 6 | Eindeutige Werte und Häufigkeiten | nunique, unique, value_counts |
| 7 | Sortieren | sort_values, reset_index |

#### 🟢 Optional (2 Aufgaben)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 8 | Erste Analysen | Anwendungsübung |
| 9 | Eigenständige Erkundung (A-D) | Challenge: 4 Aufgabenblöcke ohne Lösung |

---

### pd-02-datenzugriff.md

#### 🔴 Pflicht (7 Aufgaben)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 1 | Datensatz laden und erkunden | Setup |
| 2 | iloc: Positions-basierter Zugriff | Essentiell für numerischen Zugriff |
| 3 | loc: Label-basierter Zugriff | Essentiell für Namen-Zugriff |
| 4 | Boolean Indexing Grundlagen | Kernkonzept für Filtern |
| 5 | Mehrere Bedingungen kombinieren | &, |, ~ mit Klammern |
| 6 | Filtern mit Textbedingungen | isin, String-Methoden |
| 7 | loc mit Bedingungen kombinieren | Filter + Spaltenauswahl |

#### 🟢 Optional (3 Aufgaben)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 8 | Query-Methode | Nice-to-have: SQL-ähnliche Syntax |
| 9 | Praktische Analysen | Anwendungsübungen |
| 10 | Komplexe Analyseaufgaben (A-E) | Challenge: 5 Aufgaben ohne Lösung |

---

### pd-03-aggregation.md

#### 🔴 Pflicht (7 Aufgaben)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 1 | Datensatz laden und verstehen | Setup |
| 2 | Grundlagen von groupby() | Kernkonzept Split-Apply-Combine |
| 3 | Verschiedene Aggregatfunktionen | mean, sum, count, std, quantile |
| 4 | Mehrere Spalten gruppieren | Multi-Level-Gruppierung |
| 5 | agg() mit mehreren Funktionen | Mehrfach-Aggregation, Named Aggregation |
| 6 | Eigene Aggregatfunktionen | Lambda und eigene Funktionen |
| 7 | Pivot-Tabellen | Kreuztabellen mit Aggregation |

#### 🟢 Optional (3 Aufgaben)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 8 | Crosstab für Häufigkeiten | Vertiefung Häufigkeitstabellen |
| 9 | Praktische Analysen | Komplexere Anwendung |
| 10 | Komplexe Analyseaufgaben (A-E) | Challenge: 5 Aufgaben ohne Lösung |

---

### pd-04-transformation.md

#### 🔴 Pflicht (7 Aufgaben)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 1 | Probleme identifizieren | Datenqualität prüfen |
| 2 | Fehlende Werte behandeln | dropna, fillna Strategien |
| 3 | Duplikate entfernen | duplicated, drop_duplicates |
| 4 | Neue Spalten berechnen | np.where, np.select, pd.cut |
| 5 | map() für Wertersetzung | Dictionary und Funktion-Mapping |
| 6 | apply() für komplexe Transformationen | axis=0/1, Lambda |
| 7 | Datentypen konvertieren | category, ordinal |

#### 🟢 Optional (4 Aufgaben)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 8 | Strings bereinigen | str.strip, str.lower, str.replace |
| 9 | Ausreißer behandeln | IQR-Methode, clip |
| 10 | Bereinigungspipeline | Vertiefung: eigene Funktion |
| 11 | Komplexe Transformationen (A-E) | Challenge: 5 Aufgaben ohne Lösung |

---

### pd-05-fallstudie.md

#### 🔴 Pflicht (6 Aufgaben)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 1 | Daten laden | Setup mit Encoding |
| 2 | Relevante Spalten auswählen | Spaltenfilterung |
| 3 | Daten bereinigen | Jahr, Fatal, Alter, Geschlecht bereinigen |
| 4 | Deskriptive Statistik | Grundlegende Kennzahlen |
| 5 | Zeitliche Trends | Gruppierung nach Jahr/Dekade |
| 6 | Tödliche Angriffe analysieren | Tödlichkeitsrate berechnen |

#### 🟢 Optional (3 + Bonus)

| Aufgabe | Thema | Begründung |
|---------|-------|------------|
| 7 | Tiefere Analysen | Geschlecht, Alter, Länderprofile |
| 8 | Pivot-Tabellen erstellen | Komplexe Kreuztabellen |
| 9 | Erkenntnisse dokumentieren | Report schreiben |
| Bonus A-D | Hai-Arten, Text Mining, Hotspots, Korrelation | Extra-Challenges |

---

### abschluss-projekt.md

#### 🔴 Pflicht (alle 5 Phasen)

| Phase | Thema | Begründung |
|-------|-------|------------|
| 1 | Daten laden und erkunden | Grundexploration |
| 2 | Datenbereinigung | Cleaning-Pipeline |
| 3 | NumPy-Analyse | Arrays, Statistik, Boolean Indexing |
| 4 | Pandas-Aggregation | groupby, pivot, Transformation |
| 5 | Erkenntnisse dokumentieren | Zusammenfassung |

*Das Projekt ist vollständig Pflicht – der Vertiefungsumfang variiert nach Anspruch.*

---

## Empfehlungen

### Für Schüler mit wenig Zeit (nur Pflicht)
- Geschätzter Zeitaufwand: **11-14 Stunden**
- Fokus auf: Alle 69 Pflichtaufgaben
- Ergebnis: Solides Grundverständnis von NumPy und Pandas

### Für Schüler mit mehr Zeit (Pflicht + Optional)
- Geschätzter Zeitaufwand: **18-25 Stunden**
- Fokus auf: Alle Aufgaben inklusive Challenges
- Ergebnis: Vertieftes Verständnis, prüfungsbereit

### Für Prüfungsvorbereitung
Die "Eigenständigen Aufgaben" am Ende jedes Arbeitsblatts (ohne Hilfe) sind ideal:
- np-02 Aufgabe 9 (A-D)
- np-03 Aufgabe 9 (A-E)
- np-04 Aufgabe 9 (A-E)
- pd-02 Aufgabe 10 (A-E)
- pd-03 Aufgabe 10 (A-E)
- pd-04 Aufgabe 11 (A-E)

---

## Kriterien für die Kategorisierung

### 🔴 Pflicht-Kriterien
- Führt ein **neues Kernkonzept** ein
- Ohne diese Aufgabe fehlt **Grundlagenwissen**
- **Voraussetzung** für spätere Arbeitsblätter
- Einmaliges Kennenlernen einer **wichtigen Funktion**

### 🟢 Optional-Kriterien
- **Wiederholt** bereits Gelerntes in variierter Form
- **Komplexe Kombination** von Konzepten
- Explizite **Challenge/Bonus-Aufgaben**
- **Eigenständige Analysen** ohne Hilfestellung
- **Nice-to-have** Funktionen (z.B. query-Methode)
