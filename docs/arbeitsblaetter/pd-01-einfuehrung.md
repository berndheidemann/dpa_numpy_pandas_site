# Pandas – Einführung in DataFrames

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- Pandas importieren und DataFrames erstellen
- CSV-Dateien laden und erste Erkundungen durchführen
- Grundlegende Informationen über Datensätze abrufen
- Datentypen verstehen und konvertieren

!!! note "Begleitende Infoblätter"
    - [:material-book-open-variant: Pandas Grundlagen](../infoblaetter/pandas-grundlagen.md) – DataFrame & Series
    - [:material-book-open-variant: NumPy Grundlagen](../infoblaetter/numpy-grundlagen.md) – Arrays als Basis

---

## Einführung

Pandas ist **die** Python-Bibliothek für Datenanalyse. Sie baut auf NumPy auf und bietet mächtige Datenstrukturen für tabellarische Daten.

```kroki-plantuml
@startuml
!theme plain
skinparam backgroundColor transparent

package "Pandas Datenstrukturen" {
    rectangle "DataFrame" as df #lightblue {
        rectangle "Zeilen × Spalten\n(wie Excel-Tabelle)" as df_desc
    }
    
    rectangle "Series" as series #lightgreen {
        rectangle "Eine Spalte\n(wie Liste mit Index)" as series_desc
    }
}

note right of df
  Hauptstruktur für 
  Datenanalyse
end note

df --> series : "enthält"
@enduml
```

---

## Aufgaben

### Aufgabe 1 – Pandas importieren und erkunden

- [ ] **Importiere Pandas:**
    ```python
    import pandas as pd
    import numpy as np
    
    # Version prüfen
    print(f"Pandas Version: {pd.__version__}")
    ```

- [ ] **DataFrame manuell erstellen:**
    ```python
    # Aus einem Dictionary
    daten = {
        'Name': ['Anna', 'Ben', 'Clara', 'David'],
        'Alter': [22, 25, 28, 24],
        'Stadt': ['Berlin', 'München', 'Hamburg', 'Köln'],
        'Gehalt': [45000, 52000, 48000, 51000]
    }
    
    df = pd.DataFrame(daten)
    print(df)
    ```

- [ ] **Grundlegende Eigenschaften:**
    ```python
    print(f"\nShape (Zeilen, Spalten): {df.shape}")
    print(f"Spalten: {df.columns.tolist()}")
    print(f"Index: {df.index.tolist()}")
    print(f"Anzahl Werte: {df.size}")
    ```

---

### Aufgabe 2 – CSV-Dateien laden

- [ ] **Lade den Games-Datensatz:**
    ```python
    # CSV laden
    games = pd.read_csv('../assets/files/games.csv')
    
    print("Datensatz geladen!")
    print(f"Shape: {games.shape}")
    ```

- [ ] **Erste Zeilen anschauen:**
    ```python
    # Standard: erste 5 Zeilen
    print(games.head())
    
    # Erste 10 Zeilen
    print("\n--- Erste 10 Zeilen ---")
    print(games.head(10))
    
    # Letzte 3 Zeilen
    print("\n--- Letzte 3 Zeilen ---")
    print(games.tail(3))
    ```

- [ ] **Zufällige Stichprobe:**
    ```python
    # 5 zufällige Zeilen
    print("Zufällige Zeilen:")
    print(games.sample(5))
    ```

---

### Aufgabe 3 – Datensatz erkunden

- [ ] **Allgemeine Info:**
    ```python
    # Kompakte Übersicht
    print("=== Datensatz Info ===")
    games.info()
    ```

- [ ] **Datentypen anzeigen:**
    ```python
    print("\nDatentypen der Spalten:")
    print(games.dtypes)
    ```

- [ ] **Statistische Zusammenfassung:**
    ```python
    # Nur numerische Spalten
    print("\nStatistische Zusammenfassung:")
    print(games.describe())
    
    # Alle Spalten (inkl. kategorische)
    print("\nAlle Spalten:")
    print(games.describe(include='all'))
    ```

- [ ] **Fehlende Werte prüfen:**
    ```python
    print("\nFehlende Werte pro Spalte:")
    print(games.isnull().sum())
    
    # Prozentsatz fehlender Werte
    print("\nProzent fehlend:")
    print((games.isnull().sum() / len(games) * 100).round(2))
    ```

