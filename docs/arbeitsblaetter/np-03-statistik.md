# NumPy – Statistik & Aggregation

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- statistische Kennzahlen mit NumPy berechnen
- Aggregationsfunktionen auf Arrays anwenden
- den `axis`-Parameter für zeilen-/spaltenweise Berechnungen nutzen
- mit NaN-Werten in Statistiken umgehen

!!! note "Begleitendes Infoblatt"
    [:material-book-open-variant: NumPy Funktionen](../infoblaetter/numpy-funktionen.md) – Mathematische Operationen, Statistik, Achsen

---

## Einführung

Statistische Analysen sind das Kerngeschäft eines Data Analysts. NumPy bietet optimierte Funktionen für alle gängigen Statistiken.

```kroki-plantuml
@startuml
!theme plain
skinparam backgroundColor transparent

package "Statistische Funktionen" {
    [Lagemaße] as lag #lightblue
    [Streuungsmaße] as streu #lightgreen
    [Aggregation] as agg #lightyellow
}

note bottom of lag
  mean, median
  min, max
end note

note bottom of streu
  std, var
  Spannweite
end note

note bottom of agg
  sum, prod
  cumsum
end note
@enduml
```

---

## Aufgaben

### Aufgabe 1 – Daten vorbereiten

Lade die Taxi-Daten und bereite sie für die Analyse vor.

- [ ] **Lade die Daten:**
    ```python
    import numpy as np
    
    daten = np.genfromtxt('../assets/files/taxi_tripdata.csv',
                          delimiter=',',
                          skip_header=1)
    
    print(f"Datensatz: {daten.shape[0]} Fahrten, {daten.shape[1]} Merkmale")
    ```

- [ ] **Extrahiere relevante Spalten:**
    ```python
    # Spaltenindizes
    # 7: passenger_count, 8: trip_distance, 9: fare_amount
    # 12: tip_amount, 16: total_amount
    
    passagiere = daten[:, 7]
    strecke = daten[:, 8]
    fahrpreis = daten[:, 9]
    trinkgeld = daten[:, 12]
    gesamt = daten[:, 16]
    ```

---

### Aufgabe 2 – Lagemaße berechnen

Berechne Mittelwert, Median und Extremwerte.

- [ ] **Durchschnitt der Fahrstrecke:**
    ```python
    # Mit normaler Funktion (NaN führt zu NaN!)
    mean_normal = np.mean(strecke)
    print(f"Mean (normal): {mean_normal}")
    
    # NaN-sicher
    mean_safe = np.nanmean(strecke)
    print(f"Mean (nanmean): {mean_safe:.2f} Meilen")
    ```

- [ ] **Median des Fahrpreises:**
    ```python
    median_preis = np.nanmedian(fahrpreis)
    print(f"Median Fahrpreis: ${median_preis:.2f}")
    
    # Vergleiche mit Mittelwert
    mean_preis = np.nanmean(fahrpreis)
    print(f"Mittelwert Fahrpreis: ${mean_preis:.2f}")
    ```

    !!! question "Reflexionsfrage"
        Warum unterscheiden sich Median und Mittelwert? Was sagt das über die Verteilung aus?

- [ ] **Minimum und Maximum:**
    ```python
    print("\n=== Extremwerte ===")
    print(f"Kürzeste Strecke: {np.nanmin(strecke):.2f} Meilen")
    print(f"Längste Strecke: {np.nanmax(strecke):.2f} Meilen")
    print(f"Niedrigster Preis: ${np.nanmin(fahrpreis):.2f}")
    print(f"Höchster Preis: ${np.nanmax(fahrpreis):.2f}")
    ```

---

### Aufgabe 3 – Streuungsmaße

Berechne Standardabweichung und Varianz.

- [ ] **Standardabweichung der Fahrpreise:**
    ```python
    std_preis = np.nanstd(fahrpreis)
    var_preis = np.nanvar(fahrpreis)
    
    print(f"Standardabweichung: ${std_preis:.2f}")
    print(f"Varianz: ${var_preis:.2f}")
    ```

- [ ] **Interpretiere die Streuung:**
    ```python
    mean = np.nanmean(fahrpreis)
    std = np.nanstd(fahrpreis)
    
    print(f"\nFahrpreis-Statistik:")
    print(f"Mittelwert: ${mean:.2f}")
    print(f"68% der Preise liegen zwischen ${mean-std:.2f} und ${mean+std:.2f}")
    print(f"95% der Preise liegen zwischen ${mean-2*std:.2f} und ${mean+2*std:.2f}")
    ```

- [ ] **Variationskoeffizient:**
    ```python
    # Relative Streuung (CV = std / mean)
    cv_strecke = np.nanstd(strecke) / np.nanmean(strecke)
    cv_preis = np.nanstd(fahrpreis) / np.nanmean(fahrpreis)
    
    print(f"\nVariationskoeffizient:")
    print(f"Strecke: {cv_strecke:.2%}")
    print(f"Fahrpreis: {cv_preis:.2%}")
    ```

