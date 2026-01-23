# NumPy – Filtern & Vektorisierung

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- Boolean Indexing für komplexe Filter anwenden
- mehrere Bedingungen mit logischen Operatoren kombinieren
- vektorisierte Berechnungen statt Schleifen nutzen
- effiziente Datenmanipulationen durchführen

!!! note "Begleitende Infoblätter"
    - [:material-book-open-variant: NumPy Indexierung](../infoblaetter/numpy-indexierung.md) – Boolean Indexing
    - [:material-book-open-variant: NumPy Broadcasting](../infoblaetter/numpy-broadcasting.md) – Vektorisierung

---

## Einführung

Boolean Indexing ist eine der mächtigsten Techniken in NumPy. Statt Schleifen nutzt du Bedingungen direkt auf Arrays.

```kroki-plantuml
@startuml
!theme plain
skinparam backgroundColor transparent

rectangle "Traditionelle Schleife" as loop #lightcoral {
    rectangle "for x in daten:\n    if x > 5:\n        ergebnis.append(x)" as l1
}

rectangle "Boolean Indexing" as bool #lightgreen {
    rectangle "ergebnis = daten[daten > 5]" as b1
}

loop --> bool : "Viel kürzer\nund schneller!"
@enduml
```

---

## Aufgaben

### Aufgabe 1 – Daten vorbereiten

- [ ] **Lade die Taxi-Daten:**
    ```python
    import numpy as np
    
    daten = np.genfromtxt('../assets/files/taxi_tripdata.csv',
                          delimiter=',',
                          skip_header=1)
    
    # Relevante Spalten extrahieren
    passagiere = daten[:, 7]
    strecke = daten[:, 8]
    fahrpreis = daten[:, 9]
    trinkgeld = daten[:, 12]
    gesamt = daten[:, 16]
    
    print(f"Anzahl Fahrten: {len(strecke)}")
    ```

---

### Aufgabe 2 – Grundlagen Boolean Indexing

Verstehe, wie Boolean Indexing funktioniert.

![Filtering](../assets/images/numpy/filtering.png)

- [ ] **Schritt 1: Bedingung erstellt Boolean-Array:**
    ```python
    # Fahrten mit mehr als 5 Meilen
    bedingung = strecke > 5
    
    print(f"Bedingung (erste 10): {bedingung[:10]}")
    print(f"Datentyp: {bedingung.dtype}")
    print(f"Anzahl True: {bedingung.sum()}")
    ```

- [ ] **Schritt 2: Boolean-Array als Index verwenden:**
    ```python
    # Filtere mit der Bedingung
    lange_fahrten = strecke[bedingung]
    
    print(f"\nAnzahl Fahrten > 5 Meilen: {len(lange_fahrten)}")
    print(f"Kürzeste 'lange' Fahrt: {lange_fahrten.min():.2f} Meilen")
    print(f"Durchschnitt: {np.nanmean(lange_fahrten):.2f} Meilen")
    ```

- [ ] **Kombiniert in einer Zeile:**
    ```python
    # Üblicher Stil: Bedingung direkt in Klammern
    kurze_fahrten = strecke[strecke < 1]
    print(f"Fahrten unter 1 Meile: {len(kurze_fahrten)}")
    ```

---

### Aufgabe 3 – Verschiedene Vergleichsoperatoren

- [ ] **Teste alle Vergleichsoperatoren:**
    ```python
    print("Vergleichsoperatoren auf Strecke:")
    print(f"  > 10 Meilen:  {(strecke > 10).sum()} Fahrten")
    print(f"  >= 10 Meilen: {(strecke >= 10).sum()} Fahrten")
    print(f"  < 1 Meile:    {(strecke < 1).sum()} Fahrten")
    print(f"  <= 1 Meile:   {(strecke <= 1).sum()} Fahrten")
    print(f"  == 0 Meilen:  {(strecke == 0).sum()} Fahrten")
    print(f"  != 0 Meilen:  {(strecke != 0).sum()} Fahrten")
    ```

- [ ] **Finde Fahrten mit genau 1 Passagier:**
    ```python
    einzelfahrten = passagiere[passagiere == 1]
    print(f"\nFahrten mit 1 Passagier: {len(einzelfahrten)}")
    print(f"Anteil: {len(einzelfahrten) / len(passagiere) * 100:.1f}%")
    ```

---

### Aufgabe 4 – Mehrere Bedingungen kombinieren

!!! warning "Wichtig: Operatoren und Klammern"
    - Verwende `&` (und), `|` (oder), `~` (nicht)
    - **Nicht** `and`, `or`, `not`
    - Jede Bedingung muss in Klammern stehen!

- [ ] **UND-Verknüpfung (&):**
    ```python
    # Fahrten mit mehr als 2 Passagieren UND Strecke unter 2 Meilen
    bed1 = passagiere > 2
    bed2 = strecke < 2
    
    kombiniert = daten[(bed1) & (bed2)]
    print(f"Fahrten mit >2 Passagiere UND <2 Meilen: {len(kombiniert)}")
    ```

