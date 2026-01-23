# Pandas – Datenzugriff mit loc und iloc

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- gezielt auf Zeilen und Spalten zugreifen mit `loc` und `iloc`
- den Unterschied zwischen label- und positionsbasiertem Zugriff verstehen
- Boolean Indexing für komplexe Filter anwenden
- Daten effizient auswählen und manipulieren

!!! note "Begleitende Infoblätter"
    - [:material-book-open-variant: Pandas Datenzugriff](../infoblaetter/pandas-datenzugriff.md) – loc, iloc, Boolean Indexing
    - [:material-book-open-variant: Pandas Grundlagen](../infoblaetter/pandas-grundlagen.md)

---

## Einführung

Pandas bietet verschiedene Wege, auf Daten zuzugreifen. Die wichtigsten sind `loc` (label-basiert) und `iloc` (positions-basiert).

```kroki-plantuml
@startuml
!theme plain
skinparam backgroundColor transparent

rectangle "Datenzugriff" as main {
    rectangle "iloc" as iloc #lightcoral {
        rectangle "Positions-basiert\n[0, 1, 2, ...]" as iloc_desc
    }
    
    rectangle "loc" as loc #lightblue {
        rectangle "Label-basiert\n['Name', 'Alter']" as loc_desc
    }
    
    rectangle "Boolean" as bool #lightgreen {
        rectangle "Bedingungs-basiert\n[True, False, ...]" as bool_desc
    }
}

note bottom of iloc
  Wie bei NumPy/Listen:
  numerische Indizes
end note

note bottom of loc
  Mit Spaltennamen
  und Index-Labels
end note

note bottom of bool
  Filtert mit
  Bedingungen
end note
@enduml
```

---

## Aufgaben

### Aufgabe 1 – Datensatz laden

- [ ] **Lade den MBA-Decisions Datensatz:**
    ```python
    import pandas as pd
    import numpy as np
    
    # MBA Entscheidungen laden
    mba = pd.read_csv('../assets/files/mba_decisions.csv')
    
    print("Shape:", mba.shape)
    print("\nSpalten:")
    print(mba.columns.tolist())
    print("\nErste Zeilen:")
    print(mba.head())
    ```

- [ ] **Spalten verstehen:**
    ```python
    # Übersicht
    mba.info()
    
    # Was bedeuten die Spalten?
    # - Application_ID: Eindeutige ID der Bewerbung
    # - Gender: Geschlecht
    # - International: Internationaler Bewerber?
    # - GPA: Grade Point Average (Notenschnitt)
    # - Major: Studienfach
    # - Work_Experience: Berufserfahrung in Jahren
    # - Decision: Entscheidung (Admit/Waitlist/Deny)
    ```

---

### Aufgabe 2 – iloc: Positions-basierter Zugriff

`iloc` nutzt **numerische Positionen** (0-basiert), wie bei Listen.

![Positions-basiert](../assets/images/pandas/pd-datenzugriff-01.png)

- [ ] **Einzelne Zeile auswählen:**
    ```python
    # Erste Zeile (Index 0)
    print("Erste Zeile:")
    print(mba.iloc[0])
    
    print("\n--- Als DataFrame (mit doppelten Klammern) ---")
    print(mba.iloc[[0]])
    ```

- [ ] **Mehrere Zeilen auswählen:**
    ```python
    # Zeilen 0, 1, 2
    print("Erste drei Zeilen:")
    print(mba.iloc[0:3])  # Slicing (Ende exklusiv!)
    
    print("\n--- Bestimmte Zeilen ---")
    print(mba.iloc[[0, 5, 10]])  # Zeilen 0, 5, 10
    ```

- [ ] **Zeilen und Spalten auswählen:**
    ```python
    # Zeile 0, Spalte 3 (einzelner Wert)
    print(f"Wert [0, 3]: {mba.iloc[0, 3]}")
    
    # Zeilen 0-2, Spalten 0-3
    print("\nBereich [0:3, 0:4]:")
    print(mba.iloc[0:3, 0:4])
    
    # Bestimmte Zeilen, bestimmte Spalten
    print("\nZeilen [0,5,10], Spalten [1,3,5]:")
    print(mba.iloc[[0, 5, 10], [1, 3, 5]])
    ```

