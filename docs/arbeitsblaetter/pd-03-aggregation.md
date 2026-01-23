# Pandas – Aggregation & Gruppierung

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- Daten mit `groupby()` gruppieren und aggregieren
- verschiedene Aggregatfunktionen anwenden
- mehrere Aggregationen gleichzeitig ausführen
- Pivot-Tabellen für Kreuztabellen erstellen

!!! note "Begleitende Infoblätter"
    - [:material-book-open-variant: Pandas Aggregation](../infoblaetter/pandas-aggregation.md) – groupby, agg, pivot_table
    - [:material-book-open-variant: Pandas Datenzugriff](../infoblaetter/pandas-datenzugriff.md)

---

## Einführung

Gruppieren und Aggregieren folgt dem **Split-Apply-Combine** Paradigma:

```kroki-plantuml
@startuml
!theme plain
skinparam backgroundColor transparent

rectangle "Split-Apply-Combine" {
    rectangle "1. Split" as split #lightcoral {
        rectangle "Teile Daten\nnach Gruppen" as s
    }
    
    rectangle "2. Apply" as apply #lightyellow {
        rectangle "Wende Funktion\nauf jede Gruppe an" as a
    }
    
    rectangle "3. Combine" as combine #lightgreen {
        rectangle "Füge Ergebnisse\nzusammen" as c
    }
}

split --> apply
apply --> combine
@enduml
```

---

## Aufgaben

### Aufgabe 1 – Datensatz laden und verstehen

- [ ] **Lade den MBA-Datensatz:**
    ```python
    import pandas as pd
    import numpy as np
    
    mba = pd.read_csv('../assets/files/mba_decisions.csv')
    
    print("Shape:", mba.shape)
    print("\nSpalten:", mba.columns.tolist())
    print("\nErste Zeilen:")
    print(mba.head())
    ```

- [ ] **Überblick über kategorische Spalten:**
    ```python
    print("=== Kategorische Spalten ===")
    
    for col in ['Gender', 'International', 'Major', 'Decision']:
        if col in mba.columns:
            print(f"\n{col}:")
            print(mba[col].value_counts())
    ```

---

### Aufgabe 2 – Grundlagen von groupby()

```kroki-plantuml
@startuml
!theme plain
skinparam backgroundColor transparent

rectangle "Split-Apply-Combine" {
    rectangle "Original DataFrame" as orig #white
    
    rectangle "Split" as split {
        rectangle "Gruppe A" as ga #lightcoral
        rectangle "Gruppe B" as gb #lightblue
        rectangle "Gruppe C" as gc #lightgreen
    }
    
    rectangle "Apply (mean)" as apply {
        rectangle "mean(A)" as ma #lightcoral
        rectangle "mean(B)" as mb #lightblue
        rectangle "mean(C)" as mc #lightgreen
    }
    
    rectangle "Combine" as result #lightyellow {
        rectangle "Ergebnis" as res
    }
}

orig --> split
split --> apply
apply --> result
@enduml
```

- [ ] **Nach einer Spalte gruppieren:**
    ```python
    # Gruppiere nach Decision
    grouped = mba.groupby('Decision')
    
    # Was ist das für ein Objekt?
    print(f"Typ: {type(grouped)}")
    print(f"Anzahl Gruppen: {grouped.ngroups}")
    print(f"Gruppen-Keys: {list(grouped.groups.keys())}")
    ```

- [ ] **Einfache Aggregation:**
    ```python
    # Durchschnittswerte pro Gruppe
    print("Durchschnittswerte nach Entscheidung:")
    print(grouped.mean(numeric_only=True))
    ```

- [ ] **Auf eine Spalte anwenden:**
    ```python
    # Nur GPA aggregieren
    print("\nGPA nach Entscheidung:")
    print(mba.groupby('Decision')['GPA'].mean())
    
    print("\nWork_Experience nach Entscheidung:")
    print(mba.groupby('Decision')['Work_Experience'].mean())
    ```

---

### Aufgabe 3 – Verschiedene Aggregatfunktionen

- [ ] **Standardfunktionen:**
    ```python
    print("=== Aggregatfunktionen auf GPA ===")
    
    gpa_stats = mba.groupby('Decision')['GPA'].agg([
        'count',    # Anzahl
        'mean',     # Mittelwert
        'std',      # Standardabweichung
        'min',      # Minimum
        'max',      # Maximum
        'median'    # Median
    ])
    
    print(gpa_stats)
    ```

