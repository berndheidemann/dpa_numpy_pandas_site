# Die Welt von Zuul – Kapselung führt zu loser Kopplung

## Das Problem

Im letzten Arbeitsblatt waren ja eine ganze Menge if-Anweisungen nötig, um die verschiedenen Richtungen umzusetzen.

Stell dir vor, du wolltest die Anwendung um weitere Richtungen wie `nordwest` oder `südost` erweitern. Jedes Mal wäre in den Methoden `setExits()` und `getExit()` der Klasse `Room` eine weitere Fallunterscheidung nötig.

!!! warning "Schlechte Entwurfsentscheidung"
    Das verweist auf eine schlechte Entwurfsentscheidung bei der Implementierung der Richtungen.

Um diesem Problem beizukommen, beschließt du, deinen Entwurf noch einmal zu verändern.

## Aufgaben

### Aufgabe 1 – HashMap kennenlernen

- [ ] Erkundige dich mithilfe des [Infoblatts HashMap](../infoblaetter/hashmap.md) über die Funktionsweise von Maps in Java

### Aufgabe 2 – Refactoring der Klasse Room

- [ ] Ersetze die Attribute, die in der Klasse `Room` Ausgänge repräsentieren, durch eine `HashMap`

- [ ] Erzeuge im Konstruktor der Klasse `Room` ein entsprechendes HashMap-Objekt

- [ ] Ändere die Methode `getExit()` entsprechend ab

- [ ] Ändere die Methode `exitsToString()` entsprechend ab

### Aufgabe 3 – Neue Methode setExit()

Die Methode `setExits()` funktioniert nun ebenfalls nicht mehr. Hier ist noch Wissen über die sechs festen Ausgänge verdrahtet.

!!! danger "Problem"
    Das führt dazu, dass wir bei jeder zusätzlichen Richtung, um die unser Programm erweitert wird, der Methode `setExits()` einen weiteren Parameter hinzufügen müssen. Das verändert also jedes Mal die Signatur unserer Schnittstelle nach außen!

- [ ] Ersetze die Methode `setExits()` durch die neue Methode:
    ```java
    public void setExit(String direction, Room neighbour)
    ```

!!! note "Hinweis"
    Das führt dazu, dass **jede beliebige Richtung** – nicht nur `up` und `down`, sondern z.B. `southwest` hinzugefügt werden kann, ohne irgendeine weitere Änderung vornehmen zu müssen.

### Aufgabe 4 – Anpassung der Klasse Game

- [ ] Ersetze den Methodenaufruf von `setExits()` in `Game` durch entsprechende Aufrufe von `setExit()`

- [ ] Teste deine Anwendung

### Aufgabe 5 – Versionierung

- [ ] Versioniere deinen aktuellen Projektstand:
    ```bash
    git status
    git add -A
    git commit -m "zuul 3"
    ```
