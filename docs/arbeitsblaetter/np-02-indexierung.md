# NumPy – Indexierung & Slicing

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- Daten aus spezifischen Positionen eines Arrays extrahieren
- 1D- und 2D-Slicing sicher anwenden
- gezielt Teilbereiche großer Datensätze auswählen
- den Unterschied zwischen Views und Copies verstehen

!!! note "Begleitendes Infoblatt"
    [:material-book-open-variant: NumPy Indexierung](../infoblaetter/numpy-indexierung.md) – Slicing, Fancy Indexing, Boolean Indexing

---

## Einführung

In dieser Lernsituation arbeitest du mit echten NYC Yellow Taxi Trip-Daten. Du lernst, wie du gezielt auf Teile großer Datensätze zugreifst.

**Datenfelder im Datensatz:**

| Spalte | Index | Beschreibung |
|--------|-------|--------------|
| VendorID | 0 | Anbieter-ID |
| lpep_pickup_datetime | 1 | Startzeit |
| lpep_dropoff_datetime | 2 | Endzeit |
| passenger_count | 7 | Anzahl Passagiere |
| trip_distance | 8 | Strecke (Meilen) |
| fare_amount | 9 | Fahrpreis ($) |
| tip_amount | 12 | Trinkgeld ($) |
| total_amount | 16 | Gesamtbetrag ($) |

---

## Aufgaben

### Aufgabe 1 – Daten laden

Lade die Taxi-Daten als NumPy-Array.

- [ ] **Lade die CSV-Datei:**
    ```python
    import numpy as np
    
    # Daten laden (numerische Spalten)
    daten = np.genfromtxt('../assets/files/taxi_tripdata.csv', 
                          delimiter=',', 
                          skip_header=1)
    
    print(f"Shape: {daten.shape}")
    print(f"Erste Zeile: {daten[0]}")
    ```

- [ ] **Untersuche das Array:**
    ```python
    print(f"Dimensionen: {daten.ndim}")
    print(f"Datentyp: {daten.dtype}")
    print(f"Anzahl Fahrten: {daten.shape[0]}")
    print(f"Anzahl Spalten: {daten.shape[1]}")
    ```

- [ ] **Prüfe auf NaN-Werte:**
    ```python
    nan_count = np.isnan(daten).sum()
    print(f"Anzahl NaN-Werte: {nan_count}")
    ```

!!! question "Reflexionsfrage"
    Warum enthält das Array NaN-Werte? (Tipp: Was passiert mit Text-Spalten?)

---

### Aufgabe 2 – 1D-Indexierung und Slicing

Arbeite mit einzelnen Spalten.

- [ ] **Extrahiere die Spalte `trip_distance` (Index 8):**
    ```python
    trip_distance = daten[:, 8]
    print(f"Shape: {trip_distance.shape}")
    print(f"Erste 10 Werte: {trip_distance[:10]}")
    ```

- [ ] **Zeige die ersten 30 Einträge:**
    ```python
    print("Erste 30 Fahrstrecken:")
    print(trip_distance[:30])
    ```

- [ ] **Zeige die letzten 10 Einträge:**
    ```python
    print("Letzte 10 Fahrstrecken:")
    print(trip_distance[-10:])
    ```

- [ ] **Jeder fünfte Wert:**
    ```python
    print("Jeder 5. Wert (erste 20):")
    print(trip_distance[::5][:20])
    ```

---

### Aufgabe 3 – Mehrere Spalten extrahieren

Extrahiere mehrere Spalten gleichzeitig.

- [ ] **Extrahiere `fare_amount` (9) und `tip_amount` (12):**
    ```python
    # Beide Spalten
    preise = daten[:, [9, 12]]
    print(f"Shape: {preise.shape}")
    print(f"Erste 10 Zeilen:\n{preise[:10]}")
    ```

- [ ] **Erstelle eine Übersicht der ersten 30 Fahrten:**
    ```python
    # Spalten: passenger_count(7), trip_distance(8), fare_amount(9), total_amount(16)
    uebersicht = daten[:30, [7, 8, 9, 16]]
    print("Passagiere | Strecke | Fahrpreis | Gesamt")
    print("-" * 45)
    for zeile in uebersicht:
        print(f"{zeile[0]:10.0f} | {zeile[1]:7.2f} | {zeile[2]:9.2f} | {zeile[3]:6.2f}")
    ```

---

### Aufgabe 4 – 2D-Slicing

Wähle gezielte Bereiche aus der Matrix.

```kroki-plantuml
@startuml
!theme plain
skinparam backgroundColor transparent

rectangle "daten[zeilen, spalten]" as title #white

rectangle "Alle Zeilen\ndaten[:, 5]" as r1 #lightblue
rectangle "Zeilen 10-20\ndaten[10:20, :]" as r2 #lightgreen
rectangle "Bereich\ndaten[10:20, 5:10]" as r3 #lightyellow

note bottom
  [start:stop] - stop ist exklusiv
  [:] - alle Elemente
  [::2] - jedes zweite Element
end note
@enduml
```

- [ ] **Wähle Zeilen 1011 bis 1097 und Spalten 6 bis 9:**
    ```python
    ausschnitt = daten[1011:1098, 6:10]
    print(f"Shape: {ausschnitt.shape}")  # Sollte (87, 4) sein
    print(f"Erste 5 Zeilen:\n{ausschnitt[:5]}")
    ```

- [ ] **Extrahiere jede zweite Zeile:**
    ```python
    jede_zweite = daten[::2]
    print(f"Original: {daten.shape[0]} Zeilen")
    print(f"Jede zweite: {jede_zweite.shape[0]} Zeilen")
    ```