- [ ] **ODER-Verknüpfung (|):**
    ```python
    # Sehr kurze ODER sehr lange Fahrten
    extrem = strecke[(strecke < 0.5) | (strecke > 20)]
    print(f"Extreme Fahrten: {len(extrem)}")
    ```

- [ ] **Bereichsfilter (zwischen zwei Werten):**
    ```python
    # Fahrpreise zwischen 10 und 20 Dollar
    mittel_preis = fahrpreis[(fahrpreis >= 10) & (fahrpreis <= 20)]
    print(f"Fahrten mit Preis $10-$20: {len(mittel_preis)}")
    print(f"Durchschnitt: ${np.nanmean(mittel_preis):.2f}")
    ```

- [ ] **Negation (~):**
    ```python
    # Alle Fahrten AUSSER 0 Passagiere
    mit_passagieren = passagiere[~(passagiere == 0)]
    print(f"Fahrten mit mindestens 1 Passagier: {len(mit_passagieren)}")
    ```

---

### Aufgabe 5 – Komplexe Filter auf Datensätze

Filtere den gesamten Datensatz (alle Spalten).

- [ ] **Filtere Zeilen basierend auf einer Bedingung:**
    ```python
    # Alle Spalten für Fahrten > 5 Meilen
    maske = strecke > 5
    lange_fahrten_komplett = daten[maske]
    
    print(f"Original Shape: {daten.shape}")
    print(f"Gefiltert Shape: {lange_fahrten_komplett.shape}")
    ```

- [ ] **Analysiere gefilterte Daten:**
    ```python
    # Durchschnittlicher Preis für lange Fahrten
    print(f"\nLange Fahrten (>5 Meilen):")
    print(f"  Durchschnittspreis: ${np.nanmean(lange_fahrten_komplett[:, 9]):.2f}")
    print(f"  Durchschnitt Trinkgeld: ${np.nanmean(lange_fahrten_komplett[:, 12]):.2f}")
    
    # Vergleich mit allen Fahrten
    print(f"\nAlle Fahrten:")
    print(f"  Durchschnittspreis: ${np.nanmean(fahrpreis):.2f}")
    print(f"  Durchschnitt Trinkgeld: ${np.nanmean(trinkgeld):.2f}")
    ```

- [ ] **Zähle Fahrten mit genau 0 Passagieren:**
    ```python
    null_passagiere = daten[passagiere == 0]
    print(f"\nFahrten mit 0 Passagieren: {len(null_passagiere)}")
    # Das könnten Datenfehler sein!
    ```

---

### Aufgabe 6 – Vektorisierte Berechnungen

Führe Berechnungen auf ganzen Arrays durch – ohne Schleifen!

![Vektorisierung](../assets/images/numpy/vec1.png)

- [ ] **Berechne Preis pro Meile:**
    ```python
    # Vektorisiert: Alle Werte auf einmal
    preis_pro_meile = fahrpreis / strecke
    
    # NaN entfernen (Division durch 0)
    preis_pro_meile_sauber = preis_pro_meile[~np.isnan(preis_pro_meile) & 
                                              ~np.isinf(preis_pro_meile)]
    
    print(f"Durchschnittlicher Preis pro Meile: ${np.mean(preis_pro_meile_sauber):.2f}")
    print(f"Median Preis pro Meile: ${np.median(preis_pro_meile_sauber):.2f}")
    ```

- [ ] **Preiskorrektur: Erhöhe alle Fahrpreise um 10%:**
    ```python
    # Kopie erstellen (Original nicht verändern!)
    fahrpreis_neu = fahrpreis.copy()
    
    # Vektorisierte Erhöhung
    fahrpreis_neu = fahrpreis_neu * 1.10
    
    print(f"Alter Durchschnitt: ${np.nanmean(fahrpreis):.2f}")
    print(f"Neuer Durchschnitt: ${np.nanmean(fahrpreis_neu):.2f}")
    ```

- [ ] **Trinkgeld-Anteil berechnen:**
    ```python
    # Trinkgeld als Prozent vom Fahrpreis
    trinkgeld_prozent = (trinkgeld / fahrpreis) * 100
    
    # Nur gültige Werte
    gueltig = ~np.isnan(trinkgeld_prozent) & ~np.isinf(trinkgeld_prozent)
    trinkgeld_prozent_sauber = trinkgeld_prozent[gueltig]
    
    print(f"Durchschnittliches Trinkgeld: {np.mean(trinkgeld_prozent_sauber):.1f}%")
    print(f"Median Trinkgeld: {np.median(trinkgeld_prozent_sauber):.1f}%")
    ```

---

### Aufgabe 7 – np.where() für bedingte Berechnungen

`np.where(bedingung, wenn_true, wenn_false)` ermöglicht bedingte Wertzuweisungen.

![Vektorisierung](../assets/images/numpy/vec2.png)