---

### Aufgabe 4 – Aggregationsfunktionen

Summen, Produkte und kumulative Werte.

![Statistik](../assets/images/numpy/stats.png)

- [ ] **Summiere die Gesamtbeträge:**
    ```python
    gesamt_umsatz = np.nansum(gesamt)
    print(f"Gesamtumsatz: ${gesamt_umsatz:,.2f}")
    
    gesamt_trinkgeld = np.nansum(trinkgeld)
    print(f"Gesamtes Trinkgeld: ${gesamt_trinkgeld:,.2f}")
    
    trinkgeld_anteil = gesamt_trinkgeld / gesamt_umsatz * 100
    print(f"Trinkgeld-Anteil: {trinkgeld_anteil:.1f}%")
    ```

- [ ] **Summe aller Spalten:**
    ```python
    # Summiere jede numerische Spalte
    spalten_summen = np.nansum(daten, axis=0)
    print(f"\nSumme pro Spalte (erste 10):")
    print(spalten_summen[:10])
    ```

- [ ] **Anzahl gültiger Werte:**
    ```python
    anzahl_gesamt = daten.shape[0]
    anzahl_gueltig_strecke = np.sum(~np.isnan(strecke))
    anzahl_gueltig_preis = np.sum(~np.isnan(fahrpreis))
    
    print(f"\nGültige Werte:")
    print(f"Strecke: {anzahl_gueltig_strecke} von {anzahl_gesamt}")
    print(f"Preis: {anzahl_gueltig_preis} von {anzahl_gesamt}")
    ```

---

### Aufgabe 5 – Der axis-Parameter

Verstehe die Unterscheidung zwischen zeilen- und spaltenweiser Aggregation.

```kroki-plantuml
@startuml
!theme plain
skinparam backgroundColor transparent

rectangle "axis-Parameter" as title #white

rectangle "axis=0\n↓ Spaltenweise\n(entlang der Zeilen)" as ax0 #lightblue
rectangle "axis=1\n→ Zeilenweise\n(entlang der Spalten)" as ax1 #lightgreen
rectangle "axis=None\nAlle Werte" as axn #lightyellow

title --> ax0 : "Ergebnis: 1 Wert pro Spalte"
title --> ax1 : "Ergebnis: 1 Wert pro Zeile"
title --> axn : "Ergebnis: 1 Wert gesamt"
@enduml
```

- [ ] **Erstelle eine Test-Matrix:**
    ```python
    # 4 Produkte (Zeilen), 3 Monate (Spalten)
    verkaeufe = np.array([
        [100, 120, 110],  # Produkt A
        [80, 90, 85],     # Produkt B
        [200, 180, 220],  # Produkt C
        [150, 160, 140]   # Produkt D
    ])
    
    print("Verkaufsdaten:")
    print(verkaeufe)
    ```

- [ ] **Berechne Statistiken mit verschiedenen Achsen:**
    ```python
    # Summe ohne axis = über alle Werte
    print(f"\nGesamtverkauf: {np.sum(verkaeufe)}")
    
    # axis=0: Summe pro Spalte (Monat)
    print(f"Pro Monat (axis=0): {np.sum(verkaeufe, axis=0)}")
    
    # axis=1: Summe pro Zeile (Produkt)
    print(f"Pro Produkt (axis=1): {np.sum(verkaeufe, axis=1)}")
    ```

- [ ] **Wende es auf die Taxi-Daten an:**
    ```python
    # Durchschnitt pro Spalte (alle Merkmale)
    spalten_mean = np.nanmean(daten, axis=0)
    print("\nDurchschnitt pro Merkmal:")
    merkmal_namen = ['Passagiere', 'Strecke', 'Fahrpreis', 'Trinkgeld', 'Gesamt']
    for name, wert, idx in zip(merkmal_namen, 
                                [spalten_mean[7], spalten_mean[8], 
                                 spalten_mean[9], spalten_mean[12], 
                                 spalten_mean[16]], 
                                [7, 8, 9, 12, 16]):
        print(f"  {name}: {wert:.2f}")
    ```

---

### Aufgabe 6 – Extremwerte finden

Finde die Position von Minimum und Maximum.

- [ ] **Index des Maximums:**
    ```python
    # Höchstes Trinkgeld
    idx_max_tip = np.nanargmax(trinkgeld)
    print(f"Höchstes Trinkgeld:")
    print(f"  Index: {idx_max_tip}")
    print(f"  Betrag: ${trinkgeld[idx_max_tip]:.2f}")
    print(f"  Gesamte Fahrt: ${gesamt[idx_max_tip]:.2f}")
    print(f"  Strecke: {strecke[idx_max_tip]:.2f} Meilen")
    ```

