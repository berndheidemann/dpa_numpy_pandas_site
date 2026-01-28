# SVG-Visualisierungsvorschläge für NumPy/Pandas Lernmaterial

## Arbeitsblätter

### np-01-einfuehrung.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| Nach "Aufgabe 2 – Performance-Vergleich" | Geschwindigkeitsvergleich Python-Liste vs. NumPy | Balkendiagramm mit zwei Balken, Zeit auf Y-Achse, Speedup-Faktor visualisiert |
| Nach "Aufgabe 5 – Reshaping" | reshape()-Operation | 1D-Array (12 Elemente) → verschiedene 2D/3D-Formen mit Pfeilen, Elemente farblich passend |
| Nach dem Info-Block "flatten() vs. ravel()" | View vs. Copy Konzept | Zwei Diagramme: ravel() zeigt Pfeile zum Original, flatten() zeigt separate Kopie |

### np-02-indexierung.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| Nach "2D-Indexierung Grundlagen" | 2D-Slicing-Bereich in Matrix | Matrix mit farblich markierten Bereichen für `[0:2, 1:3]` |
| Nach "Aufgabe 6 – Views vs. Copies" | View-Referenz auf Original | Speicher-Diagramm: View-Zeiger auf Original-Array-Bereich |
| Nach der Tabelle "Slicing-Beispiele visualisiert" | Slicing-Operationen visuell | Horizontales Array mit markierten Bereichen für verschiedene Slices |

### np-03-statistik.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| Nach "Aufgabe 5 – Der axis-Parameter" | axis=0 vs. axis=1 Aggregation | 3×4 Matrix mit Pfeilen: axis=0 (↓ vertikal) und axis=1 (→ horizontal) |
| Nach "Aufgabe 7 – Perzentile und Quartile" | Quartile und IQR auf Zahlenstrahl | Zahlenstrahl mit Q1, Median, Q3, IQR-Bereich markiert |

### np-04-filtern.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| Nach "Grundlagen Boolean Indexing" | Boolean-Maske auf Array | Array mit True/False-Maske darunter, gefilterte Elemente hervorgehoben |
| Nach "Aufgabe 4 – Mehrere Bedingungen" | AND/OR-Verknüpfung visuell | Venn-Diagramm für `(bed1) & (bed2)` und `(bed1) \| (bed2)` |

### np-05-fallstudie.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| Nach "Aufgabe 4 – Zusammenhang Alkohol und Noten" | Korrelation visuell erklärt | Streudiagramm-Skizze mit positiver/negativer/keiner Korrelation |

### pd-01-einfuehrung.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| Nach "Pandas Datenstrukturen" Tabelle | Series vs. DataFrame Struktur | Series als einzelne beschriftete Spalte, DataFrame als Tabelle mit mehreren beschrifteten Spalten |
| Nach "Aufgabe 4 – Spalten auswählen" | `df['col']` vs. `df[['col']]` | Vergleich: Series (1D) mit Index vs. DataFrame (2D Tabelle mit Header) |

### pd-02-datenzugriff.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| Nach "Einführung" | iloc vs. loc Unterschied | Zwei DataFrames: einer mit numerischen Positionen markiert (iloc), einer mit Labels (loc) |
| Nach "Aufgabe 3 – loc: Label-basierter Zugriff" | Slicing-Unterschied iloc vs. loc | Zahlenstrahl: `iloc[0:3]` → 3 Elemente, `loc[0:3]` → 4 Elemente (inklusiv) |

### pd-03-aggregation.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| ✅ Split-Apply-Combine bereits vorhanden | - | - |
| Nach "Aufgabe 6 – Eigene Aggregatfunktionen" | agg() mit Named Aggregation | DataFrame → groupby → verschiedene Aggregatfunktionen → Ergebnis-DataFrame |

### pd-04-verbinden.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| Nach "Aufgabe 5 – Join-Typen verstehen" | 4 Join-Typen visuell | Venn-Diagramme für inner, left, right, outer Join mit farblichen Markierungen |
| Nach "concat vs. merge" Tabelle | concat (vertikal/horizontal) | Zwei kleine DataFrames, die untereinander (axis=0) und nebeneinander (axis=1) verbunden werden |

### pd-05-transformation.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| Nach "Vergleich map vs. apply" Tabelle | map() vs. apply() Workflow | map: Dict → Series → neue Series; apply: Funktion → Row/Column → Ergebnis |
| Nach "pd.cut() – Numerische Kategorisierung" | Binning/Kategorisierung | Zahlenstrahl mit Grenzen, Werte fallen in Kategorien (Bins) |

### pd-06-fallstudie.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| Nach "Aufgabe 3 – Daten bereinigen" | Datenbereinigungspipeline | Flowchart: Rohdaten → Typenkonvertierung → NaN-Behandlung → Standardisierung → Saubere Daten |

---

## Infoblätter

### numpy-grundlagen.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| Nach "Array-Eigenschaften" Tabelle | ndarray-Struktur mit Eigenschaften | 3D-Array-Würfel mit shape, dtype, ndim, size Labels |
| Nach "Datentypen (dtype)" Tabelle | Speicherverbrauch verschiedener dtypes | Balkendiagramm: int8 vs. int32 vs. float64 Speichergröße |
| Nach "Reshaping" Abschnitt | Reshape-Prozess | Lineare Elemente → verschiedene Matrixformen |

### numpy-indexierung.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| Nach "1D-Indexierung" Tabelle | Positive und negative Indizes | Array mit beiden Index-Richtungen: 0,1,2... und -1,-2,-3... |
| Nach "Boolean Indexing Grundprinzip" | Boolean-Maske Prozess | Schritt 1: Array → Bedingung → Boolean-Array; Schritt 2: Filterung |
| Nach "Views vs. Copies" Danger-Block | Speicher-Unterschied View/Copy | Zwei Diagramme mit Speicheradressen, View zeigt auf Original |

