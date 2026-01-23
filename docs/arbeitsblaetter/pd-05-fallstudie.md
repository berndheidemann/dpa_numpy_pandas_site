# Pandas – Fallstudie Shark Attacks

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- einen komplexen, realen Datensatz selbstständig analysieren
- Datenbereinigung bei unstrukturierten Daten durchführen
- fortgeschrittene Pandas-Techniken anwenden
- aussagekräftige Erkenntnisse aus Daten ableiten

!!! note "Begleitende Infoblätter"
    - [:material-book-open-variant: Pandas Grundlagen](../infoblaetter/pandas-grundlagen.md)
    - [:material-book-open-variant: Pandas Aggregation](../infoblaetter/pandas-aggregation.md)
    - [:material-book-open-variant: Datenbereinigung](../infoblaetter/datenbereinigung.md)

---

## Einführung

Der Global Shark Attack File (GSAF) ist eine Datenbank aller dokumentierten Haiangriffe weltweit. Der Datensatz enthält viele fehlende Werte und unstrukturierte Textdaten – eine realistische Herausforderung!

```kroki-plantuml
@startuml
!theme plain
skinparam backgroundColor transparent

rectangle "Analyse-Workflow" {
    rectangle "1. Laden & Verstehen" as step1 #lightcoral {
        rectangle "• Shape, Spalten\n• Datentypen\n• Fehlende Werte" as s1
    }
    
    rectangle "2. Bereinigen" as step2 #lightyellow {
        rectangle "• Spalten auswählen\n• NaN behandeln\n• Typen korrigieren" as s2
    }
    
    rectangle "3. Analysieren" as step3 #lightblue {
        rectangle "• Verteilungen\n• Trends\n• Muster" as s3
    }
    
    rectangle "4. Erkenntnisse" as step4 #lightgreen {
        rectangle "• Zusammenfassen\n• Visualisieren\n• Interpretieren" as s4
    }
}

step1 --> step2
step2 --> step3
step3 --> step4
@enduml
```

---

## Aufgaben

### Aufgabe 1 – Daten laden und ersten Überblick gewinnen

- [ ] **Lade den Datensatz:**
    ```python
    import pandas as pd
    import numpy as np
    
    # Datensatz laden
    sharks = pd.read_csv('../assets/files/global_shark_attacks.csv',
                         encoding='latin-1')  # Encoding für Sonderzeichen
    
    print(f"Shape: {sharks.shape}")
    print(f"Spalten: {sharks.columns.tolist()}")
    ```

- [ ] **Erste Zeilen anschauen:**
    ```python
    print("\nErste 3 Zeilen:")
    print(sharks.head(3).T)  # Transponiert für bessere Lesbarkeit
    ```

- [ ] **Datentypen und fehlende Werte:**
    ```python
    print("\n=== Datensatz-Info ===")
    sharks.info()
    
    print("\n=== Fehlende Werte (Top 10) ===")
    missing = sharks.isnull().sum().sort_values(ascending=False)
    print(missing.head(10))
    print(f"\nGesamt fehlende Werte: {missing.sum()}")
    ```

---

### Aufgabe 2 – Relevante Spalten auswählen

Der Datensatz hat viele Spalten. Wähle die wichtigsten aus.

- [ ] **Spalten identifizieren:**
    ```python
    # Typische wichtige Spalten
    # (Namen können je nach Datensatz-Version variieren!)
    print("Alle Spaltennamen:")
    for i, col in enumerate(sharks.columns):
        print(f"  {i}: {col}")
    ```

- [ ] **Arbeits-DataFrame erstellen:**
    ```python
    # Relevante Spalten auswählen (Namen anpassen falls nötig!)
    # Typische Spalten: Year, Country, Area, Activity, Name, Sex, Age, Injury, Fatal
    
    # Versuche gängige Spaltennamen
    possible_cols = ['Year', 'Country', 'Area', 'Location', 'Activity', 
                     'Name', 'Sex', 'Age', 'Injury', 'Fatal (Y/N)', 
                     'Time', 'Species']
    
    # Nur vorhandene Spalten auswählen
    use_cols = [c for c in possible_cols if c in sharks.columns]
    print(f"Gefundene Spalten: {use_cols}")
    
    df = sharks[use_cols].copy()
    print(f"\nArbeits-DataFrame Shape: {df.shape}")
    ```

---

### Aufgabe 3 – Daten bereinigen

