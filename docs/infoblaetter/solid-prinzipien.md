# Die SOLID-Prinzipien

## Überblick

Die **SOLID-Prinzipien** sind fünf grundlegende Designprinzipien der objektorientierten Programmierung. Sie wurden von Robert C. Martin ("Uncle Bob") formuliert und helfen dabei, wartbare, erweiterbare und verständliche Software zu entwickeln.

| Buchstabe | Prinzip | Kurzbeschreibung |
|-----------|---------|------------------|
| **S** | Single Responsibility | Eine Klasse – eine Verantwortung |
| **O** | Open/Closed | Offen für Erweiterung, geschlossen für Änderung |
| **L** | Liskov Substitution | Subtypen verhalten sich wie Basistypen |
| **I** | Interface Segregation | Kleine, spezifische Interfaces |
| **D** | Dependency Inversion | Abhängigkeit von Abstraktionen |

---

## S – Single Responsibility Principle (SRP)

!!! success "Prinzip"
    **Eine Klasse sollte nur einen Grund haben, sich zu ändern.**

Eine Klasse ist für genau **eine Aufgabe** zuständig. Dieses Prinzip entspricht dem Konzept der **hohen Kohäsion**.

### Beispiel: Verletzung des SRP

```java
// Schlecht: Game macht zu viel
public class Game {
    public void play() { ... }           // Spielablauf
    public void createRooms() { ... }    // Welterstellung
    public void printHelp() { ... }      // Ausgabe
    public void goRoom() { ... }         // Befehlslogik
    public void takeItem() { ... }       // Befehlslogik
}
```

### Beispiel: SRP angewandt

```java
// Gut: Jede Klasse hat eine Verantwortung
public class Game { ... }           // Nur Spielablauf
public class Map { ... }            // Welterstellung
public class GoCommand { ... }      // go-Befehl
public class TakeCommand { ... }    // take-Befehl
```

!!! tip "In der Lernsituation"
    - **AB 04**: `Room` liefert seine eigene Beschreibung (Entwurf nach Zuständigkeiten)
    - **AB 05**: `CommandWords` verwaltet nur die gültigen Befehle
    - **AB 06**: `Item` modelliert nur Gegenstände, `Room` nur Räume
    - **AB 10**: Jeder Command ist eine eigene Klasse

---

## O – Open/Closed Principle (OCP)

!!! success "Prinzip"
    **Software-Einheiten sollten offen für Erweiterungen, aber geschlossen für Modifikationen sein.**

Neues Verhalten wird durch **Hinzufügen** von Code erreicht, nicht durch **Ändern** von bestehendem Code.

### Beispiel: Verletzung des OCP

```java
// Schlecht: Bei neuem Befehl muss processCommand geändert werden
public void processCommand(Command cmd) {
    if (cmd.equals("go")) { ... }
    else if (cmd.equals("look")) { ... }
    else if (cmd.equals("take")) { ... }
    // Neuer Befehl? → Code hier ändern!
}
```

### Beispiel: OCP angewandt (Command-Pattern)

```java
// Gut: Neue Befehle durch neue Klassen
public interface Command {
    void execute();
}

// Neuer Befehl? → Neue Klasse erstellen, registrieren, fertig!
public class FlyCommand implements Command { ... }
```

!!! tip "In der Lernsituation"
    - **AB 10**: Command-Pattern – neue Befehle ohne Änderung der if-else-Kette
    - **AB 11**: Factory-Pattern – neue Landschaften ohne Änderung von `Game`
    - **AB 09**: State-Pattern – neue Zustände ohne Änderung von `Player`

---

## L – Liskov Substitution Principle (LSP)

!!! success "Prinzip"
    **Objekte einer Subklasse müssen überall verwendbar sein, wo Objekte der Basisklasse erwartet werden.**

Subklassen dürfen das Verhalten der Basisklasse nicht auf unerwartete Weise ändern.

### Beispiel: LSP in Aktion

```java
// Überall wo Map erwartet wird, kann auch MayaMap stehen
Map map = new MayaMap();
map.createRooms();  // Funktioniert wie erwartet

Map map2 = new UniversityMap();
map2.createRooms(); // Funktioniert ebenfalls
```

### Warum ist das wichtig?

```java
// Game arbeitet nur mit Map – egal welche konkrete Implementierung
public class Game {
    private Map currentMap;
    
    public void startGame(Map map) {
        this.currentMap = map;
        // Egal ob MayaMap, UniversityMap oder PirateMap
        currentMap.createRooms();
        currentMap.setItems();
    }
}
```

