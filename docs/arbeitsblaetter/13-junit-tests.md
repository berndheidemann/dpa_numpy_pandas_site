# Die Welt von Zuul – Unit-Tests mit JUnit

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- [ ] Erklären, warum automatisierte Tests wichtig sind
- [ ] JUnit 5 in einem Java-Projekt einrichten
- [ ] Testklassen und Testmethoden schreiben
- [ ] Assertions verwenden, um erwartete Ergebnisse zu prüfen
- [ ] Tests für Exceptions schreiben

---

## Motivation

Bisher haben wir unsere Anwendung manuell getestet: Programm starten, Befehle eingeben, Ausgabe prüfen. Das ist:

- **Zeitaufwändig**: Bei jeder Änderung alles erneut testen
- **Fehleranfällig**: Testfälle werden vergessen
- **Nicht reproduzierbar**: Verschiedene Tester, verschiedene Ergebnisse

!!! success "Lösung: Automatisierte Tests"
    Mit **Unit-Tests** können wir einzelne Klassen und Methoden automatisch testen. Die Tests laufen in Sekunden und zeigen sofort, ob etwas kaputt ist.

## Aufgaben

### Aufgabe 1 – JUnit verstehen

- [ ] Lies das [Infoblatt JUnit](../infoblaetter/junit.md) und mache dich mit den Grundkonzepten vertraut

!!! tip "Wichtige Konzepte"
    - `@Test` – Markiert eine Testmethode
    - `@BeforeEach` – Setup vor jedem Test
    - Assertions: `assertEquals`, `assertTrue`, `assertNull`, `assertThrows`

### Aufgabe 2 – JUnit einrichten

- [ ] Füge JUnit 5 zu deinem Projekt hinzu:

**IntelliJ (ohne Build-Tool):**

1. Rechtsklick auf Projekt → **Add Framework Support** → **JUnit 5**
2. Oder: ++alt+enter++ auf einer `@Test`-Annotation → **Add JUnit 5 to classpath**

**Mit Maven (pom.xml):**

```xml
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.10.0</version>
    <scope>test</scope>
</dependency>
```

- [ ] Erstelle im Projektverzeichnis einen Ordner `test` (parallel zu `src`)
- [ ] Markiere den Ordner als **Test Sources Root** (Rechtsklick → Mark Directory as → Test Sources Root)

### Aufgabe 3 – Item testen

- [ ] Erstelle eine Testklasse `ItemTest` im `test`-Verzeichnis

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class ItemTest {
    
    @Test
    void testItemHasCorrectName() {
        // Arrange
        Item item = new Item("Schwert", "ein scharfes Schwert", 5);
        
        // Assert
        assertEquals("Schwert", item.getName());
    }
    
    @Test
    void testItemHasCorrectWeight() {
        // Arrange
        Item item = new Item("Schild", "ein stabiler Schild", 10);
        
        // Assert
        assertEquals(10, item.getWeight());
    }
}
```

- [ ] Führe die Tests aus (Rechtsklick auf Testklasse → **Run 'ItemTest'**)
- [ ] Ergänze weitere Tests für die `toString()`-Methode

### Aufgabe 4 – Room testen

- [ ] Erstelle eine Testklasse `RoomTest`

```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.DisplayName;
import static org.junit.jupiter.api.Assertions.*;

class RoomTest {
    private Room room;
    private Room northRoom;
    
    @BeforeEach
    void setUp() {
        room = new Room("in einem Testraum");
        northRoom = new Room("im nördlichen Raum");
    }
    
    @Test
    @DisplayName("Ausgang setzen und abrufen")
    void testSetAndGetExit() {
        room.setExit("north", northRoom);
        
        assertEquals(northRoom, room.getExit("north"));
    }
    
    @Test
    @DisplayName("Nicht existierender Ausgang gibt null zurück")
    void testNonExistentExitReturnsNull() {
        assertNull(room.getExit("up"));
    }
}
```

- [ ] Ergänze Tests für `putItem()` und `removeItem()`
- [ ] Teste, dass `removeItem()` eine `ItemNotFoundException` wirft, wenn der Gegenstand nicht existiert:

```java
@Test
@DisplayName("removeItem wirft Exception bei unbekanntem Item")
void testRemoveNonExistentItemThrowsException() {
    assertThrows(ItemNotFoundException.class, () -> {
        room.removeItem("Diamant");
    });
}
```

### Aufgabe 5 – Player testen

- [ ] Erstelle eine Testklasse `PlayerTest`

```java
class PlayerTest {
    private Player player;
    private Room startRoom;
    
