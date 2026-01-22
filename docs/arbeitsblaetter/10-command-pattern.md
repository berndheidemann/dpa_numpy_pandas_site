# Die Welt von Zuul – Command-Pattern

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- Das Command-Pattern erklären und anwenden
- Methodenaufrufe als Objekte kapseln
- Undo-Funktionalität mit einem Stack implementieren
- Die Rollen im Command-Pattern identifizieren

---

## Einkapseln von Methodenaufrufen im Command-Pattern

Die Klasse `Game` weist an einigen Stellen **Code-Smell** auf:

### Problem 1: Niedrige Kohäsion (Verletzung des SRP)

Die Klasse ist jetzt schon sehr groß und unübersichtlich geworden. Das liegt an niedriger Kohäsion. Sie ist nicht nur für:

- den Aufbau der Welt von Zuul
- die Spielsteuerung

zuständig, sondern enthält auch die **Logik der einzelnen Befehle**, die der Benutzer in dem Spiel ausführen kann.

!!! warning "SOLID: Single Responsibility Principle verletzt"
    Die Klasse `Game` hat **mehrere Gründe sich zu ändern**: Änderungen am Spielablauf, an der Welterstellung oder an einzelnen Befehlen. Das widerspricht dem [Single Responsibility Principle](../infoblaetter/solid-prinzipien.md#s-single-responsibility-principle-srp).

### Problem 2: Umfangreiche Fallunterscheidung (Verletzung des OCP)

Die Methode `processCommand(Command command)` enthält eine umfangreiche Fallunterscheidung in Form einer if-else-Leiter:

```java
private boolean processCommand(Command command) {
    String commandWord = command.getCommandWord();
    if (commandWord.equals("help")) {
        printHelp();
    }
    else if (commandWord.equals("go")) {
        goRoom(command);
    }
    else if (commandWord.equals("quit")) {
        wantToQuit = quit(command);
    }
    else if(commandWord.equals("look")){
        look();
    }
    else if(commandWord.equals("take")) {
        takeItem(command);
    }
    else if(commandWord.equals("drop")) {
        dropItem(command);
    }
    else if(commandWord.equals("eat")) {
        eat(command);
    }
    return wantToQuit;
}
```

!!! warning "Problem"
    Bei Erweiterung des Spiels um einen weiteren Befehl muss jeweils wieder ein neuer Fall und eine neue Hilfsmethode implementiert werden.

!!! danger "SOLID: Open/Closed Principle verletzt"
    Um einen neuen Befehl hinzuzufügen, muss **bestehender Code geändert** werden (die if-else-Kette). Das widerspricht dem [Open/Closed Principle](../infoblaetter/solid-prinzipien.md#o-openclosed-principle-ocp): Software sollte offen für Erweiterungen, aber geschlossen für Modifikationen sein.

In einem Refactoring soll dieser Code-Smell durch die Einführung des **Command-Patterns** ausgemerzt werden.

## Aufgaben

### Aufgabe 1 – Command-Pattern verstehen

- [ ] Lies das [Infoblatt Command-Pattern](../infoblaetter/command-pattern.md) und mache dich mit dem Muster vertraut

### Aufgabe 2 – Rollen identifizieren

- [ ] Identifiziere, welche Klasse in unserer Anwendung „Die Welt von Zuul" die jeweilige Rolle übernimmt:

| Rolle | Klasse in unserer Anwendung |
|-------|----------------------------|
| Invoker | |
| Command | |
| ConcreteCommand | |
| Receiver | |
| Client | |

### Aufgabe 3 – Klassendiagramm zeichnen

- [ ] Zeichne anhand der Informationen aus Aufgabe 2 ein Klassendiagramm. Aus ihm sollen neu benötigte Attribute und Methoden hervorgehen!

### Aufgabe 4 – Implementierung

- [ ] Implementiere das entwickelte und im Plenum besprochene Klassendiagramm

## Erweiterungen: Der Back-Befehl

Unsere Anwendung soll um einen Befehl `back` erweitert werden, der zuvor getätigte Befehle rückgängig macht. Dank des Command-Patterns ist es relativ einfach, Rückgängig-Befehle zu implementieren.

### Aufgabe 5 – Stack kennenlernen

- [ ] Lies das [Infoblatt LinkedList und Stack](../infoblaetter/linkedlist-stack.md) und mache dich mit dem Stack-Prinzip vertraut

- [ ] Erkundige dich über die Java-Klasse `Stack` und inwiefern man die Klasse `LinkedList` als Stack verwenden kann

!!! info "Stack"
    Da wir nicht nur einen Befehl rückgängig machen wollen, sondern alle, die der Spieler bis dato ausgeführt hat, brauchen wir eine Datenstruktur, die die ausgeführten Befehle in **umgekehrter Reihenfolge** speichert.

### Aufgabe 6 – Back-Befehl implementieren

- [ ] Erweitere das Interface `Command` um die Methode `back()`

- [ ] Implementiere `back()` in den einzelnen Befehlsklassen (nutze dafür Stacks innerhalb der Befehlsklassen)

- [ ] Speichere innerhalb des Parsers die letzten Befehle in einem Stack

- [ ] Passe die Klasse `Parser` geeignet an und fange den Fall ab, dass der Benutzer `back` ausführt, ohne dass er bereits einen Befehl ausgeführt hat

- [ ] Teste die Erweiterung

### Aufgabe 7 – Versionierung

- [ ] Versioniere deinen aktuellen Projektstand:
    ```bash
    git status
    git add -A
    git commit -m "zuul 10"
    ```

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **Command-Pattern**: Methodenaufrufe als Objekte kapseln
    - **Invoker, Command, Receiver**: Die Rollen im Pattern
    - **Stack (LIFO)**: Für Undo-Funktionalität geeignete Datenstruktur
    - **Erweiterbarkeit**: Neue Befehle ohne Änderung bestehenden Codes

!!! info "SOLID-Prinzipien erfüllt"
    Das Command-Pattern erfüllt gleich drei [SOLID-Prinzipien](../infoblaetter/solid-prinzipien.md):
    
    - **SRP**: Jeder Befehl ist eine eigene Klasse mit einer Verantwortung
    - **OCP**: Neue Befehle durch neue Klassen, ohne `processCommand()` zu ändern
    - **DIP**: Der Parser hängt nur vom `Command`-Interface ab, nicht von konkreten Befehlsklassen

---

??? question "Selbstkontrolle"
    1. Was ist der Unterschied zwischen Invoker und Receiver?
    2. Warum ist ein Stack für Undo geeignet?
    3. Welchen Vorteil bietet das Command-Pattern bei neuen Befehlen?
    
    ??? success "Antworten"
        1. Invoker löst Befehle aus (z.B. Parser), Receiver führt die eigentliche Aktion aus (z.B. Game)
        2. LIFO-Prinzip – der zuletzt ausgeführte Befehl wird zuerst rückgängig gemacht
        3. Kein Ändern der if-else-Kette – einfach neuen Command registrieren