---

### Aufgabe 4 – Spalten auswählen

```kroki-plantuml
@startuml
!theme plain
skinparam backgroundColor transparent

rectangle "DataFrame" as df {
    rectangle "Name" as c1 #lightblue
    rectangle "Alter" as c2 #white
    rectangle "Stadt" as c3 #white
    rectangle "Gehalt" as c4 #white
}

rectangle "df['Name']" as result #lightblue {
    rectangle "Series" as s
}

df --> result : "Eine Spalte\n→ Series"

note bottom of result
  Typ: pandas.Series
end note
@enduml
```

- [ ] **Eine Spalte auswählen (Series):**
    ```python
    # Mit Punkt-Notation (wenn Spaltenname einfach)
    # name_series = games.Name
    
    # Mit Bracket-Notation (immer sicher)
    name_series = games['Name']
    
    print(type(name_series))
    print(name_series.head())
    ```

- [ ] **Mehrere Spalten auswählen (DataFrame):**
    ```python
    # Liste von Spaltennamen
    auswahl = games[['Name', 'Platform', 'Year_of_Release']]
    
    print(type(auswahl))
    print(auswahl.head())
    ```

- [ ] **Spalten umbenennen:**
    ```python
    # Kopie mit neuen Spaltennamen
    games_de = games.rename(columns={
        'Name': 'Spielname',
        'Year_of_Release': 'Erscheinungsjahr',
        'Platform': 'Plattform'
    })
    
    print(games_de.columns[:5].tolist())
    ```

---

### Aufgabe 5 – Datentypen verstehen und konvertieren

- [ ] **Datentypen überprüfen:**
    ```python
    print("Datentypen im Detail:")
    for col in games.columns:
        print(f"  {col}: {games[col].dtype}")
    ```

- [ ] **Typumwandlung:**
    ```python
    # Prüfe Year_of_Release
    print("\nJahr - Ursprünglicher Typ:", games['Year_of_Release'].dtype)
    
    # Falls float, in int konvertieren (nur wenn keine NaN!)
    # Erst NaN entfernen oder ersetzen
    games_clean = games.copy()
    games_clean['Year_of_Release'] = games_clean['Year_of_Release'].fillna(0)
    games_clean['Year_of_Release'] = games_clean['Year_of_Release'].astype(int)
    
    print("Nach Konvertierung:", games_clean['Year_of_Release'].dtype)
    print(games_clean['Year_of_Release'].head())
    ```

- [ ] **Kategorische Daten:**
    ```python
    # Platform als Kategorie (speichereffizienter)
    print("\nPlattform vor Konvertierung:")
    print(f"  Typ: {games['Platform'].dtype}")
    print(f"  Speicher: {games['Platform'].memory_usage()} Bytes")
    
    games['Platform_cat'] = games['Platform'].astype('category')
    
    print("\nPlattform nach Konvertierung:")
    print(f"  Typ: {games['Platform_cat'].dtype}")
    print(f"  Speicher: {games['Platform_cat'].memory_usage()} Bytes")
    print(f"  Kategorien: {games['Platform_cat'].cat.categories.tolist()[:5]}...")
    ```

---

### Aufgabe 6 – Eindeutige Werte und Häufigkeiten

- [ ] **Eindeutige Werte:**
    ```python
    # Wie viele verschiedene Plattformen?
    print(f"Anzahl Plattformen: {games['Platform'].nunique()}")
    
    # Welche Plattformen?
    print(f"\nPlattformen: {games['Platform'].unique()}")
    ```

- [ ] **Häufigkeiten zählen:**
    ```python
    # Top 10 Plattformen nach Anzahl Spiele
    print("Top 10 Plattformen:")
    print(games['Platform'].value_counts().head(10))
    
    # Als Prozentsatz
    print("\nTop 5 Plattformen (Prozent):")
    print(games['Platform'].value_counts(normalize=True).head(5) * 100)
    ```

- [ ] **Jahre mit den meisten Releases:**
    ```python
    print("\nTop 10 Release-Jahre:")
    print(games['Year_of_Release'].value_counts().head(10))
    ```