    @BeforeEach
    void setUp() {
        player = new Player("Held", 50); // Name, Tragkraft
        startRoom = new Room("Startpunkt");
        player.goTo(startRoom);
    }
    
    @Test
    void testPlayerStartsWithEmptyInventory() {
        assertTrue(player.getItems().isEmpty());
    }
    
    @Test
    void testPlayerCanTakeItem() throws ItemTooHeavyException {
        Item sword = new Item("Schwert", "scharf", 5);
        
        player.takeItem(sword);
        
        assertTrue(player.hasItem("Schwert"));
    }
    
    @Test
    void testPlayerCannotTakeTooHeavyItem() {
        Item boulder = new Item("Felsbrocken", "riesig", 100);
        
        assertThrows(ItemTooHeavyException.class, () -> {
            player.takeItem(boulder);
        });
    }
}
```

- [ ] Ergänze Tests für:
    - `dropItem()` – Gegenstand wird entfernt
    - `dropItem()` mit nicht vorhandenem Item – Exception
    - `goTo()` – Spieler wechselt den Raum
    - `calculateWeight()` – Gewichtsberechnung

### Aufgabe 6 – State testen

Falls du das State-Pattern implementiert hast:

- [ ] Erstelle eine Testklasse `PlayerStateTest`

```java
class PlayerStateTest {
    private Player player;
    
    @BeforeEach
    void setUp() {
        player = new Player("Held", 50);
    }
    
    @Test
    void testPlayerStartsHealthy() {
        assertEquals("gesund", player.getStateDescription());
    }
    
    @Test
    void testLightHitMakesPlayerWounded() {
        player.lightHit();
        
        assertEquals("verwundet", player.getStateDescription());
    }
    
    @Test
    void testHealingWoundedPlayerMakesHealthy() {
        player.lightHit();  // verwundet
        player.heal();      // gesund
        
        assertEquals("gesund", player.getStateDescription());
    }
    
    @Test
    void testImmobilePlayerCannotMove() {
        player.heavyHit();  // bewegungsunfähig
        
        assertFalse(player.canMove());
    }
}
```

### Aufgabe 7 – Testabdeckung prüfen

- [ ] Führe die Tests mit **Coverage** aus:
    - Rechtsklick auf `test`-Ordner → **Run 'All Tests' with Coverage**
    - Oder: ++ctrl+shift+f10++ (Windows/Linux) / ++cmd+shift+r++ (macOS)

- [ ] Analysiere die Ergebnisse:
    - Welche Klassen haben gute Abdeckung (>80%)?
    - Welche Methoden sind nicht getestet?

!!! info "Coverage-Farben"
    - **Grün**: Zeile wurde durch Tests ausgeführt
    - **Rot**: Zeile wurde nicht getestet
    - **Gelb**: Nur teilweise getestet (z.B. nur ein Branch einer if-Anweisung)

### Aufgabe 8 – Weitere Tests schreiben

- [ ] Schreibe Tests für mindestens **eine weitere Klasse** deiner Wahl:
    - `CommandWords` – Test der Befehlserkennung
    - `Map` (falls vorhanden) – Test der Raumerstellung
    - Eigene Klassen

### Aufgabe 9 – Versionierung

- [ ] Versioniere deinen aktuellen Projektstand:
    ```bash
    git status
    git add -A
    git commit -m "zuul 13"
    ```

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **JUnit 5** ist das Standard-Framework für Unit-Tests in Java
    - Tests folgen dem **AAA-Pattern**: Arrange → Act → Assert
    - **@BeforeEach** bereitet jeden Test vor
    - **assertThrows** testet, ob Exceptions geworfen werden
    - **Code Coverage** zeigt, welcher Code getestet wurde

---

??? question "Selbstkontrolle"
    1. Was bedeutet das AAA-Pattern bei Unit-Tests?
    2. Welche Annotation markiert eine Setup-Methode, die vor jedem Test läuft?
    3. Wie testet man, ob eine Methode eine bestimmte Exception wirft?
    4. Warum sollte jeder Test unabhängig von anderen Tests sein?
    
    ??? success "Antworten"
        1. **Arrange** (Vorbereitung), **Act** (Ausführung), **Assert** (Prüfung)
        2. `@BeforeEach`
        3. Mit `assertThrows(ExceptionClass.class, () -> { methodCall(); })`
        4. Damit Tests in beliebiger Reihenfolge laufen können und ein fehlgeschlagener Test keine anderen beeinflusst