- [ ] **Jahr bereinigen:**
    ```python
    # Jahr sollte numerisch sein
    print("=== Jahr bereinigen ===")
    print(f"Datentyp vorher: {df['Year'].dtype}")
    print(f"Beispielwerte: {df['Year'].head(10).tolist()}")
    
    # Zu numerisch konvertieren (Fehler werden NaN)
    df['Year'] = pd.to_numeric(df['Year'], errors='coerce')
    
    # Unrealistische Jahre entfernen (vor 1800, nach aktuellem Jahr)
    import datetime
    current_year = datetime.datetime.now().year
    df = df[(df['Year'] >= 1800) & (df['Year'] <= current_year)]
    
    print(f"Nach Bereinigung: {df['Year'].min():.0f} - {df['Year'].max():.0f}")
    print(f"Anzahl nach Filter: {len(df)}")
    ```

- [ ] **Fatal (tödlich) bereinigen:**
    ```python
    print("\n=== Fatal bereinigen ===")
    if 'Fatal (Y/N)' in df.columns:
        print("Werte vorher:")
        print(df['Fatal (Y/N)'].value_counts(dropna=False))
        
        # Standardisieren
        df['Fatal'] = df['Fatal (Y/N)'].str.upper().str.strip()
        df['Fatal'] = df['Fatal'].map({'Y': True, 'N': False})
        
        print("\nWerte nachher:")
        print(df['Fatal'].value_counts(dropna=False))
    ```

- [ ] **Alter bereinigen:**
    ```python
    print("\n=== Alter bereinigen ===")
    print(f"Datentyp: {df['Age'].dtype}")
    print(f"Beispiele: {df['Age'].dropna().head(10).tolist()}")
    
    # Zu numerisch konvertieren
    df['Age'] = pd.to_numeric(df['Age'], errors='coerce')
    
    # Unrealistische Alter entfernen
    df.loc[(df['Age'] < 0) | (df['Age'] > 100), 'Age'] = np.nan
    
    print(f"\nAlter-Statistik:")
    print(df['Age'].describe())
    ```

- [ ] **Geschlecht standardisieren:**
    ```python
    print("\n=== Geschlecht bereinigen ===")
    print("Werte vorher:")
    print(df['Sex'].value_counts(dropna=False))
    
    df['Sex'] = df['Sex'].str.upper().str.strip()
    df['Sex'] = df['Sex'].map({'M': 'Male', 'F': 'Female'})
    
    print("\nWerte nachher:")
    print(df['Sex'].value_counts(dropna=False))
    ```

---

### Aufgabe 4 – Deskriptive Statistik

- [ ] **Grundstatistiken:**
    ```python
    print("=== Grundstatistiken ===")
    print(f"Anzahl Angriffe: {len(df)}")
    print(f"Zeitraum: {df['Year'].min():.0f} - {df['Year'].max():.0f}")
    print(f"Anzahl Länder: {df['Country'].nunique()}")
    ```

- [ ] **Top Länder:**
    ```python
    print("\n=== Top 10 Länder ===")
    top_countries = df['Country'].value_counts().head(10)
    print(top_countries)
    
    # Prozentsatz
    print(f"\nAnteil Top 10: {top_countries.sum() / len(df) * 100:.1f}%")
    ```

- [ ] **Häufigste Aktivitäten:**
    ```python
    print("\n=== Top 10 Aktivitäten ===")
    activities = df['Activity'].value_counts().head(10)
    print(activities)
    ```

- [ ] **Altersverteilung:**
    ```python
    print("\n=== Altersverteilung ===")
    print(df['Age'].describe())
    
    # Altersgruppen
    df['Age_Group'] = pd.cut(
        df['Age'],
        bins=[0, 10, 20, 30, 40, 50, 60, 100],
        labels=['0-10', '11-20', '21-30', '31-40', '41-50', '51-60', '60+']
    )
    
    print("\nNach Altersgruppe:")
    print(df['Age_Group'].value_counts().sort_index())
    ```

---

### Aufgabe 5 – Zeitliche Trends

- [ ] **Angriffe pro Jahr:**
    ```python
    print("=== Zeitlicher Trend ===")
    
    # Angriffe pro Jahr
    yearly = df.groupby('Year').size()
    
    print("Letzte 10 Jahre:")
    print(yearly.tail(10))
    
    # Trend berechnen
    recent = yearly[yearly.index >= 2000]
    print(f"\nDurchschnitt 2000-heute: {recent.mean():.1f} Angriffe/Jahr")
    print(f"Max: {recent.max()} ({recent.idxmax():.0f})")
    print(f"Min: {recent.min()} ({recent.idxmin():.0f})")
    ```

- [ ] **Dekaden-Analyse:**
    ```python
    # Dekade erstellen
    df['Decade'] = (df['Year'] // 10) * 10
    
    decade_stats = df.groupby('Decade').agg(
        Angriffe=('Year', 'count'),
        Fatal_Prozent=('Fatal', lambda x: x.mean() * 100 if x.notna().any() else np.nan)
    ).round(1)
    
    print("\nAngriffe pro Dekade:")
    print(decade_stats[decade_stats.index >= 1900])
    ```