- [ ] **Letzte Zeilen:**
    ```python
    # Letzte 3 Zeilen (negative Indizes!)
    print("Letzte 3 Zeilen:")
    print(mba.iloc[-3:])
    
    # Vorletzte Zeile
    print("\nVorletzte Zeile:")
    print(mba.iloc[-2])
    ```

---

### Aufgabe 3 – loc: Label-basierter Zugriff

`loc` nutzt **Labels** (Spaltennamen und Index-Werte).

![Label-basiert](../assets/images/pandas/pd-datenzugriff-02.png)

- [ ] **Spalten mit Namen auswählen:**
    ```python
    # Eine Spalte
    print("GPA Spalte (erste 5):")
    print(mba.loc[:, 'GPA'].head())  # : = alle Zeilen
    
    # Mehrere Spalten
    print("\nMehrere Spalten:")
    print(mba.loc[:, ['Gender', 'GPA', 'Decision']].head())
    ```

- [ ] **Zeilen und Spalten kombinieren:**
    ```python
    # Zeilen 0-4, bestimmte Spalten
    # ACHTUNG: bei loc ist das Ende INKLUSIV!
    print("Zeilen 0-4 (inklusiv), Spalten Gender bis GPA:")
    print(mba.loc[0:4, 'Gender':'GPA'])
    ```

- [ ] **Wichtiger Unterschied iloc vs loc bei Slicing:**
    ```python
    print("iloc[0:3] -> Zeilen 0, 1, 2 (Ende exklusiv)")
    print(mba.iloc[0:3])
    
    print("\nloc[0:3] -> Zeilen 0, 1, 2, 3 (Ende inklusiv!)")
    print(mba.loc[0:3])
    ```

!!! warning "Vorsicht bei loc-Slicing"
    Bei `loc` ist das Ende **inklusiv**: `loc[0:3]` gibt 4 Zeilen zurück!
    Bei `iloc` ist das Ende **exklusiv**: `iloc[0:3]` gibt 3 Zeilen zurück!

---

### Aufgabe 4 – Boolean Indexing

Filtere Daten mit Bedingungen – der mächtigste Zugriffsmodus!

![Boolean Indexing](../assets/images/pandas/pd-boolean-indexing-01.png)

- [ ] **Einfache Bedingung:**
    ```python
    # Alle mit GPA > 3.5
    hoher_gpa = mba[mba['GPA'] > 3.5]
    
    print(f"Bewerber mit GPA > 3.5: {len(hoher_gpa)}")
    print(hoher_gpa.head())
    ```

- [ ] **Verstehe, was passiert:**
    ```python
    # Schritt 1: Bedingung erstellt Boolean-Series
    bedingung = mba['GPA'] > 3.5
    print("Boolean Series (erste 10):")
    print(bedingung.head(10))
    print(f"\nAnzahl True: {bedingung.sum()}")
    
    # Schritt 2: Boolean-Series filtert DataFrame
    gefiltert = mba[bedingung]
    print(f"\nGefilterte Zeilen: {len(gefiltert)}")
    ```

- [ ] **Mehrere Bedingungen kombinieren:**
    ```python
    # UND: & mit Klammern!
    elite = mba[(mba['GPA'] > 3.5) & (mba['Work_Experience'] > 5)]
    print(f"Elite-Bewerber (GPA>3.5 & Erfahrung>5): {len(elite)}")
    print(elite.head())
    
    # ODER: |
    interessant = mba[(mba['GPA'] > 3.8) | (mba['Work_Experience'] > 10)]
    print(f"\nSehr hoher GPA ODER viel Erfahrung: {len(interessant)}")
    
    # NICHT: ~
    nicht_international = mba[~(mba['International'] == 'Yes')]
    # oder einfacher:
    nicht_international = mba[mba['International'] != 'Yes']
    print(f"\nNicht-internationale Bewerber: {len(nicht_international)}")
    ```

---

### Aufgabe 5 – Filtern mit Textbedingungen

- [ ] **Gleichheit bei Text:**
    ```python
    # Nur Aufnahmen (Admit)
    aufgenommen = mba[mba['Decision'] == 'Admit']
    print(f"Aufgenommene: {len(aufgenommen)}")
    
    # Abgelehnte
    abgelehnt = mba[mba['Decision'] == 'Deny']
    print(f"Abgelehnte: {len(abgelehnt)}")
    ```

