# Das State-Pattern

## Motivation

Objekte verhalten sich oft unterschiedlich, je nachdem in welchem **Zustand** sie sich befinden. Ein klassisches Beispiel:

```java
// Problematischer Code mit vielen if-else
public class Order {
    private String state = "pending"; // pending, paid, shipped, delivered
    
    public void pay() {
        if (state.equals("pending")) {
            state = "paid";
        } else if (state.equals("paid")) {
            // Bereits bezahlt
        } else if (state.equals("shipped")) {
            // Kann nicht mehr bezahlen
        }
    }
    
    public void ship() {
        if (state.equals("pending")) {
            // Noch nicht bezahlt!
        } else if (state.equals("paid")) {
            state = "shipped";
        } else if (state.equals("shipped")) {
            // Bereits versendet
        }
    }
    
    // ... weitere Methoden mit ähnlichen if-else-Ketten
}
```

!!! warning "Probleme mit diesem Ansatz"
    1. **Unübersichtlich**: Jede Methode enthält Fallunterscheidungen für alle Zustände
    2. **Fehleranfällig**: Bei neuen Zuständen müssen alle Methoden angepasst werden
    3. **Schlecht erweiterbar**: Neue Zustände führen zu vielen Codeänderungen
    4. **Verletzt Open/Closed-Prinzip**: Bestehender Code muss geändert werden

---

## Die Lösung: State-Pattern

Das **State-Pattern** kapselt jeden Zustand in einer eigenen Klasse. Das Objekt delegiert zustandsabhängiges Verhalten an das aktuelle Zustandsobjekt.

!!! success "Definition"
    Das State-Pattern ermöglicht es einem Objekt, sein **Verhalten zu ändern**, wenn sich sein **interner Zustand** ändert. Es sieht so aus, als würde das Objekt seine Klasse wechseln.

---

## Struktur des Patterns

```kroki-plantuml
@startuml
skinparam style strictuml
skinparam classAttributeIconSize 0

class Context {
    -state: State
    +setState(State): void
    +request(): void
}

class State <<interface>> {
    +handle(Context): void
}

class ConcreteStateA {
    +handle(Context): void
}

class ConcreteStateB {
    +handle(Context): void
}

Context --> State : hält aktuellen
ConcreteStateA ..|> State
ConcreteStateB ..|> State
@enduml
```

### Rollen im Pattern

| Rolle | Beschreibung | Beispiel: Online-Shop |
|-------|--------------|------------------------|
| **Context** | Objekt mit zustandsabhängigem Verhalten | `Order` (Bestellung) |
| **State** | Interface für alle Zustände | `OrderState` Interface |
| **ConcreteState** | Implementiert Verhalten für einen Zustand | `PendingState`, `PaidState`, `ShippedState` |

---

## Implementierung

### 1. Das State-Interface

```java
public interface OrderState {
    /**
     * Bestellung wird bezahlt
     * @return Der neue Zustand nach der Zahlung
     */
    OrderState pay();
    
    /**
     * Bestellung wird versendet
     * @return Der neue Zustand nach dem Versand
     */
    OrderState ship();
    
    /**
     * Bestellung wird storniert
     * @return Der neue Zustand nach der Stornierung
     */
    OrderState cancel();
    
    /**
     * @return Beschreibung des aktuellen Zustands
     */
    String getDescription();
}
```

### 2. Konkrete Zustände

```java
public class PendingState implements OrderState {
    
    @Override
    public OrderState pay() {
        System.out.println("Zahlung eingegangen. Bestellung wird bearbeitet.");
        return new PaidState();
    }
    
    @Override
    public OrderState ship() {
        System.out.println("Fehler: Bestellung muss erst bezahlt werden!");
        return this; // Bleibt im gleichen Zustand
    }
    
    @Override
    public OrderState cancel() {
        System.out.println("Bestellung wurde storniert.");
        return new CancelledState();
    }
    
    @Override
    public String getDescription() {
        return "wartend auf Zahlung";
    }
}
```

```java
public class PaidState implements OrderState {
    
    @Override
    public OrderState pay() {
        System.out.println("Bestellung ist bereits bezahlt.");
        return this;
    }
    
    @Override
    public OrderState ship() {
        System.out.println("Bestellung wird versendet.");
        return new ShippedState();
    }
    
    @Override
    public OrderState cancel() {
        System.out.println("Bestellung storniert. Rückerstattung wird veranlasst.");
        return new CancelledState();
    }
    
    @Override
    public String getDescription() {
        return "bezahlt";
    }
}
```

```java
public class ShippedState implements OrderState {
    
    @Override
    public OrderState pay() {
        System.out.println("Bestellung ist bereits bezahlt.");
        return this;
    }
    
    @Override
    public OrderState ship() {
        System.out.println("Bestellung wurde bereits versendet.");
        return this;
    }
    
    @Override
    public OrderState cancel() {
        System.out.println("Versendete Bestellung kann nicht storniert werden.");
        return this;
    }
    
    @Override
    public String getDescription() {
        return "versendet";
    }
}
```

### 3. Der Context (Order)