- [ ] **Saisonale Muster (falls Time vorhanden):**
    ```python
    # Monat extrahieren (falls Date-Spalte vorhanden)
    # Oder aus 'Date' String parsen
    
    # Beispiel-Ansatz:
    # df['Month'] = pd.to_datetime(df['Date'], errors='coerce').dt.month
    
    print("\nSaisonale Analyse erfordert Datums-Parsing")
    print("(Datensatz-spezifisch)")
    ```

---

### Aufgabe 6 – Tödliche Angriffe analysieren

- [ ] **Tödlichkeitsrate:**
    ```python
    print("=== Tödlichkeit ===")
    
    # Gesamtrate
    fatal_rate = df['Fatal'].mean() * 100
    print(f"Gesamt-Tödlichkeitsrate: {fatal_rate:.1f}%")
    
    # Nach Dekade
    print("\nTödlichkeit nach Dekade:")
    fatal_by_decade = df.groupby('Decade')['Fatal'].mean() * 100
    print(fatal_by_decade[fatal_by_decade.index >= 1950].round(1))
    ```

- [ ] **Tödlichkeit nach Land:**
    ```python
    print("\n=== Tödlichkeit nach Land (Top 10 nach Anzahl) ===")
    
    # Nur Länder mit mindestens 50 Angriffen
    country_stats = df.groupby('Country').agg(
        Angriffe=('Year', 'count'),
        Tödlich=('Fatal', 'sum'),
        Rate=('Fatal', lambda x: x.mean() * 100)
    ).round(1)
    
    country_stats = country_stats[country_stats['Angriffe'] >= 50]
    country_stats = country_stats.sort_values('Angriffe', ascending=False)
    
    print(country_stats.head(10))
    ```

- [ ] **Tödlichkeit nach Aktivität:**
    ```python
    print("\n=== Tödlichkeit nach Aktivität (mind. 20 Fälle) ===")
    
    activity_stats = df.groupby('Activity').agg(
        Angriffe=('Year', 'count'),
        Rate=('Fatal', lambda x: x.mean() * 100)
    ).round(1)
    
    activity_stats = activity_stats[activity_stats['Angriffe'] >= 20]
    activity_stats = activity_stats.sort_values('Rate', ascending=False)
    
    print(activity_stats.head(10))
    ```

---

### Aufgabe 7 – Tiefere Analysen

- [ ] **Geschlechtervergleich:**
    ```python
    print("=== Geschlechtervergleich ===")
    
    gender_stats = df.groupby('Sex').agg(
        Anzahl=('Year', 'count'),
        Durchschnittsalter=('Age', 'mean'),
        Tödlichkeit=('Fatal', lambda x: x.mean() * 100)
    ).round(1)
    
    # Anteil berechnen
    gender_stats['Anteil_%'] = (gender_stats['Anzahl'] / gender_stats['Anzahl'].sum() * 100).round(1)
    
    print(gender_stats)
    ```

- [ ] **Altersanalyse:**
    ```python
    print("\n=== Alter und Tödlichkeit ===")
    
    # Tödlichkeit nach Altersgruppe
    age_fatal = df.groupby('Age_Group').agg(
        Anzahl=('Year', 'count'),
        Tödlichkeit=('Fatal', lambda x: x.mean() * 100)
    ).round(1)
    
    print(age_fatal)
    
    # Durchschnittsalter bei tödlichen vs. nicht-tödlichen
    print(f"\nDurchschnittsalter:")
    print(f"  Tödliche Angriffe: {df[df['Fatal'] == True]['Age'].mean():.1f}")
    print(f"  Nicht-tödliche: {df[df['Fatal'] == False]['Age'].mean():.1f}")
    ```

- [ ] **Länderprofile erstellen:**
    ```python
    print("\n=== Länderprofile (Top 5) ===")
    
    top5_countries = df['Country'].value_counts().head(5).index.tolist()
    
    for country in top5_countries:
        subset = df[df['Country'] == country]
        print(f"\n{country}:")
        print(f"  Angriffe: {len(subset)}")
        print(f"  Zeitraum: {subset['Year'].min():.0f}-{subset['Year'].max():.0f}")
        print(f"  Tödlichkeit: {subset['Fatal'].mean() * 100:.1f}%")
        print(f"  Top-Aktivität: {subset['Activity'].mode().iloc[0] if len(subset['Activity'].mode()) > 0 else 'N/A'}")
        print(f"  Durchschnittsalter: {subset['Age'].mean():.1f}")
    ```

---

### Aufgabe 8 – Pivot-Tabellen erstellen