- [ ] **Einzelne Funktionen aufrufen:**
    ```python
    print("\n=== Einzelne Funktionen ===")
    print(f"Summe Erfahrung pro Gruppe:\n{mba.groupby('Decision')['Work_Experience'].sum()}")
    print(f"\nAnzahl pro Gruppe:\n{mba.groupby('Decision').size()}")
    print(f"\nErste 2 pro Gruppe:\n{mba.groupby('Decision').head(2)}")
    ```

- [ ] **Perzentile:**
    ```python
    # Quantile
    print("\n=== Quartile GPA nach Decision ===")
    quartile = mba.groupby('Decision')['GPA'].quantile([0.25, 0.5, 0.75])
    print(quartile)
    ```

---

### Aufgabe 4 – Mehrere Spalten gruppieren

```kroki-plantuml
@startuml
!theme plain
skinparam backgroundColor transparent

rectangle "Multi-Level Gruppierung" {
    rectangle "groupby(['Gender', 'Decision'])" as cmd
    
    rectangle "Hierarchische Gruppen" as groups {
        rectangle "Female" as f {
            rectangle "F-Admit" as fa #lightgreen
            rectangle "F-Deny" as fd #lightcoral
        }
        rectangle "Male" as m {
            rectangle "M-Admit" as ma #lightgreen
            rectangle "M-Deny" as md #lightcoral
        }
    }
    
    rectangle "Ergebnis: MultiIndex" as result #lightyellow
}

cmd --> groups
groups --> result
@enduml
```

- [ ] **Gruppierung nach zwei Spalten:**
    ```python
    # Nach Gender UND Decision
    multi_group = mba.groupby(['Gender', 'Decision'])['GPA'].mean()
    print("GPA nach Gender und Decision:")
    print(multi_group)
    ```

- [ ] **Ergebnis als DataFrame:**
    ```python
    # Mit reset_index() als normaler DataFrame
    multi_df = mba.groupby(['Gender', 'Decision'])['GPA'].mean().reset_index()
    multi_df.columns = ['Gender', 'Decision', 'Avg_GPA']
    print("\nAls DataFrame:")
    print(multi_df)
    ```

- [ ] **Unstack für bessere Darstellung:**
    ```python
    # Pivot-artige Darstellung
    print("\nUnstacked (Gender als Zeilen, Decision als Spalten):")
    print(mba.groupby(['Gender', 'Decision'])['GPA'].mean().unstack())
    ```

---

### Aufgabe 5 – agg() mit mehreren Funktionen

- [ ] **Verschiedene Funktionen auf eine Spalte:**
    ```python
    # Mehrere Aggregationen auf GPA
    agg_result = mba.groupby('Decision')['GPA'].agg(['mean', 'std', 'count'])
    print("Mehrere Funktionen auf GPA:")
    print(agg_result)
    ```

- [ ] **Verschiedene Funktionen auf verschiedene Spalten:**
    ```python
    # Dictionary-Syntax
    agg_dict = mba.groupby('Decision').agg({
        'GPA': ['mean', 'std'],
        'Work_Experience': ['mean', 'max'],
        'Application_ID': 'count'  # Anzahl Bewerber
    })
    
    print("Unterschiedliche Funktionen pro Spalte:")
    print(agg_dict)
    ```

- [ ] **Spalten umbenennen:**
    ```python
    # Mit Named Aggregations (moderner Ansatz)
    agg_named = mba.groupby('Decision').agg(
        avg_gpa=('GPA', 'mean'),
        std_gpa=('GPA', 'std'),
        avg_exp=('Work_Experience', 'mean'),
        max_exp=('Work_Experience', 'max'),
        count=('Application_ID', 'count')
    )
    
    print("\nMit benannten Spalten:")
    print(agg_named)
    ```

---

### Aufgabe 6 – Eigene Aggregatfunktionen

- [ ] **Lambda-Funktionen:**
    ```python
    # Range (Max - Min)
    print("GPA-Spannweite pro Decision:")
    print(mba.groupby('Decision')['GPA'].agg(lambda x: x.max() - x.min()))
    
    # Anteil über einem Schwellenwert
    print("\nAnteil GPA > 3.5 pro Decision:")
    print(mba.groupby('Decision')['GPA'].agg(lambda x: (x > 3.5).mean() * 100))
    ```