```java
public class Order {
    private String orderId;
    private List<String> items;
    private OrderState state;
    
    public Order(String orderId) {
        this.orderId = orderId;
        this.items = new ArrayList<>();
        this.state = new PendingState(); // Startzustand
    }
    
    public void pay() {
        state = state.pay();
    }
    
    public void ship() {
        state = state.ship();
    }
    
    public void cancel() {
        state = state.cancel();
    }
    
    public void showStatus() {
        System.out.println("Bestellung: " + orderId);
        System.out.println("Status: " + state.getDescription());
    }
    
    public boolean canBeEdited() {
        // Bearbeitung nur möglich wenn noch nicht versendet
        return state instanceof PendingState || state instanceof PaidState;
    }
}
```

---

## Zustandsübergänge

Die Übergänge zwischen Zuständen lassen sich als **Zustandsdiagramm** darstellen:

```kroki-plantuml
@startuml
skinparam style strictuml

[*] --> Wartend

Wartend --> Bezahlt : bezahlen
Wartend --> Storniert : stornieren
Wartend --> Wartend : versenden (Fehler)

Bezahlt --> Versendet : versenden
Bezahlt --> Storniert : stornieren

Versendet --> Zugestellt : zustellen
Versendet --> Versendet : stornieren (Fehler)

Zugestellt --> [*]
Storniert --> [*]
@enduml
```

### Übergangstabelle

| Aktueller Zustand | Aktion | Neuer Zustand |
|-------------------|--------|---------------|
| wartend | bezahlen | bezahlt |
| wartend | versenden | wartend (Fehler) |
| wartend | stornieren | storniert |
| bezahlt | bezahlen | bezahlt (bereits bezahlt) |
| bezahlt | versenden | versendet |
| bezahlt | stornieren | storniert |
| versendet | versenden | versendet (bereits versendet) |
| versendet | stornieren | versendet (nicht möglich) |

---

## Verbesserung: Singleton-Zustände

!!! danger "Problem"
    Bei jedem Zustandswechsel wird ein **neues Objekt** erzeugt. Das belastet den Speicher unnötig.

### Lösung: Singleton für jeden Zustand

```java
public class PendingState implements OrderState {
    private static PendingState instance;
    
    private PendingState() {
        // Private Konstruktor
    }
    
    public static PendingState getInstance() {
        if (instance == null) {
            instance = new PendingState();
        }
        return instance;
    }
    
    @Override
    public OrderState pay() {
        System.out.println("Zahlung eingegangen. Bestellung wird bearbeitet.");
        return PaidState.getInstance();
    }
    
    // ... weitere Methoden
}
```

!!! tip "Vorteile von Singleton-Zuständen"
    - Nur **eine Instanz** pro Zustand im Speicher
    - Zustandsobjekte können **wiederverwendet** werden
    - Vergleich mit `==` möglich statt `instanceof`

---

## Vorteile des State-Patterns

### 1. Klare Struktur

Jeder Zustand hat seine eigene Klasse mit fokussiertem Code:

```java
// Statt einer großen Klasse mit if-else:
class PendingState { /* nur Pending-Logik */ }
class PaidState { /* nur Paid-Logik */ }
class ShippedState { /* nur Shipped-Logik */ }
```

### 2. Einfache Erweiterung

Neuen Zustand hinzufügen ohne bestehenden Code zu ändern:

```java
// Neuer Zustand "Zurückgegeben"
public class ReturnedState implements OrderState {
    @Override
    public OrderState pay() {
        return this; // Zurückgegebene Bestellung kann nicht bezahlt werden
    }
    // ...
}
```

### 3. Zustandsübergänge sind explizit

Die Übergänge sind im Code der Zustandsklassen klar definiert:

```java
public OrderState pay() {
    return new PaidState(); // Expliziter Übergang
}
```

---

## Vergleich: Vorher vs. Nachher

### Vorher (ohne State-Pattern)

```java
public void ship() {
    if (state.equals("pending")) {
        // Fehler: nicht bezahlt
    } else if (state.equals("paid")) {
        state = "shipped";
    } else if (state.equals("shipped")) {
        // bereits versendet
    } else if (state.equals("returned")) {
        // Fehler: zurückgegeben
    }
    // Bei jedem neuen Zustand: ALLE Methoden anpassen!
}
```

### Nachher (mit State-Pattern)

```java
public void ship() {
    state = state.ship(); // Delegation an aktuellen Zustand
}
```

---

## Wann State-Pattern verwenden?

| Situation | State-Pattern? |
|-----------|----------------|
| Objekt hat 3+ verschiedene Zustände | ✅ Ja |
| Verhalten ändert sich je nach Zustand | ✅ Ja |
| Viele if-else basierend auf Zustand | ✅ Ja |
| Zustandsübergänge komplex | ✅ Ja |
| Nur 2 Zustände (an/aus) | ❌ Overkill |
| Zustand ändert sich nie | ❌ Nicht nötig |

---

## Zusammenfassung

| Konzept | Beschreibung |
|---------|--------------|
| **State** | Kapselt zustandsabhängiges Verhalten |
| **Context** | Hält aktuellen Zustand, delegiert Aufrufe |
| **Übergang** | Methoden geben neuen Zustand zurück |
| **Singleton** | Optimierung: Nur eine Instanz pro Zustand |

!!! tip "Merksatz"
    Das State-Pattern ersetzt **komplexe Fallunterscheidungen** durch **Polymorphie** – jeder Zustand weiß selbst, wie er sich verhalten soll.