- [ ] **Extrahiere die letzte Spalte:**
    ```python
    letzte_spalte = daten[:, -1]
    print(f"Letzte Spalte (erste 10): {letzte_spalte[:10]}")
    ```

- [ ] **Umgekehrte Reihenfolge:**
    ```python
    # Letzte 10 Zeilen in umgekehrter Reihenfolge
    umgekehrt = daten[-1:-11:-1]
    print(f"Shape: {umgekehrt.shape}")
    ```

---

### Aufgabe 5 – Slicing-Visualisierung

Verstehe das Slicing besser durch Visualisierung.

- [ ] **Erstelle eine kleine Test-Matrix:**
    ```python
    test = np.arange(1, 21).reshape(4, 5)
    print("Test-Matrix:")
    print(test)
    ```

- [ ] **Übe verschiedene Slicing-Operationen:**
    ```python
    print(f"Zeile 1: {test[1]}")
    print(f"Spalte 2: {test[:, 2]}")
    print(f"Zeilen 1-2, Spalten 2-4:\n{test[1:3, 2:5]}")
    print(f"Jede 2. Zeile, jede 2. Spalte:\n{test[::2, ::2]}")
    ```

---

### Aufgabe 6 – Views vs. Copies

Ein wichtiges Konzept: Slicing erstellt Views, keine Kopien!

- [ ] **Demonstriere das View-Verhalten:**
    ```python
    original = np.array([1, 2, 3, 4, 5])
    
    # Slicing erstellt View
    view = original[1:4]
    print(f"Original: {original}")
    print(f"View: {view}")
    
    # Ändere den View
    view[0] = 99
    
    print(f"\nNach Änderung:")
    print(f"Original: {original}")  # Auch geändert!
    print(f"View: {view}")
    ```

- [ ] **Erstelle eine echte Kopie:**
    ```python
    original = np.array([1, 2, 3, 4, 5])
    
    # Explizite Kopie
    kopie = original[1:4].copy()
    kopie[0] = 99
    
    print(f"Original: {original}")  # Unverändert!
    print(f"Kopie: {kopie}")
    ```

!!! warning "Wichtig"
    Bei der Arbeit mit Daten solltest du dir immer bewusst sein, ob du mit einem View oder einer Kopie arbeitest!

---

### Aufgabe 7 – Praktische Anwendung

Wende dein Wissen auf die Taxi-Daten an.

- [ ] **Analysiere die ersten 100 Fahrten:**
    ```python
    erste_100 = daten[:100]
    
    # Durchschnittliche Fahrstrecke
    strecke = erste_100[:, 8]
    print(f"Durchschnittliche Strecke: {np.nanmean(strecke):.2f} Meilen")
    
    # Durchschnittlicher Fahrpreis
    preis = erste_100[:, 9]
    print(f"Durchschnittlicher Preis: ${np.nanmean(preis):.2f}")
    ```

- [ ] **Vergleiche erste und letzte 500 Fahrten:**
    ```python
    erste_500 = daten[:500]
    letzte_500 = daten[-500:]
    
    print("Durchschnittliche Strecke:")
    print(f"  Erste 500: {np.nanmean(erste_500[:, 8]):.2f} Meilen")
    print(f"  Letzte 500: {np.nanmean(letzte_500[:, 8]):.2f} Meilen")
    
    print("\nDurchschnittlicher Gesamtbetrag:")
    print(f"  Erste 500: ${np.nanmean(erste_500[:, 16]):.2f}")
    print(f"  Letzte 500: ${np.nanmean(letzte_500[:, 16]):.2f}")
    ```

- [ ] **Erstelle eine Stichprobe:**
    ```python
    # Jede 10. Fahrt als Stichprobe
    stichprobe = daten[::10]
    print(f"Stichprobengröße: {stichprobe.shape[0]} Fahrten")
    print(f"Durchschnittliche Strecke: {np.nanmean(stichprobe[:, 8]):.2f} Meilen")
    ```

---

### Aufgabe 8 – Bonus: Transponieren und Umformen

- [ ] **Transponiere einen Ausschnitt:**
    ```python
    # 5 Fahrten, 4 Merkmale
    ausschnitt = daten[:5, [7, 8, 9, 16]]
    print(f"Original Shape: {ausschnitt.shape}")
    
    transponiert = ausschnitt.T
    print(f"Transponiert Shape: {transponiert.shape}")
    print(f"Transponiert:\n{transponiert}")
    ```

- [ ] **Flache Darstellung:**
    ```python
    flach = ausschnitt.flatten()
    print(f"Flatten Shape: {flach.shape}")
    print(f"Flatten: {flach}")
    ```

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **1D-Slicing**: `arr[start:stop:step]`
    - **2D-Slicing**: `arr[zeilen, spalten]`
    - **Negative Indizes**: `-1` für letztes Element
    - **Mehrere Spalten**: `arr[:, [0, 2, 4]]`
    - **Views vs. Copies**: Slicing erstellt Views, `.copy()` für echte Kopie
    - **Transponieren**: `.T` oder `np.transpose()`

---

??? question "Selbstkontrolle"
    1. Was gibt `daten[5:10, 2:5]` zurück?
    2. Wie extrahierst du die letzte Zeile einer Matrix?
    3. Was ist der Unterschied zwischen `arr[1:4]` und `arr[1:4].copy()`?
    4. Wie würdest du jede dritte Zeile und jede zweite Spalte auswählen?
    
    ??? success "Antworten"
        1. Zeilen 5-9 (5 Zeilen), Spalten 2-4 (3 Spalten) → 5×3 Matrix
        2. `matrix[-1]` oder `matrix[-1, :]`
        3. Das erste ist ein View (Änderungen wirken auf Original), das zweite eine unabhängige Kopie
        4. `arr[::3, ::2]`
