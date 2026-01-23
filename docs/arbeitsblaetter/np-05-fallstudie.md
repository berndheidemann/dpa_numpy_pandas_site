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

- [ ] **Lade den Datensatz:**
    ```python
    import numpy as np
    
    # Datensatz laden
    # Die ersten Spalten sind kategorisch (Text), wir nutzen nur numerische
    daten = np.genfromtxt('../assets/files/student-mat.csv',
                          delimiter=',',
                          skip_header=1,
                          usecols=range(2, 33))  # Nur numerische Spalten
    
    print(f"Shape: {daten.shape}")
    print(f"Anzahl Schüler: {daten.shape[0]}")
    print(f"Anzahl Merkmale: {daten.shape[1]}")
    ```

- [ ] **Prüfe auf fehlende Werte:**
    ```python
    nan_count = np.isnan(daten).sum()
    print(f"Fehlende Werte (NaN): {nan_count}")
    
    # Pro Spalte
    nan_per_col = np.isnan(daten).sum(axis=0)
    print(f"\nFehlende Werte pro Spalte: {nan_per_col}")
    ```

- [ ] **Relevante Spalten extrahieren:**
    ```python
    # Spaltenindizes nach dem Filtern (ab Spalte 2)
    alter = daten[:, 0]           # age
    medu = daten[:, 4]            # Mutter Bildung
    fedu = daten[:, 5]            # Vater Bildung
    studytime = daten[:, 12]      # Lernzeit
    freetime = daten[:, 22]       # Freizeit
    dalc = daten[:, 24]           # Alkohol Werktag
    walc = daten[:, 25]           # Alkohol Wochenende
    absences = daten[:, 27]       # Fehlstunden
    g1 = daten[:, 28]             # Note 1
    g2 = daten[:, 29]             # Note 2
    g3 = daten[:, 30]             # Abschlussnote
    
    print(f"Alter-Bereich: {alter.min():.0f} - {alter.max():.0f}")
    ```

!!! tip "Hinweis"
    Die genauen Spaltenindizes können variieren. Prüfe die Wertebereiche!

---

### Aufgabe 2 – Deskriptive Statistik

- [ ] **Berechne Grundstatistiken für die Abschlussnote:**
    ```python
    print("=== Abschlussnote (G3) ===")
    print(f"Mittelwert: {np.nanmean(g3):.2f}")
    print(f"Median: {np.nanmedian(g3):.2f}")
    print(f"Standardabweichung: {np.nanstd(g3):.2f}")
    print(f"Minimum: {np.nanmin(g3):.0f}")
    print(f"Maximum: {np.nanmax(g3):.0f}")
    print(f"Spannweite: {np.nanmax(g3) - np.nanmin(g3):.0f}")
    ```

- [ ] **Quartile und Perzentile:**
    ```python
    print("\nPerzentile der Abschlussnote:")
    for p in [25, 50, 75, 90]:
        wert = np.nanpercentile(g3, p)
        print(f"  {p}. Perzentil: {wert:.1f}")
    ```

- [ ] **Notenverteilung:**
    ```python
    # Wie viele Schüler haben welche Note?
    print("\nNotenverteilung:")
    for note in range(0, 21, 5):
        anzahl = ((g3 >= note) & (g3 < note + 5)).sum()
        print(f"  {note:2d}-{note+4:2d}: {anzahl:3d} Schüler ({anzahl/len(g3)*100:.1f}%)")
    
    # Durchfallquote (Note < 10)
    durchgefallen = (g3 < 10).sum()
    print(f"\nDurchfallquote: {durchgefallen/len(g3)*100:.1f}%")
    ```

---

### Aufgabe 3 – Alkoholkonsum analysieren

- [ ] **Gesamtalkoholkonsum berechnen:**
    ```python
    # Kombinierter Score: Werktag + Wochenende
    alkohol_gesamt = dalc + walc
    
    print("=== Alkoholkonsum ===")
    print(f"Durchschnitt Werktag (1-5): {np.nanmean(dalc):.2f}")
    print(f"Durchschnitt Wochenende (1-5): {np.nanmean(walc):.2f}")
    print(f"Durchschnitt Gesamt (2-10): {np.nanmean(alkohol_gesamt):.2f}")
    ```

- [ ] **Verteilung des Wochenendkonsums:**
    ```python
    print("\nWochenendkonsum Verteilung:")
    for level in range(1, 6):
        anzahl = (walc == level).sum()
        print(f"  Level {level}: {anzahl:3d} Schüler ({anzahl/len(walc)*100:.1f}%)")
    ```