### numpy-funktionen.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| Nach "Achsen (axis) verstehen" | axis=0 vs. axis=1 detailliert | 3×4 Matrix mit Pfeilen für beide Achserichtungen und Ergebnis-Arrays |
| Nach "np.where()" Abschnitt | np.where Entscheidungsbaum | Bedingung → True-Pfad/False-Pfad → Ergebnis-Array |

### numpy-broadcasting.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| Nach "Broadcasting-Regeln" | Broadcasting-Regeln visuell | Shape-Vergleich von rechts nach links mit Kompatibilitätsprüfung |
| Nach "Visualisierung des Broadcastings" | Spalte + Zeile Broadcasting | Detaillierte Step-by-Step Expansion der Arrays |

### statistik-grundlagen.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| Nach "Mittelwert vs. Median" Abschnitt | Schiefe Verteilungen | Drei Histogramme: symmetrisch, rechtsschief, linksschief mit Mittelwert/Median markiert |
| Nach "Interquartilsabstand (IQR)" | Boxplot-Erklärung | Boxplot mit allen Elementen beschriftet: Whiskers, Box, Median, Ausreißer |
| Nach "Ausreißer erkennen" | IQR-Methode Grenzen | Zahlenstrahl mit 1.5×IQR-Grenzen und Ausreißer-Zonen |

### pandas-grundlagen.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| Nach Überschrift | DataFrame-Anatomie | DataFrame mit markierten Elementen: Index, Columns, Values, dtypes |
| Nach "Daten laden – CSV" Abschnitt | read_csv Workflow | CSV-Datei → pd.read_csv() → DataFrame mit Optionen |

### pandas-datenzugriff.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| Nach "Boolean Indexing" | Komplette Filter-Pipeline | DataFrame → Bedingung → Boolean-Series → gefilterter DataFrame |
| Nach "isin() – Mehrere Werte prüfen" | isin() vs. mehrere OR-Bedingungen | Vergleich der Syntax visuell |

### pandas-aggregation.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| ✅ Split-Apply-Combine bereits vorhanden | - | - |
| Nach "pivot_table()" Abschnitt | pivot_table Transformation | Langer DataFrame → Pivot-Operation → Breiter DataFrame (Kreuztabelle) |

### pandas-transformation.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| Nach "map() – Wertemapping" | map() mit Dictionary | Series → Dictionary-Lookup → Neue Series |
| Nach "apply() auf DataFrame (zeilenweise)" | apply(axis=1) vs. apply(axis=0) | DataFrame mit Funktionsanwendung in beide Richtungen |
| Nach "pd.cut() – Numerische Kategorisierung" | Binning-Prozess | Kontinuierliche Werte → Diskrete Kategorien mit Grenzen |

### datenbereinigung.md

| Position | Was visualisieren | Vorgeschlagene SVG-Grafik |
|----------|-------------------|---------------------------|
| Nach Einleitung "Typische Datenprobleme" | Datenqualitätsprobleme | Unsauberer DataFrame mit markierten Problemen: NaN, Duplikate, Ausreißer, Tippfehler |
| Nach "Fehlende Werte" Abschnitt | fillna() Strategien | DataFrame mit NaN → verschiedene Füllstrategien (mean, median, ffill) |
| Nach "Checkliste Datenbereinigung" | Bereinigungs-Pipeline detailliert | Flowchart mit konkreten Pandas-Befehlen an jedem Schritt |

---

## Priorisierung

### 🔴 Hohe Priorität (verbessern Verständnis am meisten)

| # | Thema | Datei | Grund |
|---|-------|-------|-------|
| 1 | **axis-Parameter** | numpy-funktionen.md, np-03-statistik.md | Häufigstes Verständnisproblem bei Anfängern |
| 2 | **View vs. Copy** | np-02-indexierung.md, numpy-indexierung.md | Kritisch für korrekten, fehlerfreien Code |
| 3 | **Join-Typen** | pd-04-verbinden.md | Zentral für Datenanalyse |
| 4 | **Boolean Indexing** | np-04-filtern.md | Mächtige Technik, Kernkonzept |
| 5 | **iloc vs. loc** | pd-02-datenzugriff.md | Häufige Fehlerquelle |

### 🟡 Mittlere Priorität

| # | Thema | Datei |
|---|-------|-------|
| 6 | Reshape-Operationen | np-01-einfuehrung.md, numpy-grundlagen.md |
| 7 | map() vs. apply() | pd-05-transformation.md |
| 8 | Broadcasting-Regeln | numpy-broadcasting.md |
| 9 | Datenbereinigungspipeline | pd-06-fallstudie.md, datenbereinigung.md |
| 10 | pivot_table Transformation | pandas-aggregation.md |

### 🟢 Niedrigere Priorität (bereits teilweise visualisiert oder weniger komplex)

| # | Thema | Datei |
|---|-------|-------|
| 11 | Series vs. DataFrame | pd-01-einfuehrung.md |
| 12 | pd.cut() Binning | pd-05-transformation.md |
| 13 | Quartile/IQR/Boxplot | statistik-grundlagen.md |
| 14 | Korrelation | np-05-fallstudie.md |

---

## Statistik

- **Gesamtzahl Vorschläge:** ~40 potenzielle SVG-Visualisierungen
- **Bereits vorhanden:** Split-Apply-Combine (pandas-aggregation.md)
- **Dateien mit mehreren Vorschlägen:** numpy-indexierung.md (3), pandas-transformation.md (3), datenbereinigung.md (3)