- [ ] **Eigene Funktion definieren:**
    ```python
    def iqr(series):
        """Interquartilsabstand"""
        return series.quantile(0.75) - series.quantile(0.25)
    
    def coeff_of_variation(series):
        """Variationskoeffizient in Prozent"""
        return (series.std() / series.mean()) * 100
    
    print("IQR und Variationskoeffizient:")
    print(mba.groupby('Decision')['GPA'].agg([iqr, coeff_of_variation]))
    ```

---

### Aufgabe 7 – Pivot-Tabellen

Pivot-Tabellen erstellen Kreuztabellen mit Aggregation.

```kroki-plantuml
@startuml
!theme plain
skinparam backgroundColor transparent

rectangle "Pivot-Tabelle" {
    rectangle "pivot_table(values='GPA',\nindex='Gender',\ncolumns='Decision')" as cmd
    
    map "Ergebnis" as result {
        . => Admit | Deny | Waitlist
        Female => 3.45 | 3.12 | 3.28
        Male => 3.52 | 3.08 | 3.31
    }
}

cmd --> result

note bottom of result
  Zeilen: Gender
  Spalten: Decision
  Werte: mean(GPA)
end note
@enduml
```

- [ ] **Einfache Pivot-Tabelle:**
    ```python
    # Durchschnittlicher GPA nach Gender und Decision
    pivot = pd.pivot_table(
        mba,
        values='GPA',
        index='Gender',
        columns='Decision',
        aggfunc='mean'
    )
    
    print("Pivot-Tabelle: GPA nach Gender und Decision")
    print(pivot)
    ```

- [ ] **Mit Summen (margins):**
    ```python
    # Mit Gesamtsummen
    pivot_margins = pd.pivot_table(
        mba,
        values='GPA',
        index='Gender',
        columns='Decision',
        aggfunc='mean',
        margins=True,
        margins_name='Gesamt'
    )
    
    print("\nMit Gesamtzeile/-spalte:")
    print(pivot_margins)
    ```

- [ ] **Mehrere Aggregatfunktionen:**
    ```python
    # Mean und Count
    pivot_multi = pd.pivot_table(
        mba,
        values='GPA',
        index='Gender',
        columns='Decision',
        aggfunc=['mean', 'count']
    )
    
    print("\nMehrere Funktionen:")
    print(pivot_multi)
    ```

- [ ] **Mehrere Werte:**
    ```python
    # GPA und Work_Experience
    pivot_values = pd.pivot_table(
        mba,
        values=['GPA', 'Work_Experience'],
        index='Gender',
        columns='Decision',
        aggfunc='mean'
    )
    
    print("\nMehrere Werte-Spalten:")
    print(pivot_values)
    ```

---

### Aufgabe 8 – Crosstab für Häufigkeiten

`pd.crosstab()` ist spezialisiert auf Häufigkeitstabellen.

- [ ] **Häufigkeitstabelle:**
    ```python
    # Anzahl Bewerber nach Gender und Decision
    cross = pd.crosstab(mba['Gender'], mba['Decision'])
    print("Crosstab: Anzahl nach Gender und Decision")
    print(cross)
    ```

- [ ] **Mit Prozenten:**
    ```python
    # Zeilen-Prozente (pro Gender)
    cross_row = pd.crosstab(mba['Gender'], mba['Decision'], normalize='index') * 100
    print("\nProzent pro Zeile (Gender):")
    print(cross_row.round(1))
    
    # Spalten-Prozente (pro Decision)
    cross_col = pd.crosstab(mba['Gender'], mba['Decision'], normalize='columns') * 100
    print("\nProzent pro Spalte (Decision):")
    print(cross_col.round(1))
    ```

- [ ] **Mit Margins:**
    ```python
    # Mit Summen
    cross_margins = pd.crosstab(
        mba['Gender'], 
        mba['Decision'],
        margins=True,
        margins_name='Summe'
    )
    print("\nMit Summen:")
    print(cross_margins)
    ```

---

### Aufgabe 9 – Praktische Analysen

