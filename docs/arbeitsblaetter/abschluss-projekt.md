# Abschlussprojekt – Datenanalyse mit NumPy & Pandas

## Projektbeschreibung

In diesem Abschlussprojekt wendest du alle gelernten Techniken aus NumPy und Pandas an, um einen Datensatz deiner Wahl vollständig zu analysieren.

```kroki-plantuml
@startuml
!theme plain
skinparam backgroundColor transparent

rectangle "Abschlussprojekt" {
    rectangle "Phase 1" as p1 #lightcoral {
        rectangle "Datensatz wählen\n& verstehen" as ph1
    }
    
    rectangle "Phase 2" as p2 #lightyellow {
        rectangle "Daten laden\n& bereinigen" as ph2
    }
    
    rectangle "Phase 3" as p3 #lightblue {
        rectangle "Explorative\nAnalyse" as ph3
    }
    
    rectangle "Phase 4" as p4 #lightgreen {
        rectangle "Vertiefende\nAnalyse" as ph4
    }
    
    rectangle "Phase 5" as p5 #plum {
        rectangle "Dokumentation\n& Präsentation" as ph5
    }
}

p1 --> p2
p2 --> p3
p3 --> p4
p4 --> p5
@enduml
```

---

## Anforderungen

### Technische Anforderungen

Dein Projekt muss folgende Techniken demonstrieren:

!!! abstract "NumPy-Anforderungen"
    - [ ] Arrays erstellen und manipulieren
    - [ ] Indexierung und Slicing
    - [ ] Statistische Funktionen (mean, std, median, etc.)
    - [ ] Boolean Indexing / Filtering
    - [ ] Vektorisierte Berechnungen

!!! abstract "Pandas-Anforderungen"
    - [ ] DataFrame aus CSV laden
    - [ ] Datenbereinigung (NaN, Duplikate, Typen)
    - [ ] Datenzugriff mit loc/iloc
    - [ ] Gruppierung und Aggregation (groupby)
    - [ ] Transformation (map, apply, neue Spalten)
    - [ ] Pivot-Tabellen oder Crosstabs

---

## Option A: Verkaufsdaten-Analyse

Analysiere den Datensatz `verkaufsdaten.csv` mit Verkaufsinformationen.

### Aufgabe 1 – Daten laden und erkunden

- [ ] **Lade den Datensatz:**
    ```python
    import pandas as pd
    import numpy as np
    
    # Datensatz laden
    sales = pd.read_csv('../assets/files/verkaufsdaten.csv')
    
    # Grundlegende Exploration
    print(f"Shape: {sales.shape}")
    print(f"\nSpalten: {sales.columns.tolist()}")
    print(f"\nDatentypen:\n{sales.dtypes}")
    print(f"\nErste Zeilen:\n{sales.head()}")
    ```

- [ ] **Fehlende Werte und Duplikate prüfen:**
    ```python
    print("\n=== Datenqualität ===")
    print(f"Fehlende Werte:\n{sales.isnull().sum()}")
    print(f"\nDuplikate: {sales.duplicated().sum()}")
    ```

### Aufgabe 2 – Datenbereinigung

- [ ] **Bereinigungspipeline erstellen:**
    ```python
    def clean_sales_data(df):
        """Bereinige Verkaufsdaten"""
        df = df.copy()
        
        # Deine Bereinigungsschritte hier...
        # - Fehlende Werte behandeln
        # - Datentypen korrigieren
        # - Duplikate entfernen
        # - Ausreißer prüfen
        
        return df
    
    sales_clean = clean_sales_data(sales)
    ```

### Aufgabe 3 – NumPy-Analyse

- [ ] **Numerische Analyse mit NumPy:**
    ```python
    # Extrahiere numerische Spalte als NumPy Array
    # Beispiel: Umsatz
    # umsatz = sales_clean['Umsatz'].values
    
    # Berechne Statistiken
    # Wende Boolean Indexing an
    # Führe vektorisierte Berechnungen durch
    ```

### Aufgabe 4 – Pandas-Aggregation

- [ ] **Gruppierte Analysen:**
    ```python
    # Gruppiere nach relevanten Kategorien
    # Berechne Aggregationen (sum, mean, count, etc.)
    # Erstelle Pivot-Tabellen
    ```

### Aufgabe 5 – Erkenntnisse dokumentieren

- [ ] **Erstelle eine Zusammenfassung:**
    ```python
    # Fasse die wichtigsten Erkenntnisse zusammen
    # Beantworte Geschäftsfragen:
    # - Welche Produkte/Kategorien sind am erfolgreichsten?
    # - Gibt es saisonale Muster?
    # - Welche Kunden/Regionen sind am profitabelsten?
    ```

---

## Option B: Freie Datensatzwahl

Wähle einen eigenen Datensatz und analysiere ihn vollständig.

### Mögliche Datenquellen

