# Die Welt von Zuul – Kohäsionsverletzung durch Codeduplizierung

## Einführung

Schlechte Klassenentwürfe werden meistens dann offensichtlich, wenn eine Anwendung angepasst oder erweitert werden soll. Unser Programm kann bisher nicht sonderlich viel und ist nicht sonderlich spannend. Wir wollen es daher Schritt für Schritt erweitern.

Während der Erweiterung gehen wir auf einige Aspekte des Klassenentwurfs ein und werden feststellen, dass diese Anwendung einige schlechte Entwurfsentscheidungen enthält, die wir beheben.

!!! warning "Wichtig"
    Einen schlechten Entwurf erkennt man nicht unbedingt daran, dass eine Anwendung nicht funktioniert. Diese Anwendung tut, was sie soll. Ein schlechter Entwurf ist niemals dadurch zu entschuldigen, dass das umzusetzende Problem komplex ist.

Ein schlechter Entwurf hängt von den Entscheidungen ab, die wir bei der Lösung des Problems treffen. Um Anhaltspunkte dafür zu haben, was schlechte Entwurfsentscheidungen sind, benötigen wir die Begriffe **Kopplung** und **Kohäsion**.

## Kopplung

Der Begriff **Kopplung** beschreibt den Grad der Abhängigkeit zwischen Klassen. Es wird eine möglichst **lose Kopplung** angestrebt, also ein System, in dem jede Klasse weitgehend unabhängig ist und mit anderen Klassen nur über möglichst schmale, wohl definierte Schnittstellen kommuniziert.

Der Begriff bezieht sich also darauf, wie Klassen miteinander verknüpft sind. Je loser die Kopplung von Klassen ist, desto leichter kann eine Anwendung geändert werden. In einer eng gekoppelten Klassenstruktur zieht eine Änderung in einer Klasse viele Änderungen in anderen Klassen nach sich. Das soll vermieden werden.

!!! danger "Zeichen für schlechten Entwurf"
    Zieht sich eine Änderung durch eine ganze Anwendung, ist das ein Zeichen für einen schlechten Entwurf!

## Kohäsion

Der Begriff **Kohäsion** beschreibt, wie gut eine Programmeinheit eine logische Aufgabe oder Einheit abbildet. In einem System mit hoher Kohäsion ist jede Programmeinheit (eine Methode, Klasse oder ein Modul) für genau **eine** wohl definierte Aufgabe oder Einheit verantwortlich.

- Eine **Methode** sollte daher immer nur eine logische Operation implementieren.
- Eine **Klasse** sollte genau einen Typ von Objekt modellieren.

Hauptanlass für Kohäsion ist die **Wiederverwendung**. Ein hoher Grad an Kohäsion erhöht die Wahrscheinlichkeit, dass eine Klasse oder Methode in einem anderen Zusammenhang eingesetzt werden kann.

!!! tip "DRY-Prinzip"
    Verstößt man gegen das **DRY-Prinzip** (Don't repeat yourself), liegt häufig eine schlechte Kohäsion vor. **Code-Duplizierung** ist dafür ein Beispiel. Von Code-Duplizierung spricht man dann, wenn ein Quelltextabschnitt mehr als einmal in einer Anwendung erscheint.

## Aufgaben

### Aufgabe 1 – Code-Duplizierung identifizieren

Schaue dir die Methoden `private void goRoom(Command command)` und `private void printWelcome()` der Klasse `Game` an. Dort gibt es eine Code-Duplizierung:

```java
System.out.println();
System.out.println("You are " + currentRoom.getDescription());
System.out.print("Exits: ");
if(currentRoom.northExit != null) {
    System.out.print("north ");
}
if(currentRoom.eastExit != null) {
    System.out.print("east ");
}
if(currentRoom.southExit != null) {
    System.out.print("south ");
}
if(currentRoom.westExit != null) {
    System.out.print("west ");
}
System.out.println();
```

Diese Code-Duplizierung liegt daran, dass beide Methoden zwei Dinge erledigen:

- `printWelcome()` gibt einen Willkommenstext aus **und** liefert Informationen über den aktuellen Raum.
- `goRoom()` wechselt den Raum **und** gibt ebenfalls Informationen über diesen aus.

- [ ] Identifiziere die Code-Duplizierung in den Methoden `goRoom()` und `printWelcome()`

### Aufgabe 2 – Refactoring durchführen

- [ ] Schreibe eine Methode `printRoomInformation()`, die in den Methoden `goRoom()` und `printWelcome()` aufgerufen wird und darüber die Codeduplizierung und die schlechte Kohäsion auflöst

!!! tip "IntelliJ-Shortcut"
    Benutze für das Auslagern der Methode die Tastenkombination ++ctrl+alt+m++, nachdem du den auszulagernden Quellcode markiert hast.

### Aufgabe 3 – Erweiterung um neue Richtungen

Wir wollen im nächsten Schritt das Spiel um die Bewegungsrichtungen `up` und `down` erweitern. Dementsprechend brauchen wir ein paar neue Räume:

- Die Tempelpyramide hat nun einen **ersten Stock**, in dem der Zauberer untergebracht ist.
- Außerdem hat die Tempelpyramide einen **Keller**, von dem ein Geheimgang zu einer alten **Piratenhöhle** unter der Opferstätte führt.

- [ ] Erweitere das Programm um die neuen Räume und Richtungen `up` und `down`
- [ ] Teste dein Programm

!!! note "Hinweis"
    Mache dir vor deiner Erweiterung klar, an welchen Stellen im Programm du Änderungen durchführen musst.

### Aufgabe 4 – Versionierung mit Git

- [ ] Versioniere die Änderungen mit Git:
    ```bash
    git status
    git add dateiname_mit_dateiendung
    git commit -m "zuul 1"
    ```

### Aufgabe 5 – Versionshistorie anzeigen

- [ ] Lasse dir die Versionshistorie anzeigen:
    ```bash
    git log
    git log --oneline
    ```

### Aufgabe 6 – Zwischen Versionen wechseln

- [ ] Wechsle zu einer älteren Version:
    ```bash
    git checkout ID_des_Commits
    ```

- [ ] Kehre zur aktuellen Version zurück:
    ```bash
    git checkout master
    ```
