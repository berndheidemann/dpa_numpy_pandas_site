# Didaktische Analyse der Arbeitsblätter

## Zusammenfassung

| AB | Thema | Code-Anteil | Eigenleistung | Bewertung | Empfehlung |
|----|-------|-------------|---------------|-----------|------------|
| 00 | Vorbereitungen | 🟡 Mittel | 🟢 Gut | ⭐⭐⭐⭐ | OK |
| 01 | Verzweigungen | 🟢 Niedrig | 🟢 Sehr gut | ⭐⭐⭐⭐⭐ | Vorbildlich |
| 02 | Schleifen | 🟢 Niedrig | 🟢 Sehr gut | ⭐⭐⭐⭐⭐ | Vorbildlich |
| 03 | Listen | 🟢 Niedrig | 🟢 Sehr gut | ⭐⭐⭐⭐⭐ | Vorbildlich |
| 04 | Dictionaries | 🟡 Mittel | 🟡 Mittel | ⭐⭐⭐⭐ | Leicht anpassen |
| 05 | OOP Grundlagen | 🔴 Hoch | 🔴 Gering | ⭐⭐ | **Überarbeiten** |
| 06 | Abschlussprojekt | 🔴 Sehr hoch | 🔴 Sehr gering | ⭐ | **Dringend überarbeiten** |

---

## Detailanalyse

### AB 00 – Vorbereitungen & Erste Schritte ⭐⭐⭐⭐

**Positiv:**
- Installation und Setup gut erklärt
- Progression von einfach zu komplex (Hello World → BMI-Rechner)
- Aufgaben 8 + 10 sind echte Eigenleistung (kein Code vorgegeben)
- Gute Mischung aus Nachmachen und Selbstmachen

**Code-Vorgaben:**
- Aufgabe 1–7: Vollständiger Code vorgegeben ✓ (sinnvoll bei Einstieg)
- Aufgabe 8: Nur Anforderungen, kein Code ✓
- Aufgabe 9: Teilcode + Experimentieren ✓
- Aufgabe 10: Nur Formel, kein Code ✓

**Fazit:** Gute Balance für ein Einführungsblatt.

---

### AB 01 – Verzweigungen ⭐⭐⭐⭐⭐

**Positiv:**
- ✅ **Keine Musterlösungen vorgegeben** – Schüler müssen selbst programmieren
- ✅ Klare Anforderungstabellen statt Code
- ✅ Tipps nur für Syntax (kein Lösungscode)
- ✅ Aufgaben steigen progressiv im Schwierigkeitsgrad
- ✅ Praxisnahe Szenarien (Kreditprüfung, Server-Monitoring)

**Code-Vorgaben:**
- Nur syntaktische Hinweise (z.B. wie man ja/nein einliest)
- Keine fertigen Lösungen

**Fazit:** **Vorbildlich!** So sollten alle Blätter aussehen.

---

### AB 02 – Schleifen ⭐⭐⭐⭐⭐

**Positiv:**
- ✅ Code nur als Strukturhilfe, nie als Lösung
- ✅ Tipps zeigen Patterns, nicht Lösungen
- ✅ Aufgaben erfordern eigenständiges Denken
- ✅ Bonus-Aufgaben für Schnelle

**Code-Vorgaben:**
- Aufgabe 1: Nur while-Struktur skizziert
- Aufgabe 3: Nur `random.choice()` erklärt
- Aufgabe 5: Grundgerüst, aber Logik muss selbst entwickelt werden

**Fazit:** **Vorbildlich!** Schüler müssen aktiv programmieren.

---

### AB 03 – Listen ⭐⭐⭐⭐⭐

**Positiv:**
- ✅ Keine fertigen Funktionen vorgegeben
- ✅ Funktionssignaturen als Gerüst (gut für Struktur)
- ✅ Test-Code zeigt erwartetes Verhalten, nicht Implementierung
- ✅ Daten vorgegeben, Verarbeitung muss selbst geschrieben werden

**Code-Vorgaben:**
- Nur Datensätze (Listen mit Werten)
- Leere Funktionsrümpfe oder nur Signaturen
- List Comprehension Aufgaben mit `???` Platzhaltern

**Fazit:** **Vorbildlich!** Anspruchsvoll, aber lösbar.

---

### AB 04 – Dictionaries ⭐⭐⭐⭐

**Positiv:**
- ✅ Gute Praxisbeispiele
- ✅ Aufgabe 3 (CSV) zeigt Technik, Analyse muss selbst gemacht werden
- ✅ Aufgabe 5/6 erfordern Eigenleistung

**Kritisch:**
- ⚠️ Aufgabe 3: `read_csv()`-Funktion vollständig vorgegeben – könnte als Aufgabe gestellt werden
- ⚠️ Teilweise zu viele Hinweise bei Dict-Comprehensions

**Empfehlung:**
- `read_csv()` als Aufgabe formulieren statt vorgeben
- Oder: zwei Varianten anbieten (mit/ohne Hilfe)

---

### AB 05 – OOP Grundlagen ⭐⭐

**Kritisch:**
- 🔴 **Fast alle Klassen sind vollständig implementiert**
- 🔴 Schüler müssen nur abtippen und testen
- 🔴 Kaum Eigenleistung erforderlich
- 🔴 Lerneffekt: gering (Copy & Paste)

**Konkrete Probleme:**

| Aufgabe | Problem |
|---------|---------|
| 1 | `Messwert`-Klasse komplett vorgegeben, nur `ist_im_bereich()` selbst |
| 2 | `Verkauf`-Klasse komplett vorgegeben, nur `berechne_mit_rabatt()` selbst |
| 3 | Alles vorgegeben |
| 4 | `Sensor`-Klasse komplett vorgegeben |
| 5 | `Messstation`-Klasse komplett vorgegeben |
| 6 | Vererbung: `Sensor`, `TemperaturSensor`, `DruckSensor` komplett vorgegeben |