- [ ] **Komplette Aufnahmestatistik:**
    ```python
    print("=== MBA Aufnahme-Statistik ===")
    
    # Aufnahmequoten nach verschiedenen Kriterien
    stats = mba.groupby('Decision').agg(
        Anzahl=('Application_ID', 'count'),
        GPA_mean=('GPA', 'mean'),
        GPA_median=('GPA', 'median'),
        GPA_min=('GPA', 'min'),
        GPA_max=('GPA', 'max'),
        Exp_mean=('Work_Experience', 'mean')
    ).round(2)
    
    print(stats)
    ```

- [ ] **Aufnahmequoten berechnen:**
    ```python
    print("\n=== Aufnahmequoten ===")
    
    # Gesamtquote
    total = len(mba)
    admitted = (mba['Decision'] == 'Admit').sum()
    print(f"Gesamt-Aufnahmequote: {admitted/total*100:.1f}%")
    
    # Nach Gender
    print("\nNach Gender:")
    for gender in mba['Gender'].unique():
        subset = mba[mba['Gender'] == gender]
        admitted = (subset['Decision'] == 'Admit').sum()
        print(f"  {gender}: {admitted/len(subset)*100:.1f}%")
    
    # Nach International
    print("\nNach International:")
    for intl in mba['International'].unique():
        subset = mba[mba['International'] == intl]
        admitted = (subset['Decision'] == 'Admit').sum()
        print(f"  {intl}: {admitted/len(subset)*100:.1f}%")
    ```

- [ ] **GPA-Schwellen analysieren:**
    ```python
    print("\n=== GPA-Schwellen und Aufnahmequoten ===")
    
    # GPA in Kategorien einteilen
    mba['GPA_Kategorie'] = pd.cut(
        mba['GPA'],
        bins=[0, 3.0, 3.3, 3.6, 3.8, 4.0],
        labels=['<3.0', '3.0-3.3', '3.3-3.6', '3.6-3.8', '3.8-4.0']
    )
    
    # Aufnahmequote pro Kategorie
    quote_pro_gpa = mba.groupby('GPA_Kategorie').apply(
        lambda x: (x['Decision'] == 'Admit').mean() * 100
    )
    
    print("Aufnahmequote nach GPA-Kategorie:")
    print(quote_pro_gpa.round(1))
    
    # Anzahl pro Kategorie
    print("\nAnzahl pro Kategorie:")
    print(mba['GPA_Kategorie'].value_counts().sort_index())
    ```

- [ ] **Interaktionseffekte:**
    ```python
    print("\n=== Interaktion: GPA-Kategorie × Gender ===")
    
    # Pivot: Aufnahmequote nach GPA und Gender
    interaction = pd.pivot_table(
        mba[mba['Decision'].isin(['Admit', 'Deny'])],
        values='Decision',
        index='GPA_Kategorie',
        columns='Gender',
        aggfunc=lambda x: (x == 'Admit').mean() * 100
    ).round(1)
    
    print("Aufnahmequote (%):")
    print(interaction)
    ```

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **groupby()**: Split-Apply-Combine für Gruppierungen
    - **Aggregatfunktionen**: mean, sum, count, std, min, max, median
    - **agg()**: Mehrere Funktionen auf einmal, pro Spalte unterschiedlich
    - **Named Aggregation**: Übersichtliche Spaltenbenennung
    - **pivot_table()**: Kreuztabellen mit Aggregation
    - **crosstab()**: Speziell für Häufigkeitstabellen

---

??? question "Selbstkontrolle"
    1. Was macht `df.groupby('A')['B'].mean()`?
    2. Wie aggregierst du verschiedene Funktionen auf verschiedene Spalten?
    3. Was ist der Unterschied zwischen `pivot_table` und `crosstab`?
    4. Wie bekommst du die Anzahl pro Gruppe?
    
    ??? success "Antworten"
        1. Gruppiert nach Spalte A und berechnet den Mittelwert von B pro Gruppe
        2. Mit Dictionary: `.agg({'spalte1': 'mean', 'spalte2': ['sum', 'count']})`
        3. `pivot_table` aggregiert beliebige Werte, `crosstab` ist für Häufigkeiten optimiert
        4. `.groupby('A').size()` oder `.groupby('A')['B'].count()` oder `.value_counts()`