---

### Aufgabe 7 – Sortieren

- [ ] **Nach einer Spalte sortieren:**
    ```python
    # Nach Jahr sortieren (älteste zuerst)
    games_chronologisch = games.sort_values('Year_of_Release')
    print("Älteste Spiele:")
    print(games_chronologisch[['Name', 'Year_of_Release', 'Platform']].head())
    
    # Neueste zuerst
    games_neu = games.sort_values('Year_of_Release', ascending=False)
    print("\nNeueste Spiele:")
    print(games_neu[['Name', 'Year_of_Release', 'Platform']].head())
    ```

- [ ] **Nach mehreren Spalten sortieren:**
    ```python
    # Nach Plattform, dann Jahr
    sortiert = games.sort_values(['Platform', 'Year_of_Release'],
                                  ascending=[True, False])
    print("Sortiert nach Plattform, dann Jahr (absteigend):")
    print(sortiert[['Name', 'Platform', 'Year_of_Release']].head(10))
    ```

- [ ] **Index zurücksetzen:**
    ```python
    # Nach Sortierung Index neu setzen
    games_sorted = games.sort_values('Year_of_Release').reset_index(drop=True)
    print(f"Neuer Index: {games_sorted.index[:10].tolist()}")
    ```

---

### Aufgabe 8 – Erste Analysen

- [ ] **Fragen zum Games-Datensatz:**
    
    Beantworte folgende Fragen mit Pandas-Code:

    ```python
    # 1. Wie viele Spiele sind im Datensatz?
    print(f"Anzahl Spiele: {len(games)}")
    
    # 2. In welchem Jahr wurden die meisten Spiele veröffentlicht?
    top_jahr = games['Year_of_Release'].value_counts().idxmax()
    anzahl = games['Year_of_Release'].value_counts().max()
    print(f"Bestes Jahr: {top_jahr} ({anzahl} Spiele)")
    
    # 3. Welches Genre ist am häufigsten?
    if 'Genre' in games.columns:
        top_genre = games['Genre'].value_counts().idxmax()
        print(f"Häufigstes Genre: {top_genre}")
    
    # 4. Wie viele verschiedene Publisher gibt es?
    if 'Publisher' in games.columns:
        print(f"Anzahl Publisher: {games['Publisher'].nunique()}")
    
    # 5. Welche Spalten haben fehlende Werte?
    missing = games.isnull().sum()
    spalten_mit_nan = missing[missing > 0].index.tolist()
    print(f"Spalten mit NaN: {spalten_mit_nan}")
    ```

!!! tip "Methoden-Kette"
    Pandas erlaubt das Verketten von Methoden:
    ```python
    # Statt:
    temp = games.sort_values('Year_of_Release')
    ergebnis = temp.head(10)
    
    # Besser:
    ergebnis = games.sort_values('Year_of_Release').head(10)
    ```

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **DataFrame erstellen**: `pd.DataFrame(dict)` oder `pd.read_csv()`
    - **Erkunden**: `.head()`, `.tail()`, `.info()`, `.describe()`
    - **Spalten**: `df['col']` (Series) oder `df[['a', 'b']]` (DataFrame)
    - **Häufigkeiten**: `.value_counts()`, `.nunique()`, `.unique()`
    - **Sortieren**: `.sort_values()` mit `ascending=True/False`
    - **Datentypen**: `.dtypes`, `.astype()` zum Konvertieren

---

??? question "Selbstkontrolle"
    1. Was ist der Unterschied zwischen `df['col']` und `df[['col']]`?
    2. Wie findest du heraus, wie viele verschiedene Werte eine Spalte hat?
    3. Welche Methode zeigt Speicherverbrauch und Datentypen aller Spalten?
    4. Wie sortierst du absteigend nach einer Spalte?
    
    ??? success "Antworten"
        1. `df['col']` gibt eine Series zurück, `df[['col']]` gibt einen DataFrame (mit einer Spalte) zurück
        2. `df['col'].nunique()` für die Anzahl, `df['col'].unique()` für die Werte selbst
        3. `df.info()` zeigt kompakte Übersicht mit Datentypen und Non-Null-Counts
        4. `df.sort_values('spalte', ascending=False)`