- [ ] **Kategorisiere nach Alkoholkonsum:**
    ```python
    # Niedrig: gesamt <= 4, Mittel: 5-6, Hoch: >= 7
    konsum_niedrig = alkohol_gesamt <= 4
    konsum_mittel = (alkohol_gesamt > 4) & (alkohol_gesamt <= 6)
    konsum_hoch = alkohol_gesamt >= 7
    
    print("\nKonsumkategorien:")
    print(f"  Niedrig: {konsum_niedrig.sum()} ({konsum_niedrig.sum()/len(alkohol_gesamt)*100:.1f}%)")
    print(f"  Mittel: {konsum_mittel.sum()} ({konsum_mittel.sum()/len(alkohol_gesamt)*100:.1f}%)")
    print(f"  Hoch: {konsum_hoch.sum()} ({konsum_hoch.sum()/len(alkohol_gesamt)*100:.1f}%)")
    ```

---

### Aufgabe 4 – Zusammenhang Alkohol und Noten

!!! question "Forschungsfrage"
    Hat Alkoholkonsum einen messbaren Zusammenhang mit den Schulnoten?

- [ ] **Noten nach Konsumgruppen vergleichen:**
    ```python
    print("=== Noten nach Alkoholkonsum ===")
    print(f"Niedriger Konsum:")
    print(f"  Durchschnittsnote: {np.nanmean(g3[konsum_niedrig]):.2f}")
    print(f"  Standardabweichung: {np.nanstd(g3[konsum_niedrig]):.2f}")
    
    print(f"\nMittlerer Konsum:")
    print(f"  Durchschnittsnote: {np.nanmean(g3[konsum_mittel]):.2f}")
    print(f"  Standardabweichung: {np.nanstd(g3[konsum_mittel]):.2f}")
    
    print(f"\nHoher Konsum:")
    print(f"  Durchschnittsnote: {np.nanmean(g3[konsum_hoch]):.2f}")
    print(f"  Standardabweichung: {np.nanstd(g3[konsum_hoch]):.2f}")
    ```

- [ ] **Durchfallquoten vergleichen:**
    ```python
    print("\nDurchfallquoten nach Konsum:")
    print(f"  Niedrig: {(g3[konsum_niedrig] < 10).sum() / konsum_niedrig.sum() * 100:.1f}%")
    print(f"  Mittel: {(g3[konsum_mittel] < 10).sum() / konsum_mittel.sum() * 100:.1f}%")
    print(f"  Hoch: {(g3[konsum_hoch] < 10).sum() / konsum_hoch.sum() * 100:.1f}%")
    ```

- [ ] **Korrelation berechnen:**
    ```python
    # Einfache Korrelation (nur für gültige Werte)
    gueltig = ~np.isnan(alkohol_gesamt) & ~np.isnan(g3)
    korr = np.corrcoef(alkohol_gesamt[gueltig], g3[gueltig])[0, 1]
    
    print(f"\nKorrelation Alkohol-Note: {korr:.3f}")
    
    # Interpretation
    if abs(korr) < 0.1:
        staerke = "sehr schwach"
    elif abs(korr) < 0.3:
        staerke = "schwach"
    elif abs(korr) < 0.5:
        staerke = "mittel"
    else:
        staerke = "stark"
    
    richtung = "negativ" if korr < 0 else "positiv"
    print(f"Interpretation: {staerke}er {richtung}er Zusammenhang")
    ```

---

### Aufgabe 5 – Weitere Einflussfaktoren

- [ ] **Lernzeit und Noten:**
    ```python
    print("=== Lernzeit und Noten ===")
    for zeit in range(1, 5):
        maske = studytime == zeit
        if maske.sum() > 0:
            print(f"Lernzeit {zeit}: Ø Note {np.nanmean(g3[maske]):.2f} (n={maske.sum()})")
    
    korr_lernzeit = np.corrcoef(studytime[~np.isnan(studytime) & ~np.isnan(g3)],
                                 g3[~np.isnan(studytime) & ~np.isnan(g3)])[0, 1]
    print(f"\nKorrelation Lernzeit-Note: {korr_lernzeit:.3f}")
    ```

- [ ] **Elternbildung und Noten:**
    ```python
    # Kombinierter Bildungsindex der Eltern
    eltern_bildung = medu + fedu
    
    print("\n=== Elternbildung und Noten ===")
    # Niedrig (0-3), Mittel (4-5), Hoch (6-8)
    niedrig = eltern_bildung <= 3
    mittel = (eltern_bildung > 3) & (eltern_bildung <= 5)
    hoch = eltern_bildung > 5
    
    print(f"Niedrige Elternbildung: Ø Note {np.nanmean(g3[niedrig]):.2f}")
    print(f"Mittlere Elternbildung: Ø Note {np.nanmean(g3[mittel]):.2f}")
    print(f"Hohe Elternbildung: Ø Note {np.nanmean(g3[hoch]):.2f}")
    ```