!!! tip "Datensatz-Quellen"
    - [Kaggle Datasets](https://www.kaggle.com/datasets)
    - [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/index.php)
    - [Data.gov](https://www.data.gov/)
    - [Google Dataset Search](https://datasetsearch.research.google.com/)
    - Vorhandene Datensätze im Kurs (Taxi, Students, MBA, Sharks, Games)

### Anforderungen an den Datensatz

- [ ] Mindestens 500 Zeilen
- [ ] Mindestens 5 Spalten
- [ ] Mix aus numerischen und kategorischen Spalten
- [ ] Echte Daten (nicht synthetisch generiert)

### Projekt-Template

```python
# ============================================
# ABSCHLUSSPROJEKT: [Dein Projekt-Titel]
# ============================================

import pandas as pd
import numpy as np

# ============================================
# PHASE 1: Daten laden
# ============================================

# Datensatz laden
df = pd.read_csv('dein_datensatz.csv')

# Erste Exploration
print("=== DATENSATZ ÜBERSICHT ===")
print(f"Shape: {df.shape}")
print(f"Spalten: {df.columns.tolist()}")
df.info()

# ============================================
# PHASE 2: Datenbereinigung
# ============================================

print("\n=== DATENBEREINIGUNG ===")

# Fehlende Werte
print(f"Fehlende Werte:\n{df.isnull().sum()}")

# Duplikate
print(f"Duplikate: {df.duplicated().sum()}")

# Bereinigungsschritte...

# ============================================
# PHASE 3: NumPy-Analyse
# ============================================

print("\n=== NUMPY ANALYSE ===")

# Extrahiere numerische Daten
# Berechne Statistiken
# Boolean Indexing
# Vektorisierte Berechnungen

# ============================================
# PHASE 4: Pandas-Analyse
# ============================================

print("\n=== PANDAS ANALYSE ===")

# Deskriptive Statistik
# Gruppierungen
# Pivot-Tabellen
# Transformationen

# ============================================
# PHASE 5: Erkenntnisse
# ============================================

print("\n=== ZUSAMMENFASSUNG ===")

# Fasse deine wichtigsten Erkenntnisse zusammen
```

---

## Bewertungskriterien

| Kriterium | Punkte |
|-----------|--------|
| **Datenbereinigung** | 20 |
| - Fehlende Werte behandelt | 5 |
| - Datentypen korrekt | 5 |
| - Duplikate/Ausreißer geprüft | 5 |
| - Code sauber dokumentiert | 5 |
| **NumPy-Techniken** | 25 |
| - Array-Operationen | 5 |
| - Statistische Funktionen | 10 |
| - Boolean Indexing | 5 |
| - Vektorisierung | 5 |
| **Pandas-Techniken** | 35 |
| - Datenzugriff (loc/iloc) | 5 |
| - Gruppierung (groupby) | 10 |
| - Aggregation (agg) | 10 |
| - Transformation (map/apply) | 5 |
| - Pivot-Tabellen | 5 |
| **Analyse & Dokumentation** | 20 |
| - Sinnvolle Fragestellungen | 5 |
| - Aussagekräftige Ergebnisse | 10 |
| - Klare Zusammenfassung | 5 |
| **GESAMT** | **100** |

---

## Checkliste vor Abgabe

??? success "Vor der Abgabe prüfen"
    - [ ] Code läuft fehlerfrei durch
    - [ ] Alle Zellen haben Output
    - [ ] Kommentare erklären wichtige Schritte
    - [ ] NumPy-Anforderungen erfüllt
    - [ ] Pandas-Anforderungen erfüllt
    - [ ] Zusammenfassung der Erkenntnisse vorhanden
    - [ ] Datensatz ist im Repository enthalten (oder verlinkt)

---

## Beispiel: Mini-Analyse

Hier ein Beispiel für eine strukturierte Analyse:

```python
# ============================================
# BEISPIEL: Games-Datensatz Kurzanalyse
# ============================================

import pandas as pd
import numpy as np

# Laden
games = pd.read_csv('../assets/files/games.csv')

# === NUMPY ===
# Bewertungen als Array
if 'User_Score' in games.columns:
    scores = pd.to_numeric(games['User_Score'], errors='coerce').dropna().values
    
    print("=== NumPy Analyse ===")
    print(f"Durchschnitt: {np.mean(scores):.2f}")
    print(f"Standardabweichung: {np.std(scores):.2f}")
    print(f"Median: {np.median(scores):.2f}")
    
    # Boolean Indexing: Überdurchschnittliche Spiele
    ueberdurchschnitt = scores[scores > np.mean(scores)]
    print(f"Überdurchschnittlich: {len(ueberdurchschnitt)} ({len(ueberdurchschnitt)/len(scores)*100:.1f}%)")

# === PANDAS ===
print("\n=== Pandas Analyse ===")

# Gruppierung
print("\nSpiele pro Plattform (Top 5):")
print(games['Platform'].value_counts().head())

# Aggregation
if 'Genre' in games.columns and 'Global_Sales' in games.columns:
    genre_stats = games.groupby('Genre')['Global_Sales'].agg(['sum', 'mean', 'count'])
    genre_stats.columns = ['Gesamt', 'Durchschnitt', 'Anzahl']
    print("\nVerkäufe nach Genre:")
    print(genre_stats.sort_values('Gesamt', ascending=False).head())

# === ERKENNTNISSE ===
print("\n=== Erkenntnisse ===")
print("1. Die meisten Spiele erschienen für [Plattform X]")
print("2. Das erfolgreichste Genre ist [Genre Y]")
print("3. [Weitere Erkenntnisse...]")
```

---

## Hilfreiche Ressourcen

!!! info "Nützliche Links"
    - [:material-book-open-variant: NumPy Dokumentation](https://numpy.org/doc/)
    - [:material-book-open-variant: Pandas Dokumentation](https://pandas.pydata.org/docs/)
    - [:material-book-open-variant: Alle Infoblätter](../index.md)

---

Viel Erfolg bei deinem Abschlussprojekt! 🚀