- [ ] **Kategorisiere Fahrtstrecken:**
    ```python
    # Kurz, Mittel, Lang
    kategorie = np.where(strecke < 2, 'kurz',
                 np.where(strecke < 10, 'mittel', 'lang'))
    
    print("Strecken-Kategorien:")
    print(f"  Kurz (<2 Meilen): {(kategorie == 'kurz').sum()}")
    print(f"  Mittel (2-10 Meilen): {(kategorie == 'mittel').sum()}")
    print(f"  Lang (>10 Meilen): {(kategorie == 'lang').sum()}")
    ```

- [ ] **Bedingte Berechnung:**
    ```python
    # Rabatt: 10% wenn Preis > $30, sonst 5%
    rabatt = np.where(fahrpreis > 30, fahrpreis * 0.10, fahrpreis * 0.05)
    
    print(f"Durchschnittlicher Rabatt: ${np.nanmean(rabatt):.2f}")
    ```

- [ ] **Werte ersetzen:**
    ```python
    # Negative Preise durch 0 ersetzen (Datenfehler)
    preis_korrigiert = np.where(fahrpreis < 0, 0, fahrpreis)
    
    neg_count = (fahrpreis < 0).sum()
    print(f"Negative Preise gefunden und korrigiert: {neg_count}")
    ```

---

### Aufgabe 8 – Praktische Analysen

Wende alles Gelernte für echte Analysen an.

- [ ] **Analyse: Vergleich Zahlungsarten:**
    ```python
    # Zahlungsart ist Spalte 17 (1=Kreditkarte, 2=Bar, etc.)
    zahlungsart = daten[:, 17]
    
    # Kreditkarten-Zahlungen
    kreditkarte = zahlungsart == 1
    bar = zahlungsart == 2
    
    print("Zahlungsarten-Vergleich:")
    print(f"  Kreditkarte: {kreditkarte.sum()} Fahrten")
    print(f"    Durchschnitt Trinkgeld: ${np.nanmean(trinkgeld[kreditkarte]):.2f}")
    print(f"  Bar: {bar.sum()} Fahrten")
    print(f"    Durchschnitt Trinkgeld: ${np.nanmean(trinkgeld[bar]):.2f}")
    ```

- [ ] **Finde Anomalien:**
    ```python
    # Ausreißer: Mehr als 3 Standardabweichungen vom Mittelwert
    mean_preis = np.nanmean(fahrpreis)
    std_preis = np.nanstd(fahrpreis)
    
    ausreisser_hoch = fahrpreis > (mean_preis + 3 * std_preis)
    ausreisser_niedrig = fahrpreis < (mean_preis - 3 * std_preis)
    
    print(f"\nAusreißer bei Fahrpreis:")
    print(f"  Ungewöhnlich hoch: {ausreisser_hoch.sum()}")
    print(f"  Ungewöhnlich niedrig: {ausreisser_niedrig.sum()}")
    
    # Details der höchsten Ausreißer
    if ausreisser_hoch.any():
        print(f"\n  Höchste Preise: {fahrpreis[ausreisser_hoch][:5]}")
    ```

- [ ] **Effizienzanalyse:**
    ```python
    # Fahrten mit guter Effizienz: Viel Umsatz pro Meile
    effizienz = gesamt / strecke
    
    # Top 10% effizienteste Fahrten
    threshold = np.nanpercentile(effizienz[~np.isinf(effizienz)], 90)
    top_effizient = effizienz > threshold
    
    print(f"\nTop 10% effizienteste Fahrten:")
    print(f"  Anzahl: {top_effizient.sum()}")
    print(f"  Durchschnitt Strecke: {np.nanmean(strecke[top_effizient]):.2f} Meilen")
    print(f"  Durchschnitt Umsatz: ${np.nanmean(gesamt[top_effizient]):.2f}")
    ```

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **Boolean Indexing**: `arr[arr > 5]` filtert direkt
    - **Operatoren**: `&` (und), `|` (oder), `~` (nicht) + Klammern!
    - **Vektorisierung**: Operationen auf ganze Arrays statt Schleifen
    - **np.where()**: Bedingte Wertzuweisungen
    - **Komplexe Filter**: Kombiniere Bedingungen für mächtige Abfragen

---

??? question "Selbstkontrolle"
    1. Was ist falsch an `arr[arr > 5 and arr < 10]`?
    2. Wie filterst du alle Werte, die NICHT zwischen 5 und 10 liegen?
    3. Was macht `np.where(arr > 0, arr, 0)`?
    4. Warum ist `arr * 2` schneller als `[x * 2 for x in arr]`?
    
    ??? success "Antworten"
        1. Man muss `&` statt `and` verwenden und Klammern setzen: `arr[(arr > 5) & (arr < 10)]`
        2. `arr[(arr < 5) | (arr > 10)]` oder `arr[~((arr >= 5) & (arr <= 10))]`
        3. Ersetzt negative Werte durch 0, positive bleiben erhalten
        4. NumPy nutzt optimierte C-Routinen und verarbeitet alle Werte parallel