- [ ] **Kreuztabelle Land × Dekade:**
    ```python
    print("=== Angriffe: Land × Dekade ===")
    
    # Top 5 Länder, ab 1950
    df_recent = df[(df['Decade'] >= 1950) & (df['Country'].isin(top5_countries))]
    
    pivot = pd.pivot_table(
        df_recent,
        values='Year',
        index='Country',
        columns='Decade',
        aggfunc='count',
        fill_value=0
    )
    
    print(pivot)
    ```

- [ ] **Tödlichkeit: Land × Aktivität:**
    ```python
    print("\n=== Tödlichkeit: Land × Aktivität (Top) ===")
    
    top_activities = df['Activity'].value_counts().head(5).index.tolist()
    df_filtered = df[df['Country'].isin(top5_countries) & df['Activity'].isin(top_activities)]
    
    pivot_fatal = pd.pivot_table(
        df_filtered,
        values='Fatal',
        index='Country',
        columns='Activity',
        aggfunc='mean'
    ) * 100
    
    print(pivot_fatal.round(1))
    ```

---

### Aufgabe 9 – Erkenntnisse dokumentieren

- [ ] **Zusammenfassung erstellen:**
    
    Fasse deine wichtigsten Erkenntnisse zusammen:

    ```python
    print("=" * 50)
    print("ZUSAMMENFASSUNG: Global Shark Attacks")
    print("=" * 50)
    
    print(f"\n📊 DATENSATZ")
    print(f"   • {len(df):,} dokumentierte Angriffe")
    print(f"   • Zeitraum: {df['Year'].min():.0f} - {df['Year'].max():.0f}")
    print(f"   • {df['Country'].nunique()} Länder")
    
    print(f"\n🦈 RISIKO")
    print(f"   • Gesamt-Tödlichkeitsrate: {df['Fatal'].mean() * 100:.1f}%")
    print(f"   • Gefährlichstes Land: {country_stats['Rate'].idxmax()}")
    print(f"   • Sicherste Aktivität: {activity_stats['Rate'].idxmin()}")
    
    print(f"\n👤 DEMOGRAFIE")
    print(f"   • Durchschnittsalter: {df['Age'].mean():.1f} Jahre")
    print(f"   • Männeranteil: {(df['Sex'] == 'Male').sum() / df['Sex'].notna().sum() * 100:.1f}%")
    
    print(f"\n📈 TRENDS")
    recent_decade = df[df['Year'] >= 2010]
    older_decade = df[(df['Year'] >= 1990) & (df['Year'] < 2000)]
    print(f"   • Angriffe 1990er: {len(older_decade)} / Dekade")
    print(f"   • Angriffe 2010er+: {len(recent_decade)} / Dekade")
    ```

---

## Bonus-Aufgaben

??? tip "Für Fortgeschrittene"
    **A) Hai-Arten analysieren:**
    - Extrahiere Hai-Arten aus der Species-Spalte
    - Welche Art ist am gefährlichsten?
    
    **B) Text Mining:**
    - Analysiere die Injury-Beschreibungen
    - Welche Körperteile werden am häufigsten verletzt?
    
    **C) Geographische Analyse:**
    - Gruppiere nach Regionen (Area/Location)
    - Gibt es Hotspots innerhalb der Top-Länder?
    
    **D) Vorhersage-Modell:**
    - Können wir basierend auf Aktivität, Ort, Alter vorhersagen ob ein Angriff tödlich ist?

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **Reale Daten** sind messy – Bereinigung ist essentiell
    - **Encoding-Probleme** mit `encoding='latin-1'` lösen
    - **Typenkonvertierung** mit `pd.to_numeric(errors='coerce')`
    - **Aggregation** mit `groupby` und `pivot_table`
    - **Explorative Analyse** systematisch durchführen
    - **Erkenntnisse** zusammenfassen und interpretieren

---

??? question "Selbstkontrolle"
    1. Warum verwendet man `errors='coerce'` bei der Typkonvertierung?
    2. Wie berechnet man die Tödlichkeitsrate pro Gruppe?
    3. Was bedeutet `dropna=False` bei `value_counts()`?
    4. Wann ist ein Datensatz "sauber genug" für die Analyse?
    
    ??? success "Antworten"
        1. Ungültige Werte werden zu NaN statt einen Fehler zu werfen – wichtig bei unstrukturierten Daten
        2. `.groupby('Gruppe')['Fatal'].mean() * 100` – mean() auf True/False gibt den Anteil True
        3. NaN-Werte werden auch gezählt statt ignoriert – wichtig um fehlende Werte zu sehen
        4. Wenn die verbleibenden Probleme die Analyse nicht verfälschen und die Kernfragen beantwortet werden können – Perfekte Daten gibt es selten!
