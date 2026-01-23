# NumPy – Einführung

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- erklären, warum NumPy für Datenanalyse wichtig ist
- NumPy-Arrays erstellen und ihre Eigenschaften inspizieren
- den Unterschied zwischen Python-Listen und NumPy-Arrays verstehen
- grundlegende Operationen auf Arrays durchführen

!!! note "Begleitendes Infoblatt"
    [:material-book-open-variant: NumPy Grundlagen](../infoblaetter/numpy-grundlagen.md) – Installation, Arrays, Datentypen, Dimensionen

---

## Einführung

Als Data Analyst arbeitest du ständig mit großen Datenmengen. NumPy ist die Grundlage für effiziente numerische Berechnungen in Python.

```kroki-plantuml
@startuml
!theme plain
skinparam backgroundColor transparent

package "Warum NumPy?" {
    [Schnelle Berechnungen] as speed #lightgreen
    [Speichereffizient] as mem #lightblue
    [Vektorisierte Operationen] as vec #lightyellow
    [Basis für Pandas, Scikit-learn] as basis #lightpink
}

note bottom
  NumPy-Arrays sind bis zu
  100x schneller als Python-Listen!
end note
@enduml
```

**Bearbeite alle Aufgaben in einem Jupyter Notebook oder einer Python-Datei.**

---

## Aufgaben

### Aufgabe 1 – NumPy installieren und importieren

Bevor du mit NumPy arbeiten kannst, muss die Bibliothek installiert und importiert werden.

- [ ] **Installiere NumPy** (falls noch nicht geschehen):
    ```bash
    pip install numpy
    ```

- [ ] **Importiere NumPy** mit der üblichen Konvention:
    ```python
    import numpy as np
    ```

- [ ] **Prüfe die installierte Version:**
    ```python
    print(np.__version__)
    ```

---

### Aufgabe 2 – Python-Listen vs. NumPy-Arrays

Vergleiche die Performance von Python-Listen und NumPy-Arrays.

- [ ] **Erstelle `performance_vergleich.py`:**
    
    ```python
    import numpy as np
    import time
    
    # 1 Million Werte
    n = 1_000_000
    
    # Python-Liste
    python_liste = list(range(n))
    
    # NumPy-Array
    numpy_array = np.arange(n)
    ```

- [ ] **Messe die Zeit für das Verdoppeln aller Werte:**
    
    ```python
    # Python-Liste (mit List Comprehension)
    start = time.time()
    ergebnis_liste = [x * 2 for x in python_liste]
    zeit_liste = time.time() - start
    print(f"Python-Liste: {zeit_liste:.4f} Sekunden")
    
    # NumPy-Array (vektorisiert)
    start = time.time()
    ergebnis_numpy = numpy_array * 2
    zeit_numpy = time.time() - start
    print(f"NumPy-Array: {zeit_numpy:.4f} Sekunden")
    
    # Speedup berechnen
    print(f"NumPy ist {zeit_liste / zeit_numpy:.1f}x schneller!")
    ```

- [ ] **Dokumentiere deine Ergebnisse:** Wie viel schneller ist NumPy?

!!! question "Reflexionsfrage"
    Warum ist NumPy so viel schneller als Python-Listen?

---

### Aufgabe 3 – Arrays erstellen

Lerne verschiedene Methoden, um NumPy-Arrays zu erstellen.

- [ ] **Aus einer Python-Liste:**
    ```python
    temperaturen = np.array([22.5, 24.1, 19.8, 23.2, 25.0])
    print(temperaturen)
    print(type(temperaturen))
    ```

- [ ] **Mit Initialisierungsfunktionen:**
    ```python
    # Erstelle folgende Arrays und gib sie aus:
    nullen = np.zeros((3, 4))       # 3x4 Matrix mit Nullen
    einsen = np.ones((2, 5))        # 2x5 Matrix mit Einsen
    leer = np.empty((2, 2))         # 2x2 leeres Array
    fuenfer = np.full((3, 3), 5)    # 3x3 Matrix gefüllt mit 5
    identitaet = np.eye(4)          # 4x4 Einheitsmatrix
    ```

- [ ] **Sequenzen erstellen:**
    ```python
    # Mit arange (wie range, aber für Arrays)
    seq1 = np.arange(0, 10, 2)      # Start, Stop, Step
    print(f"arange: {seq1}")
    
    # Mit linspace (gleichmäßig verteilt)
    seq2 = np.linspace(0, 1, 5)     # Start, Stop, Anzahl
    print(f"linspace: {seq2}")
    ```

