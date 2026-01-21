# Die Welt von Zuul – Refactoring

## Einführung: Klasse Player

Es soll den Spielern die Möglichkeit geboten werden, Gegenstände aufzunehmen und abzulegen. Sinnvoll wäre es, eine Klasse `Player` zu erstellen, die diese Information hält.

Das Datenfeld `currentRoom` der Klasse `Game` ist ebenfalls eine Eigenschaft des Spielers, denn der Spieler befindet sich ja im Raum und nicht das „Spiel".

!!! info "Refactoring"
    Wir befreien die Klasse `Game` von dieser Aufgabe, indem wir diese Informationen in die Klasse `Player` auslagern. Diese Auslagerung ist ein **Refactoring**, weil wir die Datenstruktur so anpassen, dass der Gesamtentwurf den neuen Anforderungen des Aufnehmens und Ablegens von Gegenständen besser entspricht.

## Aufgabe 1 – Stufe 1 des Refactorings

- [ ] Implementiere eine Klasse `Player` mit der Instanzvariablen `currentRoom`

- [ ] Ergänze einen Getter für `currentRoom` und die Methode `public void goTo(Room newRoom)`

- [ ] Entferne in der Klasse `Game` das Attribut `currentRoom` und füge stattdessen ein Attribut hinzu, welches das Spielerobjekt hält

- [ ] Instanziiere im Konstruktor der Klasse `Game` ein Spielerobjekt

- [ ] Weise dem Spieler in der Methode `createRooms()` den Raum `marketsquare` zu

- [ ] Behebe alle Fehler, die durch das fehlende Attribut `currentRoom` entstanden sind, indem du die Möglichkeiten der Klasse `Player` nutzt

## Aufgabe 2 – Stufe 2 des Refactorings

- [ ] Teste deine Anwendung

## Aufgabe 3 – Stufe 3 des Refactorings

### Tragkraft und Gegenstände

- [ ] Erweitere die Klasse `Player` um eine Eigenschaft `loadCapacity`. Jeder Spieler startet mit einer Tragkraft von **10 kg**

- [ ] Erweitere die Klasse `Player` so, dass er beliebig viele Gegenstände aufnehmen und besitzen kann

!!! warning "LinkedList statt ArrayList"
    Es ist keine gute Idee, dafür die Klasse `ArrayList` zu verwenden. Diese Liste verwaltet ihre Objekte intern in einem Array. Das hat zur Folge, dass beim Entfernen von Objekten die nachfolgenden Objekte im Array nach vorne rücken müssen, was intern Rechenkapazität bindet.
    
    Erkundige dich über die Funktionsweise der `LinkedList` in Java sowie ihren Vor- bzw. Nachteilen gegenüber einer `ArrayList`. **Verwende eine `LinkedList` für die Gegenstände des Spielers.**

### Methode takeItem()

- [ ] Implementiere in der Klasse `Player` eine Methode `public boolean takeItem(Item item)`, die:
    - Das gesamte Gewicht der aufgenommenen Gegenstände ermittelt
    - Prüft, ob der Gegenstand aufgenommen werden kann
    - `true` oder `false` zurückgibt, je nachdem ob der Gegenstand aufgenommen werden konnte

!!! note "Hinweis"
    Dafür benötigst du einen Getter für das Attribut `weight` in der Klasse `Item`!

### Kohäsion verbessern

- [ ] Lagere über die Tastenkombination ++alt+shift+m++ folgende Hilfsmethoden aus:
    - `private double calculateWeight()` – Ermittlung des aufgenommenen Gewichts
    - `private boolean isTakePossible(Item item)` – Prüfung, ob der Gegenstand aufgenommen werden kann

### Methode dropItem()

- [ ] Implementiere eine Methode `public Item dropItem(String name)`, die den Gegenstand vom Spieler entfernt und zurückgibt. Falls der Spieler keinen Gegenstand mit dem übergebenen Namen besitzt, gibt die Methode `null` zurück

### Methode showStatus()

- [ ] Implementiere eine Methode `public String showStatus()`, die die Tragkraft des Spielers und alle aufgenommenen Gegenstände als String zurückgibt:
    ```
    > Status of the player
    loadCapacity: 10 kg
    taken items: sword, 5 kg
    absorbed weight: 5 kg
    ```

### Befehle take und drop

- [ ] Füge der Klasse `CommandWords` die Befehle `take` und `drop` hinzu

- [ ] Implementiere in der Klasse `Room` eine Methode `Item removeItem(String name)`, die den Gegenstand aus dem Raum entfernt und zurückgibt

- [ ] Ergänze in der Klasse `Game` die private Methode `takeItem(Command command)`

- [ ] Ergänze in der Klasse `Game` die private Methode `dropItem(Command command)`

- [ ] Rufe in der Methode `processCommand()` die Methoden `takeItem()` und `dropItem()` geeignet auf

- [ ] Lasse nach jeder Ausführung von `take` und `drop` den Spielerstatus und die Raumbeschreibung ausgeben

## Extraaufgabe

- [ ] Lege in einem Raum einen **magischen Muffin** ab
- [ ] Füge den Befehl `eat` hinzu
- [ ] Wenn ein Spieler den Muffin isst, erhöht sich seine maximale Tragkraft
- [ ] Lasse den neuen Status des Spielers nach Ausführung von `eat` anzeigen

!!! tip "Tipp"
    Es könnten weitere Dinge hinzukommen. Hier gibt es gute und schlechte Entwürfe – denke geeignet nach!

## Versionierung

- [ ] Versioniere deinen aktuellen Projektstand:
    ```bash
    git status
    git add -A
    git commit -m "zuul 7"
    ```