!!! tip "In der Lernsituation"
    - **AB 11/12**: `MayaMap` und `UniversityMap` können `Map` ersetzen
    - **AB 09**: Alle State-Klassen können `PlayerState` ersetzen
    - **AB 10**: Alle Command-Klassen können `Command` ersetzen

---

## I – Interface Segregation Principle (ISP)

!!! success "Prinzip"
    **Clients sollten nicht von Interfaces abhängen, die sie nicht nutzen.**

Lieber mehrere kleine, spezifische Interfaces als ein großes "Fat Interface".

### Beispiel: Verletzung des ISP

```java
// Schlecht: Nicht alle Items können alles
public interface Item {
    void pickup();
    void drop();
    void eat();      // Kann man ein Schwert essen?
    void equip();    // Kann man Essen ausrüsten?
}
```

### Beispiel: ISP angewandt

```java
// Gut: Kleine, spezifische Interfaces
public interface Carryable {
    void pickup();
    void drop();
}

public interface Edible {
    void eat();
}

public interface Equippable {
    void equip();
}

// Klassen implementieren nur, was sie brauchen
public class Herb implements Carryable, Edible { ... }
public class Sword implements Carryable, Equippable { ... }
```

!!! tip "In der Lernsituation"
    - **AB 09**: `Herb` könnte ein `Edible`-Interface implementieren
    - **AB 10**: Das `Command`-Interface ist bewusst klein gehalten

---

## D – Dependency Inversion Principle (DIP)

!!! success "Prinzip"
    **High-Level-Module sollten nicht von Low-Level-Modulen abhängen. Beide sollten von Abstraktionen abhängen.**

Konkret: Programmiere gegen **Interfaces**, nicht gegen **konkrete Klassen**.

### Beispiel: Verletzung des DIP

```java
// Schlecht: Game hängt von konkreten Klassen ab
public class Game {
    private MayaMap map;           // Konkrete Klasse!
    private GoCommand goCmd;       // Konkrete Klasse!
}
```

### Beispiel: DIP angewandt

```java
// Gut: Game hängt nur von Abstraktionen ab
public class Game {
    private Map map;               // Interface/abstrakte Klasse
    private Command currentCmd;    // Interface
}
```

```kroki-plantuml
@startuml
skinparam style strictuml
skinparam classAttributeIconSize 0

package "High-Level" {
    class Game {
        -map: Map
        -commands: Map<String, Command>
    }
}

package "Abstraktionen" {
    abstract class Map <<abstract>>
    class Command <<interface>>
}

package "Low-Level" {
    class MayaMap
    class UniversityMap
    class GoCommand
    class TakeCommand
}

Game --> Map
Game --> Command
MayaMap --|> Map
UniversityMap --|> Map
GoCommand ..|> Command
TakeCommand ..|> Command
@enduml
```

!!! tip "In der Lernsituation"
    - **AB 10**: `Parser` kennt nur `Command`-Interface, nicht `GoCommand`
    - **AB 11/12**: `Game` kennt nur `Map`, nicht `MayaMap`
    - **Infoblatt Interfaces**: "Programmiere auf eine Schnittstelle!"

---

## Zusammenfassung

| Prinzip | Frage zur Selbstkontrolle |
|---------|---------------------------|
| **SRP** | Hat meine Klasse nur einen Grund, sich zu ändern? |
| **OCP** | Kann ich erweitern, ohne bestehenden Code zu ändern? |
| **LSP** | Kann ich jede Subklasse überall für die Basisklasse einsetzen? |
| **ISP** | Müssen Klassen Methoden implementieren, die sie nicht brauchen? |
| **DIP** | Hängt mein Code von Interfaces oder von konkreten Klassen ab? |

!!! warning "Pragmatismus"
    Die SOLID-Prinzipien sind **Richtlinien**, keine absoluten Regeln. Bei kleinen Projekten kann strikte Einhaltung zu Over-Engineering führen. Wende sie an, wo sie echten Nutzen bringen!

---

## Weiterführende Links

- [Infoblatt Interfaces](interfaces.md) – Programmieren gegen Schnittstellen
- [Infoblatt Abstrakte Klassen](abstrakte-klassen.md) – Gemeinsame Basisklassen
- [Infoblatt Polymorphismus](polymorphismus.md) – Unterschiedliches Verhalten durch Vererbung
