# Pandas – Transformation & Datenbereinigung

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- Spalten transformieren mit `map()`, `apply()`, und `applymap()`
- Daten bereinigen: fehlende Werte, Duplikate, Ausreißer
- neue Spalten berechnen und hinzufügen
- Datentypen konvertieren und kategorische Daten erstellen

!!! note "Begleitende Infoblätter"
    - [:material-book-open-variant: Pandas Transformation](../infoblaetter/pandas-transformation.md) – map, apply, applymap
    - [:material-book-open-variant: Datenbereinigung](../infoblaetter/datenbereinigung.md) – Cleaning Pipeline

---

## Einführung

Echte Daten sind selten perfekt. Transformation und Bereinigung sind oft die zeitaufwändigsten Schritte der Datenanalyse.

```kroki-plantuml
@startuml
!theme plain
skinparam backgroundColor transparent

rectangle "Daten-Pipeline" {
    rectangle "Rohdaten" as raw #lightcoral
    rectangle "Bereinigung" as clean #lightyellow {
        rectangle "• NaN behandeln\n• Duplikate entfernen\n• Ausreißer prüfen" as c
    }
    rectangle "Transformation" as trans #lightblue {
        rectangle "• Spalten berechnen\n• Werte umkodieren\n• Typen konvertieren" as t
    }
    rectangle "Saubere Daten" as final #lightgreen
}

raw --> clean
clean --> trans
trans --> final
@enduml
```

---

## Aufgaben

### Aufgabe 1 – Datensatz laden und Probleme identifizieren

- [ ] **Lade den MBA-Datensatz:**
    ```python
    import pandas as pd
    import numpy as np
    
    mba = pd.read_csv('../assets/files/mba_decisions.csv')
    
    print("Shape:", mba.shape)
    print("\nInfo:")
    mba.info()
    ```

- [ ] **Fehlende Werte finden:**
    ```python
    print("\n=== Fehlende Werte ===")
    missing = mba.isnull().sum()
    missing_pct = (mba.isnull().sum() / len(mba) * 100).round(2)
    
    missing_df = pd.DataFrame({
        'Fehlend': missing,
        'Prozent': missing_pct
    })
    
    print(missing_df[missing_df['Fehlend'] > 0])
    ```

- [ ] **Duplikate prüfen:**
    ```python
    print("\n=== Duplikate ===")
    print(f"Anzahl Duplikate: {mba.duplicated().sum()}")
    
    # Duplikate anzeigen
    if mba.duplicated().sum() > 0:
        print("\nDuplizierte Zeilen:")
        print(mba[mba.duplicated(keep=False)])
    ```

---

### Aufgabe 2 – Fehlende Werte behandeln

- [ ] **Verschiedene Strategien:**
    ```python
    # Kopie erstellen
    df = mba.copy()
    
    # Strategie 1: Zeilen mit NaN entfernen
    df_dropped = df.dropna()
    print(f"Nach dropna: {len(df_dropped)} von {len(df)} Zeilen")
    
    # Strategie 2: Nur bestimmte Spalten prüfen
    df_subset = df.dropna(subset=['GPA', 'Work_Experience'])
    print(f"Nach dropna(subset): {len(df_subset)} Zeilen")
    ```

- [ ] **Werte ersetzen (fillna):**
    ```python
    df = mba.copy()
    
    # Mit Mittelwert füllen
    gpa_mean = df['GPA'].mean()
    df['GPA_filled'] = df['GPA'].fillna(gpa_mean)
    print(f"GPA NaN gefüllt mit Mittelwert: {gpa_mean:.2f}")
    
    # Mit Median füllen (robuster)
    exp_median = df['Work_Experience'].median()
    df['Exp_filled'] = df['Work_Experience'].fillna(exp_median)
    print(f"Experience NaN gefüllt mit Median: {exp_median}")
    
    # Mit festen Werten füllen
    # df['Spalte'] = df['Spalte'].fillna(0)
    # df['Spalte'] = df['Spalte'].fillna('Unknown')
    ```

- [ ] **Forward/Backward Fill:**
    ```python
    # Bei Zeitreihen: vorherigen/nächsten Wert verwenden
    # df['Spalte'] = df['Spalte'].ffill()  # Forward fill
    # df['Spalte'] = df['Spalte'].bfill()  # Backward fill
    
    print("Forward/Backward Fill - nützlich bei Zeitreihen")
    ```

---

### Aufgabe 3 – Duplikate entfernen

