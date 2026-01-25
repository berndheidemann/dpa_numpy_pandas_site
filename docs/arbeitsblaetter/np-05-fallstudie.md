# NumPy – Fallstudie Studentendaten

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- einen realen Datensatz selbstständig mit NumPy analysieren
- explorative Datenanalyse durchführen
- Zusammenhänge zwischen Variablen untersuchen
- Erkenntnisse aus Daten ableiten und interpretieren

!!! note "Begleitende Infoblätter"
    - [:material-book-open-variant: NumPy Grundlagen](../infoblaetter/numpy-grundlagen.md)
    - [:material-book-open-variant: NumPy Funktionen](../infoblaetter/numpy-funktionen.md)
    - [:material-book-open-variant: NumPy Indexierung](../infoblaetter/numpy-indexierung.md)

---

## Einführung

In dieser Fallstudie analysierst du einen echten Datensatz über Schülerleistungen und deren Zusammenhang mit Alkoholkonsum. Der Datensatz stammt aus einer portugiesischen Studie.

```kroki-plantuml
@startuml
!theme plain
skinparam backgroundColor transparent

rectangle "📊 Datensatz" as data {
    rectangle "student-mat.csv" as file
    rectangle "395 Schüler" as n
    rectangle "33 Merkmale" as feat
}

rectangle "🔍 Analyse-Workflow" as workflow {
    rectangle "1. Laden & Erkunden" as step1
    rectangle "2. Bereinigen" as step2
    rectangle "3. Statistiken" as step3
    rectangle "4. Zusammenhänge" as step4
    rectangle "5. Erkenntnisse" as step5
}

step1 --> step2
step2 --> step3
step3 --> step4
step4 --> step5

data --> step1
@enduml
```

!!! abstract "Datensatz herunterladen"
    [:material-download: student-mat.csv](../assets/files/student-mat.csv){ .md-button }

**Bearbeite alle Aufgaben in einem Jupyter Notebook.**

---

## Der Datensatz

Der Datensatz enthält Informationen über Schüler in portugiesischen Schulen.

### Wichtige Spalten

| Spaltenindex | Name | Beschreibung |
|--------------|------|--------------|
| 0 | school | Schule (GP oder MS) |
| 1 | sex | Geschlecht (F/M) |
| 2 | age | Alter (15-22) |
| 6 | Medu | Bildung der Mutter (0-4) |
| 7 | Fedu | Bildung des Vaters (0-4) |
| 13 | traveltime | Pendelzeit zur Schule (1-4) |
| 14 | studytime | Wöchentliche Lernzeit (1-4) |
| 24 | freetime | Freizeit nach der Schule (1-5) |
| 26 | Dalc | Alkohol an Werktagen (1-5) |
| 27 | Walc | Alkohol am Wochenende (1-5) |
| 29 | absences | Fehlstunden |
| 30 | G1 | Note 1. Periode (0-20) |
| 31 | G2 | Note 2. Periode (0-20) |
| 32 | G3 | Abschlussnote (0-20) |

---

## Aufgaben

### Aufgabe 1 – Daten laden und erkunden

- [ ] Lade den Datensatz `student-mat.csv` und wähle nur die numerischen Spalten (ab Spalte 2)
- [ ] Gib Shape, Anzahl Schüler und Anzahl Merkmale aus
- [ ] Prüfe auf fehlende Werte (NaN) – sowohl insgesamt als auch pro Spalte
- [ ] Extrahiere die relevanten Spalten in separate Variablen: `alter`, `medu`, `fedu`, `studytime`, `freetime`, `dalc`, `walc`, `absences`, `g1`, `g2`, `g3`
- [ ] Gib den Wertebereich des Alters aus (Minimum bis Maximum)

!!! tip "Hilfe"
    - Datei laden: `np.genfromtxt(pfad, delimiter=',', skip_header=1, usecols=range(start, ende))`
    - NaN zählen: `np.isnan(arr).sum()` oder mit `axis=0` pro Spalte
    - Spalte extrahieren: `daten[:, spalten_index]`
    - Die genauen Spaltenindizes können nach dem Filtern variieren – prüfe die Wertebereiche!

---

### Aufgabe 2 – Deskriptive Statistik

- [ ] Berechne für die Abschlussnote (G3): Mittelwert, Median, Standardabweichung, Minimum, Maximum und Spannweite
- [ ] Berechne die Quartile (25., 50., 75., 90. Perzentil) der Abschlussnote
- [ ] Erstelle eine Notenverteilung: Zähle, wie viele Schüler in den Bereichen 0-4, 5-9, 10-14, 15-20 liegen (absolut und in Prozent)
- [ ] Berechne die Durchfallquote (Note < 10)

