# Die Welt von Zuul – State-Pattern

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- Das State-Pattern erklären und anwenden
- Zustandsabhängiges Verhalten in separate Klassen auslagern
- Das Singleton-Pattern zur Optimierung einsetzen
- Zustandsübergänge modellieren und implementieren

---

## Zustandswechsel von Spielern

Unser Adventuregame „Die Welt von Zuul" soll um verschiedene **Zustände**, die der Spieler annehmen kann, erweitert werden. Im Hinblick auf zukünftige Erweiterungen, in denen Spieler gegeneinander oder mit anderen Figuren kämpfen können, werden diese Zustände benötigt.

### Spieler-Zustände

Ein Spieler kann **leicht** oder **schwer** getroffen werden. Ein Spieler kann aber auch **geheilt** werden.

Spieler haben drei unterschiedliche Zustände ihres Wohlbefindens:

- **gesund**
- **verwundet**
- **bewegungsunfähig**

### Zustandsübergänge

| Aktueller Zustand | Aktion | Neuer Zustand |
|-------------------|--------|---------------|
| gesund | heilen | gesund |
| gesund | leicht verletzen | verwundet |
| gesund | schwer verletzen | bewegungsunfähig |
| verwundet | heilen | gesund |
| verwundet | verletzen (egal ob leicht oder schwer) | bewegungsunfähig |
| bewegungsunfähig | heilen | verwundet |
| bewegungsunfähig | verletzen | bewegungsunfähig |

Eine Figur soll ihren aktuellen Zustand als String zurückgeben können.

## Aufgaben

### Aufgabe 1 – Klassendiagramm erstellen

- [ ] Zeichne ein Klassendiagramm für das Problem

!!! note "Hinweis"
    Das Klassendiagramm soll von den bisher erstellten Klassen lediglich die Klasse `Player` enthalten.

### Aufgabe 2 – State-Muster anwenden

- [ ] Lies das [Infoblatt State-Pattern](../infoblaetter/state-pattern.md) und mache dich mit dem Muster vertraut
- [ ] Verbessere das Klassendiagramm aus Aufgabe 1

### Aufgabe 3 – Implementierung

- [ ] Implementiere den im Plenum besprochenen Algorithmus
- [ ] Lege für die Zustände im Package `model` ein weiteres Package für die Zustände an
- [ ] Ergänze die Methode `showStatus()` in der Klasse `Player` um die Anzeige des aktuellen Zustands

!!! tip "Interfaces nutzen"
    Lies das [Infoblatt Interfaces](../infoblaetter/interfaces.md), falls du Hilfe bei der Erstellung des State-Interfaces benötigst.

### Aufgabe 4 – Test

- [ ] Lass den Spieler mit dem Zustand „verwundet" starten
- [ ] Erstelle eine Kindklasse `Herb` der Klasse `Item`
- [ ] Ändere den Datentyp des im Dschungel abgelegten Heilkrauts auf `Herb`
- [ ] Ändere die Methode `eat()` der Klasse `Game` so ab, dass nicht nur Muffins, sondern auch Heilkräuter gegessen werden können
- [ ] Wird ein Heilkraut gegessen, verbessert sich der Zustand des Spielers um einen Zustand

## Verbesserung: Singleton und Entkopplung

Die bisherige Variante unseres State-Patterns weist zwei Nachteile auf:

!!! danger "Problem a)"
    Bei jedem Zustandswechsel wird ein neues Zustandsobjekt erzeugt und das alte verbleibt solange unreferenziert im Speicher, bis es der Garbage Collector abräumt.

!!! danger "Problem b)"
    Die Zustände speichern für die Zustandsänderung eine Referenz auf die konkrete Klasse `Player`, um die Zustandsänderung herbeizuführen. Was ist, wenn in späteren Erweiterungen weitere Kreaturen diese Zustände annehmen sollen? Es ist also eine stärkere Entkopplung der Zustände vom Spieler nötig.

### Aufgabe 5 – Singleton-Refactoring

- [ ] Führe für die Zustandsklassen ein Refactoring durch, indem du jeden Zustand in ein **Singleton** verwandelst

!!! tip "Hilfe"
    Lese dafür das [Infoblatt Singleton-Muster](../infoblaetter/singleton.md).

### Aufgabe 6 – Rückgabe des neuen Zustands

- [ ] Ändere die Methoden zur Zustandsänderung in den einzelnen Zuständen so ab, dass sie den **neuen Zustand zurückgeben**
- [ ] Ändere den Aufruf der Methoden in der Klasse `Player` entsprechend ab

### Aufgabe 7 – Versionierung

- [ ] Versioniere deinen aktuellen Projektstand:
    ```bash
    git status
    git add -A
    git commit -m "zuul 9"
    ```

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **State-Pattern**: Zustände als eigene Klassen kapseln
    - **Polymorphie**: Unterschiedliches Verhalten je nach Zustand
    - **Singleton**: Nur eine Instanz pro Zustandsklasse
    - **Entkopplung**: Zustände geben neuen Zustand zurück

---

??? question "Selbstkontrolle"
    1. Warum ist das State-Pattern besser als if-else-Ketten für Zustände?
    2. Warum werden Zustände als Singleton implementiert?
    3. Was bedeutet es, wenn Zustandsmethoden den neuen Zustand zurückgeben?
    
    ??? success "Antworten"
        1. Jeder Zustand hat eine eigene Klasse – übersichtlicher, erweiterbar, keine Fallunterscheidungen
        2. Es wird nur eine Instanz pro Zustand benötigt – spart Speicher und GC-Overhead
        3. Der Context (Player) muss keine Referenz an die Zustände übergeben – stärkere Entkopplung