- [ ] **Mehrere Werte mit isin():**
    ```python
    # STEM-Fächer
    stem_faecher = ['Science', 'Engineering', 'Technology']
    
    # Prüfe welche Majors es gibt
    print("Verfügbare Majors:")
    print(mba['Major'].unique())
    
    # Filter mit isin
    stem = mba[mba['Major'].isin(['STEM', 'Science & Technology'])]
    print(f"\nSTEM-Bewerber: {len(stem)}")
    ```

- [ ] **Textsuche mit str-Methoden:**
    ```python
    # Falls Spalte 'Major' Textsuche erlaubt
    # Enthält "Business"
    # business = mba[mba['Major'].str.contains('Business', case=False, na=False)]
    
    # Beginnt mit
    # starts_with_s = mba[mba['Major'].str.startswith('S')]
    
    print("Verfügbare str-Methoden:")
    print([m for m in dir(mba['Major'].str) if not m.startswith('_')][:10])
    ```

---

### Aufgabe 6 – loc mit Bedingungen kombinieren

`loc` erlaubt Bedingungen UND Spaltenauswahl in einem Schritt.

- [ ] **Filtern und Spalten auswählen:**
    ```python
    # Aufgenommene mit hohem GPA, nur bestimmte Spalten
    ergebnis = mba.loc[
        (mba['Decision'] == 'Admit') & (mba['GPA'] > 3.5),
        ['Gender', 'GPA', 'Work_Experience', 'Major']
    ]
    
    print("Aufgenommene mit GPA > 3.5:")
    print(ergebnis.head(10))
    print(f"\nAnzahl: {len(ergebnis)}")
    ```

- [ ] **Werte ändern mit loc:**
    ```python
    # Kopie erstellen (Original nicht ändern!)
    mba_copy = mba.copy()
    
    # Erhöhe GPA aller Internationalen um 0.1 (Beispiel)
    mba_copy.loc[mba_copy['International'] == 'Yes', 'GPA'] += 0.1
    
    # Vergleiche
    print("Durchschnitt GPA vorher:")
    print(f"  International: {mba[mba['International'] == 'Yes']['GPA'].mean():.2f}")
    print(f"  Nicht-Int.: {mba[mba['International'] != 'Yes']['GPA'].mean():.2f}")
    
    print("\nDurchschnitt GPA nachher:")
    print(f"  International: {mba_copy[mba_copy['International'] == 'Yes']['GPA'].mean():.2f}")
    print(f"  Nicht-Int.: {mba_copy[mba_copy['International'] != 'Yes']['GPA'].mean():.2f}")
    ```

---

### Aufgabe 7 – Query-Methode als Alternative

Die `query()`-Methode ermöglicht SQL-ähnliche Filterung.

- [ ] **Einfache Queries:**
    ```python
    # Statt: mba[mba['GPA'] > 3.5]
    ergebnis = mba.query('GPA > 3.5')
    print(f"GPA > 3.5: {len(ergebnis)}")
    
    # Mit Strings (Anführungszeichen beachten!)
    ergebnis = mba.query('Decision == "Admit"')
    print(f"Admit: {len(ergebnis)}")
    ```

- [ ] **Komplexe Queries:**
    ```python
    # Kombinierte Bedingungen
    ergebnis = mba.query('GPA > 3.5 and Work_Experience > 3')
    print(f"GPA>3.5 und Erfahrung>3: {len(ergebnis)}")
    
    # Mit or
    ergebnis = mba.query('GPA > 3.8 or Work_Experience > 8')
    print(f"GPA>3.8 oder Erfahrung>8: {len(ergebnis)}")
    
    # Mit Variablen (@ Präfix)
    min_gpa = 3.5
    min_exp = 5
    ergebnis = mba.query('GPA > @min_gpa and Work_Experience > @min_exp')
    print(f"Variablen-Query: {len(ergebnis)}")
    ```

!!! tip "query() Vorteile"
    - Lesbarere Syntax bei komplexen Bedingungen
    - Keine Klammern und & nötig
    - Variablen mit `@` einbinden
    - Schneller bei sehr großen DataFrames