- [ ] **Längste und kürzeste Fahrt:**
    ```python
    idx_max_strecke = np.nanargmax(strecke)
    idx_min_strecke = np.nanargmin(strecke[strecke > 0])  # > 0 um 0-Fahrten auszuschließen
    
    print(f"\nLängste Fahrt:")
    print(f"  Strecke: {strecke[idx_max_strecke]:.2f} Meilen")
    print(f"  Preis: ${fahrpreis[idx_max_strecke]:.2f}")
    ```

- [ ] **Top 5 Trinkgelder:**
    ```python
    # Indizes der sortierten Werte
    sortiert_idx = np.argsort(trinkgeld)[::-1]  # Absteigend
    
    print("\nTop 5 Trinkgelder:")
    for i, idx in enumerate(sortiert_idx[:5]):
        if not np.isnan(trinkgeld[idx]):
            print(f"  {i+1}. ${trinkgeld[idx]:.2f} (Fahrt #{idx})")
    ```

---

### Aufgabe 7 – Perzentile und Quartile

Analysiere die Verteilung mit Perzentilen.

- [ ] **Quartile berechnen:**
    ```python
    q1 = np.nanpercentile(fahrpreis, 25)
    q2 = np.nanpercentile(fahrpreis, 50)  # = Median
    q3 = np.nanpercentile(fahrpreis, 75)
    iqr = q3 - q1  # Interquartilsabstand
    
    print("Fahrpreis-Quartile:")
    print(f"  Q1 (25%): ${q1:.2f}")
    print(f"  Q2 (50%): ${q2:.2f}")
    print(f"  Q3 (75%): ${q3:.2f}")
    print(f"  IQR: ${iqr:.2f}")
    ```

- [ ] **Mehrere Perzentile auf einmal:**
    ```python
    perzentile = [10, 25, 50, 75, 90, 95, 99]
    werte = np.nanpercentile(fahrpreis, perzentile)
    
    print("\nFahrpreis-Perzentile:")
    for p, w in zip(perzentile, werte):
        print(f"  {p:2d}. Perzentil: ${w:.2f}")
    ```

---

### Aufgabe 8 – Vollständige Statistik-Zusammenfassung

Erstelle eine umfassende Statistik-Funktion.

- [ ] **Erstelle eine Zusammenfassungs-Funktion:**
    ```python
    def statistik_zusammenfassung(arr, name="Daten"):
        """Erstellt eine vollständige Statistik-Zusammenfassung."""
        # NaN-Werte entfernen für Berechnung
        sauber = arr[~np.isnan(arr)]
        
        print(f"\n{'='*50}")
        print(f"Statistik für: {name}")
        print(f"{'='*50}")
        print(f"Anzahl Werte:     {len(sauber)}")
        print(f"Fehlende Werte:   {np.isnan(arr).sum()}")
        print(f"{'─'*50}")
        print(f"Minimum:          {np.min(sauber):.2f}")
        print(f"Maximum:          {np.max(sauber):.2f}")
        print(f"Spannweite:       {np.ptp(sauber):.2f}")
        print(f"{'─'*50}")
        print(f"Mittelwert:       {np.mean(sauber):.2f}")
        print(f"Median:           {np.median(sauber):.2f}")
        print(f"Standardabw.:     {np.std(sauber):.2f}")
        print(f"{'─'*50}")
        print(f"25. Perzentil:    {np.percentile(sauber, 25):.2f}")
        print(f"75. Perzentil:    {np.percentile(sauber, 75):.2f}")
        print(f"{'='*50}")
    ```

- [ ] **Wende die Funktion an:**
    ```python
    statistik_zusammenfassung(strecke, "Fahrstrecke (Meilen)")
    statistik_zusammenfassung(fahrpreis, "Fahrpreis ($)")
    statistik_zusammenfassung(trinkgeld, "Trinkgeld ($)")
    ```

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **Lagemaße**: `mean()`, `median()`, `min()`, `max()`
    - **Streuungsmaße**: `std()`, `var()`, `percentile()`
    - **Aggregation**: `sum()`, `cumsum()`, `argmin()`, `argmax()`
    - **axis-Parameter**: `axis=0` (spaltenweise), `axis=1` (zeilenweise)
    - **NaN-sichere Funktionen**: `nanmean()`, `nansum()`, etc.

---

??? question "Selbstkontrolle"
    1. Was ist der Unterschied zwischen `np.mean()` und `np.nanmean()`?
    2. Was gibt `np.argmax([3, 1, 4, 1, 5])` zurück?
    3. Bei einer 5×3 Matrix: Welche Shape hat `np.sum(matrix, axis=0)`?
    4. Wie berechnest du den Interquartilsabstand (IQR)?
    
    ??? success "Antworten"
        1. `mean()` gibt NaN zurück wenn NaN-Werte vorhanden, `nanmean()` ignoriert sie
        2. `4` (Index des Maximums 5)
        3. `(3,)` – ein Wert pro Spalte
        4. `IQR = np.percentile(arr, 75) - np.percentile(arr, 25)`