- [ ] **Duplikate finden und entfernen:**
    ```python
    df = mba.copy()
    
    # Anzahl vor Entfernung
    print(f"Zeilen vorher: {len(df)}")
    
    # Entferne Duplikate (alle Spalten)
    df_unique = df.drop_duplicates()
    print(f"Zeilen nachher: {len(df_unique)}")
    
    # Nur bestimmte Spalten prüfen
    df_subset_unique = df.drop_duplicates(subset=['Gender', 'GPA', 'Work_Experience'])
    print(f"Nach Subset-Dedup: {len(df_subset_unique)}")
    ```

- [ ] **keep-Parameter:**
    ```python
    # keep='first' (Standard): Behalte erste Vorkommen
    # keep='last': Behalte letzte Vorkommen
    # keep=False: Entferne alle Duplikate
    
    print("\nDuplikat-Optionen:")
    print(f"  keep='first': {len(df.drop_duplicates(keep='first'))}")
    print(f"  keep='last': {len(df.drop_duplicates(keep='last'))}")
    print(f"  keep=False: {len(df.drop_duplicates(keep=False))}")
    ```

---

### Aufgabe 4 – Neue Spalten berechnen

```kroki-plantuml
@startuml
!theme plain
skinparam backgroundColor transparent

rectangle "Neue Spalten erstellen" {
    rectangle "Methoden" as methods {
        rectangle "Direkte Berechnung" as m1 #lightblue
        rectangle "np.where()" as m2 #lightgreen
        rectangle "np.select()" as m3 #lightyellow
        rectangle "apply()" as m4 #plum
    }
    
    rectangle "Beispiele" as examples {
        rectangle "df['neu'] = df['a'] + df['b']" as e1
        rectangle "df['kat'] = np.where(df['x']>5, 'hoch', 'niedrig')" as e2
        rectangle "df['rating'] = np.select([bed1, bed2], ['A', 'B'], 'C')" as e3
        rectangle "df['result'] = df.apply(func, axis=1)" as e4
    }
}

m1 --> e1
m2 --> e2
m3 --> e3
m4 --> e4
@enduml
```

- [ ] **Direkte Berechnung:**
    ```python
    df = mba.copy()
    
    # GPA als Prozent (von 4.0)
    df['GPA_Prozent'] = (df['GPA'] / 4.0 * 100).round(1)
    
    # Erfahrungskategorien
    df['Exp_Jahre_Group'] = pd.cut(
        df['Work_Experience'],
        bins=[0, 2, 5, 10, np.inf],
        labels=['Junior', 'Mid', 'Senior', 'Expert']
    )
    
    print("Neue Spalten:")
    print(df[['GPA', 'GPA_Prozent', 'Work_Experience', 'Exp_Jahre_Group']].head(10))
    ```

- [ ] **Bedingte Spalten:**
    ```python
    # Mit np.where
    df['High_GPA'] = np.where(df['GPA'] >= 3.5, 'Ja', 'Nein')
    
    # Mit np.select für mehrere Bedingungen
    conditions = [
        df['GPA'] >= 3.8,
        df['GPA'] >= 3.5,
        df['GPA'] >= 3.0,
    ]
    choices = ['Excellent', 'Good', 'Average']
    df['GPA_Rating'] = np.select(conditions, choices, default='Below Average')
    
    print("\nBedingte Spalten:")
    print(df['GPA_Rating'].value_counts())
    ```

- [ ] **Kombinierte Metriken:**
    ```python
    # Scoring: GPA + Erfahrung gewichtet
    df['Score'] = (df['GPA'] * 0.6 + df['Work_Experience'] * 0.1).round(2)
    
    print("\nScore-Statistik nach Decision:")
    print(df.groupby('Decision')['Score'].agg(['mean', 'std', 'min', 'max']).round(2))
    ```

---

### Aufgabe 5 – map() für Wertersetzung

`map()` ersetzt Werte basierend auf einem Dictionary oder einer Funktion.

- [ ] **Dictionary-Mapping:**
    ```python
    df = mba.copy()
    
    # Decision übersetzen
    decision_de = {
        'Admit': 'Aufgenommen',
        'Deny': 'Abgelehnt',
        'Waitlist': 'Warteliste'
    }
    df['Decision_DE'] = df['Decision'].map(decision_de)
    
    print("Decision-Übersetzung:")
    print(df[['Decision', 'Decision_DE']].head())
    ```

- [ ] **Funktion-Mapping:**
    ```python
    # GPA Buchstabennoten
    def gpa_to_grade(gpa):
        if pd.isna(gpa):
            return None
        elif gpa >= 3.7:
            return 'A'
        elif gpa >= 3.0:
            return 'B'
        elif gpa >= 2.0:
            return 'C'
        else:
            return 'D'
    
    df['Grade'] = df['GPA'].map(gpa_to_grade)
    print("\nGPA zu Buchstabennoten:")
    print(df['Grade'].value_counts())
    ```