**Empfehlung – Struktur umkehren:**

Statt vollständigen Code zu zeigen, nur vorgeben:
1. **Klassenname und Zweck**
2. **Benötigte Attribute** (als Liste)
3. **Methodensignaturen** (ohne Implementierung)
4. **Testcode** (zeigt erwartetes Verhalten)

**Beispiel für Aufgabe 1 (verbessert):**

```markdown
### Aufgabe 1 – Klasse Messwert

Erstelle eine Klasse `Messwert` mit folgenden Eigenschaften:

**Attribute:**
- `wert` – der Messwert als Zahl
- `einheit` – die Einheit als String (z.B. "°C")

**Methoden:**
- `__init__(self, wert, einheit)` – Konstruktor
- `__str__(self)` – gibt "23.5 °C" zurück
- `ist_im_bereich(self, min, max)` – prüft, ob Wert im Bereich liegt

**Test:**
```python
temp = Messwert(23.5, "°C")
print(temp)                        # 23.5 °C
print(temp.ist_im_bereich(10, 30)) # True
```
```

---

### AB 06 – Abschlussprojekt ⭐

**Kritisch:**
- 🔴 **Das gesamte Projekt ist fertig implementiert**
- 🔴 Jede Aufgabe enthält den vollständigen Lösungscode
- 🔴 Schüler müssen nur Copy & Paste machen
- 🔴 Kein Projektcharakter – eher "Abschreib-Übung"

**Konkrete Probleme:**

| Aufgabe | Was vorgegeben ist |
|---------|-------------------|
| 2 | Komplette `Verkauf`-Klasse |
| 3 | Komplette `lade_verkaeufe()`-Funktion |
| 4 | Alle drei Auswertungsfunktionen komplett |
| 5 | Das gesamte `main()`-Programm |
| 6 | Alle Erweiterungen komplett |

**Das ist kein Projekt, sondern eine Musterlösung!**

**Empfehlung – Projektstruktur:**

```markdown
### Aufgabe 1 – Klasse Verkauf
Erstelle eine Klasse `Verkauf` mit:
- Attributen für datum, produkt, kategorie, menge, preis
- Methode `umsatz()` die menge × preis berechnet
- Methode `__str__()` für lesbare Ausgabe

### Aufgabe 2 – CSV einlesen
Schreibe eine Funktion `lade_verkaeufe(pfad)`, die:
- Die CSV-Datei öffnet
- Für jede Zeile ein Verkauf-Objekt erstellt
- Eine Liste aller Verkäufe zurückgibt

!!! tip "Hinweis"
    Nutze `csv.DictReader` – siehe Infoblatt.

### Aufgabe 3 – Auswertungen
Implementiere folgende Funktionen:
- `gesamtumsatz(verkaeufe)` → Summe aller Umsätze
- `umsatz_nach_kategorie(verkaeufe)` → Dictionary {Kategorie: Summe}
- `top_verkauf(verkaeufe)` → Verkauf mit höchstem Einzelumsatz

### Aufgabe 4 – Hauptprogramm
Schreibe ein Hauptprogramm, das:
1. Die Daten lädt
2. Die Auswertungen durchführt
3. Die Ergebnisse formatiert ausgibt
```

---

## Gesamtbewertung

### Was gut funktioniert:
- ✅ AB 01–03 sind didaktisch vorbildlich
- ✅ Praxisnahe Szenarien für Datenanalysten
- ✅ Gute Progression der Schwierigkeit
- ✅ Infoblätter als Nachschlagewerk

### Was verbessert werden sollte:

| Priorität | Arbeitsblatt | Problem | Aufwand |
|-----------|--------------|---------|---------|
| 🔴 Hoch | AB 06 | Komplette Lösung vorgegeben | Groß |
| 🔴 Hoch | AB 05 | 90% Code vorgegeben | Mittel |
| 🟡 Mittel | AB 04 | `read_csv()` könnte Aufgabe sein | Klein |

---

## Empfohlene Änderungen

### AB 05 – OOP Grundlagen

**Vorher:** Code vorgeben + Test
**Nachher:** Anforderungen + Methodensignaturen + Test

Jede Aufgabe sollte enthalten:
1. **Szenario** (was soll die Klasse darstellen)
2. **Attribute** (welche Daten werden gespeichert)
3. **Methoden** (nur Name und Zweck)
4. **Testcode** (wie soll es sich verhalten)

### AB 06 – Abschlussprojekt

**Vorher:** Schritt-für-Schritt Musterlösung
**Nachher:** Projektbeschreibung + Meilensteine + Bewertungskriterien

Das Projekt sollte:
1. **Anforderungen** klar beschreiben (was soll das Programm können)
2. **Meilensteine** definieren (nicht Lösungsschritte)
3. **Tipps** nur auf Techniken verweisen (nicht Lösungen zeigen)
4. **Bewertung** transparent machen

---

## Fazit

Die frühen Arbeitsblätter (01–03) sind **didaktisch gut gelungen**: Sie geben Struktur ohne Lösungen vorzugeben. Die späteren Blätter (05–06) haben den umgekehrten Ansatz: Fast alles ist vorgegeben, was den Lerneffekt stark reduziert.

**Kernproblem:** Bei OOP und dem Abschlussprojekt wird der Code "zum Abschreiben" präsentiert statt als Aufgabe gestellt.

**Empfehlung:** AB 05 und AB 06 nach dem Muster von AB 01–03 überarbeiten – Anforderungen statt Lösungen.