---

### Aufgabe 8 – Praktische Analysen

- [ ] **Analyse: Aufnahmekriterien erkunden:**
    ```python
    print("=== Aufnahme-Analyse ===")
    
    # Durchschnitte nach Entscheidung
    for decision in mba['Decision'].unique():
        subset = mba[mba['Decision'] == decision]
        print(f"\n{decision}:")
        print(f"  Anzahl: {len(subset)}")
        print(f"  Ø GPA: {subset['GPA'].mean():.2f}")
        print(f"  Ø Erfahrung: {subset['Work_Experience'].mean():.1f} Jahre")
    ```

- [ ] **Vergleich International vs. Nicht-International:**
    ```python
    print("\n=== International vs. Nicht-International ===")
    
    for intl in mba['International'].unique():
        subset = mba[mba['International'] == intl]
        admitted = subset[subset['Decision'] == 'Admit']
        
        print(f"\n{intl}:")
        print(f"  Bewerber: {len(subset)}")
        print(f"  Aufgenommen: {len(admitted)} ({len(admitted)/len(subset)*100:.1f}%)")
        print(f"  Ø GPA: {subset['GPA'].mean():.2f}")
    ```

- [ ] **Top-Bewerber finden:**
    ```python
    print("\n=== Top 10 nach GPA ===")
    top_gpa = mba.nlargest(10, 'GPA')[['Gender', 'GPA', 'Work_Experience', 'Decision']]
    print(top_gpa)
    
    print("\n=== Meiste Erfahrung ===")
    top_exp = mba.nlargest(5, 'Work_Experience')[['Gender', 'GPA', 'Work_Experience', 'Decision']]
    print(top_exp)
    ```

- [ ] **Kritische Fälle analysieren:**
    ```python
    print("\n=== Überraschende Ablehnungen ===")
    # Hoher GPA aber abgelehnt
    surprise_deny = mba[(mba['GPA'] > 3.7) & (mba['Decision'] == 'Deny')]
    print(f"Abgelehnt trotz GPA > 3.7: {len(surprise_deny)}")
    if len(surprise_deny) > 0:
        print(surprise_deny[['Gender', 'GPA', 'Work_Experience', 'International', 'Major']])
    
    print("\n=== Überraschende Aufnahmen ===")
    # Niedriger GPA aber aufgenommen
    surprise_admit = mba[(mba['GPA'] < 3.0) & (mba['Decision'] == 'Admit')]
    print(f"Aufgenommen trotz GPA < 3.0: {len(surprise_admit)}")
    if len(surprise_admit) > 0:
        print(surprise_admit[['Gender', 'GPA', 'Work_Experience', 'International', 'Major']])
    ```

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **iloc**: Positions-basiert mit Zahlen, Ende exklusiv bei Slicing
    - **loc**: Label-basiert mit Namen, Ende inklusiv bei Slicing
    - **Boolean Indexing**: `df[bedingung]` für mächtige Filter
    - **Operatoren**: `&` (und), `|` (oder), `~` (nicht) + Klammern!
    - **isin()**: Prüfen ob Werte in Liste enthalten
    - **query()**: SQL-ähnliche Syntax für komplexe Filter
    - **nlargest/nsmallest**: Schnell Top-N finden

---

??? question "Selbstkontrolle"
    1. Was ist der Unterschied zwischen `df.iloc[0:3]` und `df.loc[0:3]`?
    2. Wie filterst du nach zwei Bedingungen mit AND?
    3. Wie wählst du Zeilen nach Bedingung UND nur bestimmte Spalten aus?
    4. Was macht `df[df['col'].isin(['A', 'B'])]`?
    
    ??? success "Antworten"
        1. `iloc[0:3]` gibt 3 Zeilen (0,1,2), `loc[0:3]` gibt 4 Zeilen (0,1,2,3) weil Ende inklusiv
        2. `df[(df['a'] > 5) & (df['b'] < 10)]` – Klammern und `&` wichtig!
        3. `df.loc[df['a'] > 5, ['spalte1', 'spalte2']]`
        4. Filtert alle Zeilen, wo 'col' entweder 'A' oder 'B' enthält