!!! tip "Hilfe"
    - Statistische Funktionen: `np.nanmean()`, `np.nanmedian()`, `np.nanstd()`, `np.nanmin()`, `np.nanmax()`
    - Perzentile: `np.nanpercentile(arr, p)` oder mehrere: `np.nanpercentile(arr, [25, 50, 75])`
    - Bereichsfilter: `((arr >= unter) & (arr < ober)).sum()`

---

### Aufgabe 3 – Alkoholkonsum analysieren

- [ ] Berechne den Durchschnitt des Alkoholkonsums an Werktagen (dalc) und am Wochenende (walc)
- [ ] Erstelle einen kombinierten Alkohol-Score: `alkohol_gesamt = dalc + walc`
- [ ] Zeige die Verteilung des Wochenendkonsums: Wie viele Schüler haben Level 1, 2, 3, 4, 5?
- [ ] Kategorisiere die Schüler nach Alkoholkonsum:
    - Niedrig: Gesamtscore ≤ 4
    - Mittel: Gesamtscore 5-6
    - Hoch: Gesamtscore ≥ 7
- [ ] Gib für jede Kategorie die Anzahl und den Prozentsatz aus

!!! tip "Hilfe"
    - Bedingung prüfen: `(arr == wert).sum()` für Anzahl
    - Mehrere Bedingungen: `(arr > x) & (arr <= y)`
    - Prozent berechnen: `anzahl / len(arr) * 100`

---

### Aufgabe 4 – Zusammenhang Alkohol und Noten

!!! question "Forschungsfrage"
    Hat Alkoholkonsum einen messbaren Zusammenhang mit den Schulnoten?

- [ ] Vergleiche die Durchschnittsnoten und Standardabweichungen für die drei Konsumgruppen (niedrig, mittel, hoch)
- [ ] Berechne die Durchfallquoten (Note < 10) für jede Konsumgruppe
- [ ] Berechne die Korrelation zwischen `alkohol_gesamt` und `g3`
- [ ] Interpretiere die Korrelation: Ist sie schwach/mittel/stark? Positiv oder negativ?

!!! tip "Hilfe"
    - Filtern mit Maske: `np.nanmean(g3[konsum_niedrig])`
    - Korrelation: `np.corrcoef(arr1, arr2)[0, 1]` (gibt Matrix zurück, Element [0,1] ist die Korrelation)
    - Gültige Werte filtern: `gueltig = ~np.isnan(arr1) & ~np.isnan(arr2)`
    - Interpretation: |r| < 0.1 sehr schwach, < 0.3 schwach, < 0.5 mittel, ≥ 0.5 stark

---

### Aufgabe 5 – Weitere Einflussfaktoren

- [ ] **Lernzeit und Noten:** Berechne die Durchschnittsnote für jede Lernzeit-Stufe (1-4) und die Korrelation
- [ ] **Elternbildung und Noten:** Erstelle einen kombinierten Bildungsindex (medu + fedu) und vergleiche Durchschnittsnoten für niedrige (≤3), mittlere (4-5) und hohe (>5) Elternbildung
- [ ] **Fehlstunden und Noten:** Vergleiche Durchschnittsnoten für Schüler mit wenig (0-3), mittleren (4-10) und vielen (>10) Fehlstunden
- [ ] Berechne die Korrelation zwischen Fehlstunden und Abschlussnote

!!! tip "Hilfe"
    - Durchschnitt pro Gruppe: `np.nanmean(g3[studytime == zeit])`
    - Erst prüfen ob Gruppe existiert: `if (maske).sum() > 0:`

---

## Vertiefende Aufgaben

!!! tip "Optionale Aufgaben zur Vertiefung"
    Die folgenden Aufgaben sind **optional** und vertiefen das Gelernte. Sie eignen sich besonders für:
    
    - **Multifaktor-Analysen**
    - **Dokumentation von Erkenntnissen**
    - **Bonus: Geschlechtervergleich, Risikoprofile, Vorhersagemodelle**
    - **Prüfungsvorbereitung** durch eigenständiges Arbeiten

---

### Aufgabe 6 – Komplexe Analyse

