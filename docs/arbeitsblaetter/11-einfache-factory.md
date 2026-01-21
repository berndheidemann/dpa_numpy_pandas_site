# Die Welt von Zuul – Einfache Factory

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- Das Factory-Pattern (Simple Factory) erklären und anwenden
- Abstrakte Klassen verwenden
- Objekterstellung von der Verwendung trennen
- Das Open/Closed-Prinzip anwenden

---

## Erstellung verschiedener Landschaften durch eine Factory

Unser Adventuregame soll dem Benutzer die Möglichkeit geben, in **verschiedenen Welten** zu spielen. Bislang ist es lediglich möglich, sich in dem Urwald einer Mayawelt zu bewegen.

### Anforderungen

Das Spiel ist so zu erweitern, dass der Benutzer zu Beginn des Spiels über die Konsole auswählen kann, ob er sich in:

- der schon bekannten **Mayawelt**
- auf einem **Universitätsgelände**
- oder einer **Piratenwelt**

bewegen will. In jeder Spielwelt existieren dieselben Gegenstände, die in den verschiedenen Räumen platziert werden. Die Gegenstände sollen in der neuen Version **zufällig** in den Räumen verteilt werden.

## Analyse des bestehenden Codes

Beim Studium der Methode `createRooms()` innerhalb der Klasse `Game` fallen mehrere Probleme auf:

### Problem 1: Geringe Kohäsion

Die Klasse `Game` ist verantwortlich für:

1. Den Ablauf des Spiels
2. Die Instanziierung der Räume
3. Das Zusammenfügen zu einer Landschaft
4. Die weitere Verarbeitung (Verteilung der Gegenstände)

### Problem 2: Fehlende Wiederverwendbarkeit

Was ist, wenn andere Klassen andere Landschaften erstellen, aber die Verteilung der Gegenstände wiederverwenden wollen?

### Problem 3: Verletzung des Open/Closed-Prinzips

!!! info "Open/Closed-Prinzip"
    Entwürfe sollten für **Erweiterungen offen**, aber für **Veränderungen geschlossen** sein.

Für die Einführung der geforderten zusätzlichen Landschaften müsste der bestehende Code der Klasse `Game` geändert werden.

## Lösung: Factory-Pattern

!!! tip "OO-Prinzip"
    **Identifiziere jene Aspekte, die sich ändern, und trenne sie von jenen, die konstant bleiben.**

- **Veränderlich**: Instanziierung von verschiedenen Landschaften
- **Konstant**: Jede Landschaft wird mit Gegenständen ausgestattet

### Klassenstruktur

Die **MapFactory** ist für die Instanziierung der gewünschten Landschaft zuständig. Das passiert in der Methode `Map createMap(String type)`.

Eine Landschaft wird durch die abstrakte Klasse **Map** repräsentiert:

!!! tip "Abstrakte Klassen"
    Lies das [Infoblatt Abstrakte Klassen](../infoblaetter/abstrakte-klassen.md), um das Konzept zu verstehen.

- Speichert die verschiedenen Räume in einer `HashMap`
- Besitzt einen Namen
- Bietet Schnittstellen:
    - `Room getRandomRoom()` – einen zufälligen Raum erhalten
    - `void setItems()` – Gegenstände zufällig in den Räumen verteilen
- Zwingt konkrete Landschaften, folgende abstrakte Methoden zu implementieren:
    - `void createRooms()`
    - `void setExits()`
    - `Room getBeginningRoom()`

Der **MapManager** ist für den konstanten Teil der Anwendung zuständig – die Verteilung der Gegenstände. In der Methode `Map getMap(String type)` wird die MapFactory damit beauftragt, den gewünschten Landschaftstypen bereitzustellen und die Gegenstände in den Räumen zu platzieren.

## Aufgaben

### Aufgabe 1 – Factory-Pattern implementieren

- [ ] Führe ein Refactoring der Anwendung durch, indem du den Entwurf zum Factory-Pattern implementierst
- [ ] Teste deine Anwendung

### Aufgabe 2 – Universitätslandschaft hinzufügen

- [ ] Erweitere deine Anwendung um eine Universitätslandschaft

### Aufgabe 3 – Versionierung

- [ ] Versioniere deinen aktuellen Projektstand:
    ```bash
    git status
    git add -A
    git commit -m "zuul 11"
    ```

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **Simple Factory**: Zentralisiert die Objekterstellung
    - **Abstrakte Klassen**: Definieren gemeinsame Struktur für Subklassen
    - **Open/Closed-Prinzip**: Offen für Erweiterungen, geschlossen für Änderungen
    - **Trennung**: Veränderliches (Erstellung) von Konstantem (Verarbeitung) trennen

---

??? question "Selbstkontrolle"
    1. Was ist der Vorteil einer Factory gegenüber direkter Objekterstellung?
    2. Warum ist `Map` eine abstrakte Klasse?
    3. Was bedeutet das Open/Closed-Prinzip?
    
    ??? success "Antworten"
        1. Die Erstellungslogik ist zentralisiert; neue Typen ohne Änderung des Client-Codes
        2. Sie definiert gemeinsame Struktur, aber jede Landschaft implementiert Details selbst
        3. Code ist offen für Erweiterungen (neue Landschaften), aber geschlossen für Änderungen (bestehender Code bleibt)

Die dafür nötigen Codeschnipsel:

#### Räume der Universität

```java
this.rooms.put("outside", new Room("outside the main entrance of the university"));
this.rooms.put("theater", new Room("in a lecture theater"));
this.rooms.put("pub", new Room("in the campus pub"));
this.rooms.put("lab", new Room("in a computing lab"));
this.rooms.put("office", new Room("in the computing admin office"));
```

!!! tip "Diamond Operator"
    Nutze den Diamond Operator `<>` bei der Initialisierung von Collections:
    ```java
    HashMap<String, Room> rooms = new HashMap<>();  // statt new HashMap<String, Room>()
    ```

#### Ausgänge

```java
this.rooms.get("outside").setExit("east", rooms.get("theater"));
this.rooms.get("outside").setExit("south", rooms.get("lab"));
this.rooms.get("outside").setExit("west", rooms.get("pub"));
this.rooms.get("theater").setExit("west", rooms.get("outside"));
this.rooms.get("pub").setExit("east", rooms.get("outside"));
this.rooms.get("lab").setExit("north", rooms.get("outside"));
this.rooms.get("lab").setExit("east", rooms.get("office"));
this.rooms.get("office").setExit("west", rooms.get("lab"));
```