---

### Aufgabe 6 – apply() für komplexe Transformationen

`apply()` wendet eine Funktion auf Zeilen oder Spalten an.

- [ ] **apply auf Spalte (Series):**
    ```python
    df = mba.copy()
    
    # Lambda für einfache Transformationen
    df['GPA_rounded'] = df['GPA'].apply(lambda x: round(x, 1) if pd.notna(x) else x)
    
    print("GPA gerundet:")
    print(df[['GPA', 'GPA_rounded']].head())
    ```

- [ ] **apply auf Zeilen (axis=1):**
    ```python
    # Berechne Profil-String aus mehreren Spalten
    def create_profile(row):
        return f"{row['Gender']}, GPA={row['GPA']:.1f}, Exp={row['Work_Experience']}y"
    
    df['Profile'] = df.apply(create_profile, axis=1)
    print("\nProfilstring:")
    print(df['Profile'].head())
    ```

- [ ] **Komplexe Logik:**
    ```python
    # Vorhersage-Funktion
    def predict_admission(row):
        score = 0
        score += row['GPA'] * 10
        score += row['Work_Experience'] * 2
        if row['International'] == 'Yes':
            score += 5
        
        if score >= 45:
            return 'Likely Admit'
        elif score >= 35:
            return 'Uncertain'
        else:
            return 'Unlikely'
    
    df['Prediction'] = df.apply(predict_admission, axis=1)
    
    # Vergleiche mit tatsächlicher Entscheidung
    print("\nVorhersage vs. Realität:")
    print(pd.crosstab(df['Prediction'], df['Decision']))
    ```

---

### Aufgabe 7 – Datentypen konvertieren

- [ ] **Strings zu numerisch:**
    ```python
    df = mba.copy()
    
    # Falls GPA als String geladen wurde
    # df['GPA'] = pd.to_numeric(df['GPA'], errors='coerce')
    
    print("Aktuelle Datentypen:")
    print(df.dtypes)
    ```

- [ ] **Kategorische Daten:**
    ```python
    # Konvertiere zu Category (speichereffizient)
    df['Decision_cat'] = df['Decision'].astype('category')
    
    print(f"\nSpeichervergleich Decision:")
    print(f"  Object: {df['Decision'].memory_usage()} Bytes")
    print(f"  Category: {df['Decision_cat'].memory_usage()} Bytes")
    
    # Ordinale Kategorie (mit Reihenfolge)
    df['Exp_cat'] = pd.Categorical(
        df['Work_Experience'].apply(
            lambda x: 'Low' if x < 3 else 'Medium' if x < 7 else 'High'
        ),
        categories=['Low', 'Medium', 'High'],
        ordered=True
    )
    
    print(f"\nOrdinale Kategorien:")
    print(df['Exp_cat'].cat.categories)
    print(f"Ist geordnet: {df['Exp_cat'].cat.ordered}")
    ```

---

### Aufgabe 8 – Strings bereinigen

- [ ] **Whitespace entfernen:**
    ```python
    df = mba.copy()
    
    # Alle String-Spalten trimmen
    for col in df.select_dtypes(include=['object']).columns:
        df[col] = df[col].str.strip()
    
    print("Strings getrimmt")
    ```

- [ ] **Groß-/Kleinschreibung:**
    ```python
    # Einheitliche Schreibweise
    df['Gender_lower'] = df['Gender'].str.lower()
    df['Gender_upper'] = df['Gender'].str.upper()
    df['Gender_title'] = df['Gender'].str.title()
    
    print("\nGender-Varianten:")
    print(df[['Gender', 'Gender_lower', 'Gender_upper', 'Gender_title']].head())
    ```

- [ ] **Suchen und Ersetzen:**
    ```python
    # Replace in Strings
    df['Major_clean'] = df['Major'].str.replace('&', 'and', regex=False)
    
    # Mit Regex
    # df['col'] = df['col'].str.replace(r'\d+', '', regex=True)  # Zahlen entfernen
    
    print("\nMajor bereinigt:")
    print(df[['Major', 'Major_clean']].drop_duplicates())
    ```

---

### Aufgabe 9 – Ausreißer behandeln

