# Die Welt von Zuul – Command-Pattern

## Einkapseln von Methodenaufrufen im Command-Pattern

Die Klasse `Game` weist an einigen Stellen **Code-Smell** auf:

### Problem 1: Niedrige Kohäsion

Die Klasse ist jetzt schon sehr groß und unübersichtlich geworden. Das liegt an niedriger Kohäsion. Sie ist nicht nur für:

- den Aufbau der Welt von Zuul
- die Spielsteuerung

zuständig, sondern enthält auch die **Logik der einzelnen Befehle**, die der Benutzer in dem Spiel ausführen kann.

### Problem 2: Umfangreiche Fallunterscheidung

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

In einem Refactoring soll dieser Code-Smell durch die Einführung des **Command-Patterns** ausgemerzt werden.

## Aufgaben

### Aufgabe 1 – Command-Pattern verstehen

- [ ] Erkundige dich anhand des Informationsblattes „IB Command-Pattern aus Entwurfsmuster von Kopf bis Fuß.pdf" auf der itslearning-Plattform über das Command-Pattern (Seiten 191 – 215)

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

- [ ] Informiere dich anhand des Informationsblattes auf den **Seiten 216-223** über den Befehl Rückgängig im Command-Pattern

- [ ] Erkundige dich über die Java-Klasse `Stack` und inwiefern man die Klasse `LinkedList` als Stack verwenden kann

!!! info "Stack"
    Da wir nicht nur einen Befehl rückgängig machen wollen, sondern alle, die der Spieler bis dato ausgeführt hat, brauchen wir eine Datenstruktur, die die ausgeführten Befehle in **umgekehrter Reihenfolge** speichert.

### Aufgabe 6 – Back-Befehl implementieren

- [ ] Erweitere das Interface `Command` um die Methode `back()`

- [ ] Implementiere `back()` in den einzelnen Befehlsklassen (nutze dafür Stacks innerhalb der Befehlsklassen)

- [ ] Speichere innerhalb des Parsers die letzten Befehle in einem Stack

- [ ] Passe die Klasse `Parser` geeignet an und fange den Fall ab, dass der Benutzer `back` ausführt, ohne dass er bereits einen Befehl ausgeführt hat

- [ ] Teste die Erweiterung