- [ ] **Fehlstunden und Noten:**
    ```python
    print("\n=== Fehlstunden und Noten ===")
    wenig = absences <= 3
    mittel_abs = (absences > 3) & (absences <= 10)
    viel = absences > 10
    
    print(f"Wenig Fehlstunden (0-3): Ø Note {np.nanmean(g3[wenig]):.2f}")
    print(f"Mittlere Fehlstunden (4-10): Ø Note {np.nanmean(g3[mittel_abs]):.2f}")
    print(f"Viele Fehlstunden (>10): Ø Note {np.nanmean(g3[viel]):.2f}")
    
    korr_fehl = np.corrcoef(absences[~np.isnan(absences) & ~np.isnan(g3)],
                            g3[~np.isnan(absences) & ~np.isnan(g3)])[0, 1]
    print(f"\nKorrelation Fehlstunden-Note: {korr_fehl:.3f}")
    ```

---

### Aufgabe 6 – Komplexe Analyse

- [ ] **Multifaktor-Analyse:**
    ```python
    print("=== Kombinierte Analyse ===")
    print("Beste Voraussetzungen (hohe Lernzeit + niedriger Alkohol):")
    beste = (studytime >= 3) & konsum_niedrig
    print(f"  Anzahl: {beste.sum()}")
    print(f"  Ø Note: {np.nanmean(g3[beste]):.2f}")
    print(f"  Durchfallquote: {(g3[beste] < 10).sum() / beste.sum() * 100:.1f}%")
    
    print("\nSchlechteste Voraussetzungen (niedrige Lernzeit + hoher Alkohol):")
    schlechteste = (studytime <= 1) & konsum_hoch
    print(f"  Anzahl: {schlechteste.sum()}")
    if schlechteste.sum() > 0:
        print(f"  Ø Note: {np.nanmean(g3[schlechteste]):.2f}")
        print(f"  Durchfallquote: {(g3[schlechteste] < 10).sum() / schlechteste.sum() * 100:.1f}%")
    ```

- [ ] **Notenentwicklung über das Jahr:**
    ```python
    print("\n=== Notenentwicklung ===")
    print(f"Periode 1 (G1): Ø {np.nanmean(g1):.2f}")
    print(f"Periode 2 (G2): Ø {np.nanmean(g2):.2f}")
    print(f"Abschluss (G3): Ø {np.nanmean(g3):.2f}")
    
    # Wer hat sich verbessert/verschlechtert?
    verbessert = g3 > g1
    verschlechtert = g3 < g1
    gleich = g3 == g1
    
    print(f"\nVerbessert: {verbessert.sum()} ({verbessert.sum()/len(g3)*100:.1f}%)")
    print(f"Verschlechtert: {verschlechtert.sum()} ({verschlechtert.sum()/len(g3)*100:.1f}%)")
    print(f"Gleich geblieben: {gleich.sum()} ({gleich.sum()/len(g3)*100:.1f}%)")
    ```

- [ ] **Beste und schlechteste Schüler:**
    ```python
    print("\n=== Top und Bottom Performer ===")
    # Top 10%
    top_grenze = np.nanpercentile(g3, 90)
    top_schueler = g3 >= top_grenze
    
    print(f"Top 10% (Note >= {top_grenze:.0f}):")
    print(f"  Ø Alkohol: {np.nanmean(alkohol_gesamt[top_schueler]):.2f}")
    print(f"  Ø Lernzeit: {np.nanmean(studytime[top_schueler]):.2f}")
    print(f"  Ø Fehlstunden: {np.nanmean(absences[top_schueler]):.1f}")
    
    # Bottom 10%
    bottom_grenze = np.nanpercentile(g3, 10)
    bottom_schueler = g3 <= bottom_grenze
    
    print(f"\nBottom 10% (Note <= {bottom_grenze:.0f}):")
    print(f"  Ø Alkohol: {np.nanmean(alkohol_gesamt[bottom_schueler]):.2f}")
    print(f"  Ø Lernzeit: {np.nanmean(studytime[bottom_schueler]):.2f}")
    print(f"  Ø Fehlstunden: {np.nanmean(absences[bottom_schueler]):.1f}")
    ```

---

### Aufgabe 7 – Erkenntnisse dokumentieren

- [ ] **Erstelle eine Zusammenfassung deiner Analyse:**
    
    Beantworte folgende Fragen basierend auf deinen Ergebnissen:
    
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

??? tip "Für Fortgeschrittene"
    **A) Vergleiche Geschlechter:**
    - Lade den Datensatz erneut mit der Geschlechter-Spalte
    - Vergleiche Noten und Alkoholkonsum zwischen Geschlechtern
    
    **B) Erstelle Risikoprofile:**
    - Definiere Kriterien für "Risiko-Schüler"
    - Wie viele Schüler erfüllen diese Kriterien?
    
    **C) Vorhersage-Modell:**
    - Nutze G1 und G2 um G3 vorherzusagen
    - Wie genau ist die Vorhersage `G3 ≈ (G1 + G2) / 2`?

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