- [ ] **Ausreißer identifizieren:**
    ```python
    df = mba.copy()
    
    # IQR-Methode
    Q1 = df['GPA'].quantile(0.25)
    Q3 = df['GPA'].quantile(0.75)
    IQR = Q3 - Q1
    
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR
    
    outliers = df[(df['GPA'] < lower_bound) | (df['GPA'] > upper_bound)]
    
    print(f"GPA Ausreißer (IQR-Methode):")
    print(f"  Grenzen: [{lower_bound:.2f}, {upper_bound:.2f}]")
    print(f"  Anzahl Ausreißer: {len(outliers)}")
    
    if len(outliers) > 0:
        print(f"  Werte: {outliers['GPA'].tolist()}")
    ```

- [ ] **Ausreißer begrenzen (Capping):**
    ```python
    # Clip auf Grenzen
    df['GPA_clipped'] = df['GPA'].clip(lower=lower_bound, upper=upper_bound)
    
    print("\nNach Capping:")
    print(f"  Original-Bereich: [{df['GPA'].min():.2f}, {df['GPA'].max():.2f}]")
    print(f"  Geclipped-Bereich: [{df['GPA_clipped'].min():.2f}, {df['GPA_clipped'].max():.2f}]")
    ```

- [ ] **Z-Score Methode:**
    ```python
    # Z-Score für Ausreißererkennung
    from scipy import stats
    
    # Manuell berechnen
    mean = df['Work_Experience'].mean()
    std = df['Work_Experience'].std()
    df['Exp_zscore'] = (df['Work_Experience'] - mean) / std
    
    # Ausreißer: |z| > 3
    exp_outliers = df[abs(df['Exp_zscore']) > 3]
    print(f"\nWork_Experience Ausreißer (Z-Score > 3): {len(exp_outliers)}")
    ```

---

### Aufgabe 10 – Komplette Bereinigungspipeline

- [ ] **Pipeline zusammenfügen:**
    ```python
    def clean_mba_data(df):
        """Komplette Datenbereinigung für MBA-Datensatz"""
        
        # 1. Kopie erstellen
        df = df.copy()
        
        # 2. Strings bereinigen
        for col in df.select_dtypes(include=['object']).columns:
            df[col] = df[col].str.strip()
        
        # 3. Fehlende Werte
        df['GPA'] = df['GPA'].fillna(df['GPA'].median())
        df['Work_Experience'] = df['Work_Experience'].fillna(df['Work_Experience'].median())
        
        # 4. Duplikate entfernen
        df = df.drop_duplicates()
        
        # 5. Ausreißer cappen (GPA)
        Q1, Q3 = df['GPA'].quantile([0.25, 0.75])
        IQR = Q3 - Q1
        df['GPA'] = df['GPA'].clip(Q1 - 1.5*IQR, Q3 + 1.5*IQR)
        
        # 6. Neue Features
        df['GPA_Category'] = pd.cut(
            df['GPA'],
            bins=[0, 3.0, 3.5, 4.0],
            labels=['Low', 'Medium', 'High']
        )
        
        # 7. Kategorische Typen
        for col in ['Gender', 'International', 'Major', 'Decision']:
            if col in df.columns:
                df[col] = df[col].astype('category')
        
        return df
    
    # Pipeline anwenden
    mba_clean = clean_mba_data(mba)
    
    print("=== Bereinigte Daten ===")
    print(f"Shape: {mba_clean.shape}")
    print(f"\nDatentypen:")
    print(mba_clean.dtypes)
    print(f"\nFehlende Werte:")
    print(mba_clean.isnull().sum())
    ```

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **Fehlende Werte**: `dropna()`, `fillna()`, `ffill()`, `bfill()`
    - **Duplikate**: `duplicated()`, `drop_duplicates()`
    - **map()**: Wertersetzung mit Dictionary oder Funktion
    - **apply()**: Komplexe Transformationen (axis=0/1)
    - **Neue Spalten**: Direkte Berechnung, `np.where()`, `np.select()`
    - **Datentypen**: `astype()`, `pd.to_numeric()`, `pd.Categorical()`
    - **Ausreißer**: IQR-Methode, Z-Score, `clip()`

---

??? question "Selbstkontrolle"
    1. Wann verwendest du `map()` vs. `apply()`?
    2. Wie füllst du NaN mit dem Median einer Spalte?
    3. Was macht `df.clip(lower=0, upper=100)`?
    4. Wie erstellst du eine ordinale (geordnete) Kategorie?
    
    ??? success "Antworten"
        1. `map()` für einfache 1:1 Ersetzungen (Dictionary, Funktion auf einzelne Werte); `apply()` für komplexere Logik oder wenn mehrere Spalten nötig sind (axis=1)
        2. `df['col'] = df['col'].fillna(df['col'].median())`
        3. Begrenzt alle Werte auf den Bereich 0-100 (Capping)
        4. `pd.Categorical(values, categories=['A', 'B', 'C'], ordered=True)`