- [ ] **Multifaktor-Analyse:** Finde Schüler mit den besten Voraussetzungen (hohe Lernzeit ≥3 UND niedriger Alkohol) und berechne deren Durchschnittsnote und Durchfallquote
- [ ] Finde Schüler mit den schlechtesten Voraussetzungen (niedrige Lernzeit ≤1 UND hoher Alkohol) und vergleiche
- [ ] **Notenentwicklung:** Berechne die Durchschnittsnoten für G1, G2 und G3 – gibt es einen Trend?
- [ ] Bestimme, wie viele Schüler sich verbessert, verschlechtert oder gleich geblieben sind (G3 vs. G1)
- [ ] **Top und Bottom Performer:** Finde die Top 10% und Bottom 10% der Schüler und vergleiche deren durchschnittlichen Alkoholkonsum, Lernzeit und Fehlstunden

!!! tip "Hilfe"
    - Mehrere Bedingungen kombinieren: `(bedingung1) & (bedingung2)`
    - Verbesserung: `(g3 > g1).sum()`
    - Perzentil-Grenze finden: `grenze = np.nanpercentile(g3, 90)` → Top 10%: `g3 >= grenze`

---

### Aufgabe 7 – Erkenntnisse dokumentieren

Erstelle eine Zusammenfassung deiner Analyse und beantworte folgende Fragen basierend auf deinen Ergebnissen:

1. Wie stark ist der Zusammenhang zwischen Alkoholkonsum und Noten?
2. Welcher Faktor hat den stärksten Einfluss auf die Noten?
3. Welche Kombination von Faktoren führt zu den besten Ergebnissen?
4. Wie hoch ist die Durchfallquote in verschiedenen Gruppen?
5. Was sind Limitationen dieser Analyse?

!!! warning "Kausalität vs. Korrelation"
    Korrelationen zeigen nur **Zusammenhänge**, keine **Ursachen**!
    
    Dass Alkoholkonsum mit schlechteren Noten korreliert, bedeutet nicht automatisch:
    
    - dass Alkohol die Noten verschlechtert
    - oder dass schlechte Noten zu mehr Alkoholkonsum führen
    
    Es könnten auch andere Faktoren (z.B. soziales Umfeld) beides beeinflussen!

---

## Bonus-Aufgaben

!!! warning "Ohne Hilfe lösen"
    Bearbeite diese Aufgaben selbstständig ohne Hilfestellungen.

**A) Vergleiche Geschlechter:**

- Lade den Datensatz erneut mit der Geschlechter-Spalte
- Vergleiche Noten und Alkoholkonsum zwischen Geschlechtern

**B) Erstelle Risikoprofile:**

- Definiere selbst Kriterien für "Risiko-Schüler" (z.B. hoher Alkohol, wenig Lernzeit, viele Fehlstunden)
- Wie viele Schüler erfüllen diese Kriterien?
- Wie hoch ist deren Durchfallquote?

**C) Vorhersage-Modell:**

- Nutze G1 und G2 um G3 vorherzusagen
- Wie genau ist die Vorhersage `G3 ≈ (G1 + G2) / 2`?
- Berechne den mittleren absoluten Fehler dieser Vorhersage

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **Explorative Analyse**: Datensatz verstehen durch Statistiken
    - **Gruppenvergleiche**: Unterschiede zwischen Gruppen quantifizieren
    - **Korrelationen**: Zusammenhänge messen und interpretieren
    - **Multifaktor-Analyse**: Mehrere Variablen kombiniert betrachten
    - **Kritisches Denken**: Korrelation ≠ Kausalität

---

??? question "Selbstkontrolle"
    1. Was sagt ein Korrelationskoeffizient von -0.25 aus?
    2. Warum ist die Durchfallquote aussagekräftiger als der Notendurchschnitt?
    3. Wie würdest du prüfen, ob ein Zusammenhang statistisch signifikant ist?
    4. Welche Daten fehlen, um Kausalität nachzuweisen?
    
    ??? success "Antworten"
        1. Ein schwacher negativer Zusammenhang: Wenn X steigt, sinkt Y tendenziell (aber nicht stark)
        2. Sie zeigt das Risiko eines konkreten negativen Outcomes; Durchschnitte können durch Ausreißer verzerrt sein
        3. Mit statistischen Tests (t-Test, Chi-Quadrat-Test) – diese sind aber nicht Teil von NumPy allein
        4. Longitudinale Daten (über Zeit), Kontrollgruppen, Randomisierung – also ein echtes Experiment statt Beobachtungsstudie