- [ ] **Zufallszahlen:**
    ```python
    # Gleichverteilte Zufallszahlen (0-1)
    zufaellig = np.random.rand(3, 4)
    
    # Ganzzahlige Zufallszahlen
    wuerfel = np.random.randint(1, 7, size=(10,))  # 10 Würfelwürfe
    
    print(f"Zufallsmatrix:\n{zufaellig}")
    print(f"Würfelwürfe: {wuerfel}")
    ```

---

### Aufgabe 4 – Array-Eigenschaften inspizieren

Untersuche die Eigenschaften von Arrays.

- [ ] **Erstelle ein 2D-Array und inspiziere es:**
    ```python
    matrix = np.array([[1, 2, 3, 4],
                       [5, 6, 7, 8],
                       [9, 10, 11, 12]])
    
    print(f"Form (shape): {matrix.shape}")
    print(f"Dimensionen (ndim): {matrix.ndim}")
    print(f"Datentyp (dtype): {matrix.dtype}")
    print(f"Anzahl Elemente (size): {matrix.size}")
    print(f"Bytes pro Element (itemsize): {matrix.itemsize}")
    print(f"Gesamtspeicher (nbytes): {matrix.nbytes} Bytes")
    ```

- [ ] **Erstelle Arrays mit verschiedenen Datentypen und vergleiche:**
    ```python
    arr_int = np.array([1, 2, 3], dtype=np.int32)
    arr_float = np.array([1, 2, 3], dtype=np.float64)
    arr_bool = np.array([True, False, True])
    
    print(f"int32: dtype={arr_int.dtype}, itemsize={arr_int.itemsize}")
    print(f"float64: dtype={arr_float.dtype}, itemsize={arr_float.itemsize}")
    print(f"bool: dtype={arr_bool.dtype}, itemsize={arr_bool.itemsize}")
    ```

---

### Aufgabe 5 – Reshaping (Form ändern)

Arrays können in verschiedene Formen umgewandelt werden.

- [ ] **Erstelle ein 1D-Array und forme es um:**
    ```python
    # 12 Elemente
    arr = np.arange(1, 13)
    print(f"Original: {arr}")
    print(f"Shape: {arr.shape}")
    
    # Zu verschiedenen 2D-Formen
    matrix_3x4 = arr.reshape(3, 4)
    matrix_4x3 = arr.reshape(4, 3)
    matrix_2x6 = arr.reshape(2, 6)
    
    print(f"\n3x4:\n{matrix_3x4}")
    print(f"\n4x3:\n{matrix_4x3}")
    ```

- [ ] **Nutze -1 für automatische Berechnung:**
    ```python
    # Eine Dimension automatisch berechnen
    auto = arr.reshape(3, -1)  # 3 Zeilen, Spalten automatisch
    print(f"Auto-Shape: {auto.shape}")
    ```

- [ ] **Zurück zu 1D:**
    ```python
    flach1 = matrix_3x4.flatten()   # Erstellt Kopie
    flach2 = matrix_3x4.ravel()     # Erstellt View
    print(f"Flatten: {flach1}")
    ```

!!! tip "Reshape-Regel"
    Die Gesamtzahl der Elemente muss gleich bleiben! 
    12 Elemente können zu (3,4), (4,3), (2,6), (6,2), (1,12), (12,1) umgeformt werden.

---

### Aufgabe 6 – Grundlegende Operationen

NumPy-Operationen sind element-weise.

- [ ] **Arithmetische Operationen:**
    ```python
    a = np.array([10, 20, 30, 40])
    b = np.array([1, 2, 3, 4])
    
    print(f"Addition: {a + b}")
    print(f"Subtraktion: {a - b}")
    print(f"Multiplikation: {a * b}")
    print(f"Division: {a / b}")
    print(f"Potenz: {b ** 2}")
    ```

- [ ] **Operationen mit Skalaren:**
    ```python
    preise = np.array([100, 200, 150, 300])
    
    # 10% Rabatt
    rabatt_preise = preise * 0.9
    print(f"Mit 10% Rabatt: {rabatt_preise}")
    
    # 19% MwSt hinzufügen
    brutto = preise * 1.19
    print(f"Mit MwSt: {brutto}")
    ```

- [ ] **Mathematische Funktionen:**
    ```python
    werte = np.array([1, 4, 9, 16, 25])
    
    print(f"Quadratwurzel: {np.sqrt(werte)}")
    print(f"Quadrat: {np.square(werte)}")
    print(f"Exponential: {np.exp([1, 2, 3])}")
    print(f"Logarithmus: {np.log([1, 10, 100])}")
    ```

---

### Aufgabe 7 – Praxisbeispiel: Temperatursensor

Ein Temperatursensor liefert Messwerte in Fahrenheit. Konvertiere sie zu Celsius.

- [ ] **Erstelle `temperatur_konverter.py`:**
    ```python
    import numpy as np
    
    # Messwerte in Fahrenheit (simuliert)
    np.random.seed(42)  # Reproduzierbare Zufallszahlen
    fahrenheit = np.random.uniform(60, 100, size=24)  # 24 Stunden
    
    print(f"Fahrenheit (erste 5): {fahrenheit[:5].round(1)}")
    ```

- [ ] **Konvertiere zu Celsius:**
    Formel: $C = (F - 32) \times \frac{5}{9}$
    
    ```python
    celsius = (fahrenheit - 32) * 5/9
    print(f"Celsius (erste 5): {celsius[:5].round(1)}")
    ```

- [ ] **Berechne Statistiken:**
    ```python
    print(f"\n=== Tagesstatistik (Celsius) ===")
    print(f"Minimum: {celsius.min():.1f}°C")
    print(f"Maximum: {celsius.max():.1f}°C")
    print(f"Durchschnitt: {celsius.mean():.1f}°C")
    print(f"Standardabweichung: {celsius.std():.2f}°C")
    ```

- [ ] **Finde die Stunde mit der höchsten Temperatur:**
    ```python
    stunde_max = np.argmax(celsius)
    print(f"Höchste Temperatur um {stunde_max}:00 Uhr ({celsius[stunde_max]:.1f}°C)")
    ```

---

### Aufgabe 8 – Bonus: Mehrdimensionale Arrays

Arbeite mit 3D-Arrays für komplexere Datenstrukturen.

- [ ] **Erstelle ein 3D-Array (z.B. RGB-Bild):**
    ```python
    # Kleines 4x4 "Bild" mit 3 Farbkanälen (R, G, B)
    bild = np.random.randint(0, 256, size=(4, 4, 3), dtype=np.uint8)
    
    print(f"Bild-Shape: {bild.shape}")
    print(f"Dimensionen: {bild.ndim}")
    print(f"Pixel oben-links: {bild[0, 0]}")  # RGB-Werte
    ```

- [ ] **Greife auf einzelne Farbkanäle zu:**
    ```python
    rot_kanal = bild[:, :, 0]    # Nur Rot
    gruen_kanal = bild[:, :, 1]  # Nur Grün
    blau_kanal = bild[:, :, 2]   # Nur Blau
    
    print(f"Rot-Kanal:\n{rot_kanal}")
    ```

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **NumPy importieren**: `import numpy as np`
    - **Arrays erstellen**: `np.array()`, `np.zeros()`, `np.ones()`, `np.arange()`, `np.linspace()`
    - **Eigenschaften**: `shape`, `dtype`, `ndim`, `size`
    - **Reshaping**: `reshape()`, `flatten()`, `ravel()`
    - **Operationen**: Element-weise (+, -, *, /), mathematische Funktionen
    - **Performance**: NumPy ist viel schneller als Python-Listen!

---

??? question "Selbstkontrolle"
    1. Wie erstellst du ein 5x5 Array mit Nullen?
    2. Was ist der Unterschied zwischen `np.arange(0, 10, 2)` und `np.linspace(0, 10, 5)`?
    3. Kann ein Array mit 15 Elementen zu (3, 4) umgeformt werden?
    4. Was gibt `np.array([1, 2, 3]) * 2` zurück?
    
    ??? success "Antworten"
        1. `np.zeros((5, 5))`
        2. `arange` erzeugt `[0, 2, 4, 6, 8]` (Schrittweite 2), `linspace` erzeugt 5 gleichmäßig verteilte Werte von 0 bis 10
        3. Nein, 15 ≠ 3×4=12. Mögliche Formen: (3,5), (5,3), (1,15), (15,1)
        4. `array([2, 4, 6])` – element-weise Multiplikation
