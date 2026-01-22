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

### Klassisches Beispiel: Die "Gott-Klasse"

```java
// Schlecht: User macht ALLES
public class User {
    private String name;
    private String email;
    
    // Datenhaltung
    public void saveToDatabase() {
        Connection conn = DriverManager.getConnection("jdbc:mysql://...");
        // SQL-Code hier...
    }
    
    // Validierung
    public boolean isValidEmail() {
        return email.contains("@") && email.contains(".");
    }
    
    // Formatierung
    public String toJSON() {
        return "{\"name\":\"" + name + "\", \"email\":\"" + email + "\"}";
    }
    
    // E-Mail versenden
    public void sendWelcomeEmail() {
        // SMTP-Logik hier...
    }
}
```

!!! danger "Warum ist das problematisch?"
    Diese Klasse hat **vier Gründe sich zu ändern**:
    
    1. Datenbank wechseln (MySQL → PostgreSQL)
    2. Validierungsregeln ändern
    3. Ausgabeformat ändern (JSON → XML)
    4. E-Mail-Provider wechseln
    
    Jede Änderung birgt das Risiko, andere Funktionen zu beschädigen!

### Lösung: Aufgaben trennen

```java
public class User { /* nur Daten */ }
public class UserRepository { /* nur Datenbankzugriff */ }
public class UserValidator { /* nur Validierung */ }
public class UserSerializer { /* nur Formatierung */ }
public class EmailService { /* nur E-Mail-Versand */ }
```

??? tip "In der Lernsituation"
    - **AB 04**: `Room` liefert seine eigene Beschreibung (Entwurf nach Zuständigkeiten)
    - **AB 05**: `CommandWords` verwaltet nur die gültigen Befehle
    - **AB 06**: `Item` modelliert nur Gegenstände, `Room` nur Räume
    - **AB 10**: Jeder Command ist eine eigene Klasse

---

## O – Open/Closed Principle (OCP)

!!! success "Prinzip"
    **Software-Einheiten sollten offen für Erweiterungen, aber geschlossen für Modifikationen sein.**

Neues Verhalten wird durch **Hinzufügen** von Code erreicht, nicht durch **Ändern** von bestehendem Code.

### Klassisches Beispiel: Der Rabatt-Rechner

```java
// Schlecht: Bei jedem neuen Kundentyp muss der Code geändert werden
public class DiscountCalculator {
    
    public double calculateDiscount(Customer customer) {
        if (customer.getType().equals("REGULAR")) {
            return 0.0;
        } 
        else if (customer.getType().equals("PREMIUM")) {
            return 0.1;
        } 
        else if (customer.getType().equals("VIP")) {
            return 0.2;
        }
        // Neuer Kundentyp "STUDENT"? → Hier ändern!
        // Neuer Kundentyp "EMPLOYEE"? → Hier ändern!
        return 0.0;
    }
}
```

!!! danger "Warum ist das problematisch?"
    - Bei **jedem neuen Kundentyp** muss `calculateDiscount()` geändert werden
    - Entwickler können vergessen, **alle Stellen** im Code anzupassen
    - Bestehender, getesteter Code wird immer wieder angefasst

### Lösung: Erweiterung durch neue Klassen

```java
// Gut: Neue Kundentypen durch neue Klassen
public interface DiscountPolicy {
    double getDiscount();
}

public class RegularDiscount implements DiscountPolicy {
    public double getDiscount() { return 0.0; }
}

public class PremiumDiscount implements DiscountPolicy {
    public double getDiscount() { return 0.1; }
}

public class VIPDiscount implements DiscountPolicy {
    public double getDiscount() { return 0.2; }
}

// Neuer Kundentyp? → Neue Klasse, KEIN bestehender Code wird geändert!
public class StudentDiscount implements DiscountPolicy {
    public double getDiscount() { return 0.15; }
}
```

??? tip "In der Lernsituation"
    - **AB 10**: Command-Pattern – neue Befehle ohne Änderung der if-else-Kette
    - **AB 11**: Factory-Pattern – neue Landschaften ohne Änderung von `Game`
    - **AB 09**: State-Pattern – neue Zustände ohne Änderung von `Player`

---

## L – Liskov Substitution Principle (LSP)

!!! success "Prinzip"
    **Objekte einer Subklasse müssen überall verwendbar sein, wo Objekte der Basisklasse erwartet werden – ohne Überraschungen.**

Subklassen dürfen das Verhalten der Basisklasse nicht auf unerwartete Weise ändern.

### Klassisches Beispiel: Rechteck und Quadrat

Mathematisch ist ein Quadrat ein spezielles Rechteck. Aber in der Programmierung führt diese Vererbung zu Problemen:

```java
public class Rectangle {
    protected int width;
    protected int height;
    
    public void setWidth(int w) { 
        this.width = w; 
    }
    
    public void setHeight(int h) { 
        this.height = h; 
    }
    
    public int getArea() { 
        return width * height; 
    }
}

public class Square extends Rectangle {
    // Ein Quadrat muss Breite = Höhe haben
    @Override
    public void setWidth(int w) { 
        this.width = w; 
        this.height = w;  // Höhe muss auch gesetzt werden!
    }
    
    @Override
    public void setHeight(int h) { 
        this.width = h;   // Breite muss auch gesetzt werden!
        this.height = h; 
    }
}
```

### Der Test, der fehlschlägt

```java
public void testRectangle(Rectangle r) {
    r.setWidth(5);
    r.setHeight(4);
    
    // Bei einem Rechteck erwarten wir: 5 × 4 = 20
    assert r.getArea() == 20;
}
```

**Ablauf mit `Rectangle`:**

| Schritt | Aktion | width | height | Fläche |
|---------|--------|-------|--------|--------|
| 1 | `setWidth(5)` | 5 | - | - |
| 2 | `setHeight(4)` | 5 | 4 | 20 ✓ |

**Ablauf mit `Square`:**

| Schritt | Aktion | width | height | Fläche |
|---------|--------|-------|--------|--------|
| 1 | `setWidth(5)` | 5 | 5 | - |
| 2 | `setHeight(4)` | **4** | 4 | **16** ✗ |

!!! danger "Das Problem"
    Der zweite Setter **überschreibt das Ergebnis des ersten**!
    
    Code, der für `Rectangle` geschrieben wurde, erwartet, dass `setWidth()` und `setHeight()` **unabhängig** voneinander arbeiten. Bei `Square` ist das nicht der Fall.
    
    **Erkenntnis:** Mathematisch ist ein Quadrat ein Rechteck, aber **programmiertechnisch verhält es sich anders** → LSP verletzt!

### Korrekte Anwendung des LSP

Wenn Subklassen das gleiche Verhalten wie die Basisklasse haben, funktioniert Polymorphie problemlos:

```java
public class Game {
    private Map currentMap;
    
    public void startGame(Map map) {
        this.currentMap = map;
        // Egal ob MayaMap, UniversityMap oder PirateMap
        // Alle verhalten sich wie erwartet!
        currentMap.createRooms();
        currentMap.setItems();
    }
}
```

??? tip "In der Lernsituation"
    - **AB 11/12**: `MayaMap` und `UniversityMap` können `Map` ersetzen
    - **AB 09**: Alle State-Klassen können `PlayerState` ersetzen
    - **AB 10**: Alle Command-Klassen können `Command` ersetzen

---

## I – Interface Segregation Principle (ISP)

!!! success "Prinzip"
    **Clients sollten nicht von Interfaces abhängen, die sie nicht nutzen.**

Lieber mehrere kleine, spezifische Interfaces als ein großes "Fat Interface".

### Klassisches Beispiel: Der Multifunktionsdrucker

```java
// Schlecht: Ein fettes Interface für alle Geräte
public interface MultiFunctionDevice {
    void print(Document d);
    void scan(Document d);
    void fax(Document d);
    void staple(Document d);
}
```

Was passiert, wenn wir einen einfachen Drucker implementieren wollen?

```java
public class SimplePrinter implements MultiFunctionDevice {
    
    public void print(Document d) { 
        // OK, das kann der Drucker
    }
    
    public void scan(Document d) { 
        throw new UnsupportedOperationException("Kann nicht scannen!");
    }
    
    public void fax(Document d) { 
        throw new UnsupportedOperationException("Kann nicht faxen!");
    }
    
    public void staple(Document d) { 
        throw new UnsupportedOperationException("Kann nicht heften!");
    }
}
```

!!! danger "Warum ist das problematisch?"
    - Die Klasse muss **Methoden implementieren, die sie nicht braucht**
    - Leere Methoden oder `UnsupportedOperationException` sind Code-Smell
    - Clients könnten versehentlich nicht unterstützte Methoden aufrufen

### Lösung: Kleine, spezifische Interfaces

```java
public interface Printer {
    void print(Document d);
}

public interface Scanner {
    void scan(Document d);
}

public interface Fax {
    void fax(Document d);
}

// Einfacher Drucker: implementiert nur Printer
public class SimplePrinter implements Printer {
    public void print(Document d) { /* ... */ }
}

// Multifunktionsgerät: implementiert alles, was es kann
public class AllInOnePrinter implements Printer, Scanner, Fax {
    public void print(Document d) { /* ... */ }
    public void scan(Document d) { /* ... */ }
    public void fax(Document d) { /* ... */ }
}
```

??? tip "In der Lernsituation"
    - **AB 09**: `Herb` könnte ein `Edible`-Interface implementieren
    - **AB 10**: Das `Command`-Interface ist bewusst klein gehalten

---

## D – Dependency Inversion Principle (DIP)

!!! success "Prinzip"
    **High-Level-Module sollten nicht von Low-Level-Modulen abhängen. Beide sollten von Abstraktionen abhängen.**

Konkret: Programmiere gegen **Interfaces**, nicht gegen **konkrete Klassen**.

### Klassisches Beispiel: Die Lampe und der Schalter

```java
// Schlecht: Schalter hängt von konkreter Lampe ab
public class LightBulb {
    public void turnOn() { 
        System.out.println("Glühbirne an"); 
    }
    public void turnOff() { 
        System.out.println("Glühbirne aus"); 
    }
}

public class Switch {
    private LightBulb bulb;  // Konkrete Klasse!
    
    public Switch() {
        this.bulb = new LightBulb();  // Fest verdrahtet!
    }
    
    public void operate() {
        bulb.turnOn();
    }
}
```

!!! danger "Warum ist das problematisch?"
    Was, wenn der Schalter einen **Ventilator**, eine **LED** oder einen **Motor** steuern soll?
    
    → Der komplette `Switch`-Code muss geändert werden!

### Lösung: Abhängigkeit von Abstraktion

```java
// Interface als Abstraktion
public interface Switchable {
    void turnOn();
    void turnOff();
}

// Konkrete Implementierungen
public class LightBulb implements Switchable {
    public void turnOn() { System.out.println("Licht an"); }
    public void turnOff() { System.out.println("Licht aus"); }
}

public class Fan implements Switchable {
    public void turnOn() { System.out.println("Ventilator an"); }
    public void turnOff() { System.out.println("Ventilator aus"); }
}

public class Motor implements Switchable {
    public void turnOn() { System.out.println("Motor läuft"); }
    public void turnOff() { System.out.println("Motor stoppt"); }
}

// Switch hängt nur von der Abstraktion ab
public class Switch {
    private Switchable device;  // Interface!
    
    public Switch(Switchable device) {  // Dependency Injection
        this.device = device;
    }
    
    public void operate() {
        device.turnOn();
    }
}
```

```java
// Verwendung: Switch kann alles steuern!
Switch lightSwitch = new Switch(new LightBulb());
Switch fanSwitch = new Switch(new Fan());
Switch motorSwitch = new Switch(new Motor());
```

```kroki-plantuml
@startuml
skinparam style strictuml
skinparam classAttributeIconSize 0

package "High-Level" {
    class Switch {
        -device: Switchable
        +operate(): void
    }
}

package "Abstraktion" {
    interface Switchable {
        +turnOn(): void
        +turnOff(): void
    }
}

package "Low-Level" {
    class LightBulb
    class Fan
    class Motor
}

Switch --> Switchable
LightBulb ..|> Switchable
Fan ..|> Switchable
Motor ..|> Switchable
@enduml
```

??? tip "In der Lernsituation"
    - **AB 10**: `Parser` kennt nur `Command`-Interface, nicht `GoCommand`
    - **AB 11/12**: `Game` kennt nur `Map`, nicht `MayaMap`
    - **Infoblatt Interfaces**: "Programmiere auf eine Schnittstelle!"

---

## Zusammenfassung

| Prinzip | Frage zur Selbstkontrolle | Typischer Verstoß |
|---------|---------------------------|-------------------|
| **SRP** | Hat meine Klasse nur einen Grund, sich zu ändern? | "Gott-Klasse" macht alles |
| **OCP** | Kann ich erweitern, ohne bestehenden Code zu ändern? | if-else-Kette wächst ständig |
| **LSP** | Kann ich jede Subklasse überall für die Basisklasse einsetzen? | Subklasse verhält sich anders |
| **ISP** | Müssen Klassen Methoden implementieren, die sie nicht brauchen? | Leere Methoden / Exceptions |
| **DIP** | Hängt mein Code von Interfaces oder von konkreten Klassen ab? | `new KonkreteKlasse()` überall |

!!! warning "Pragmatismus"
    Die SOLID-Prinzipien sind **Richtlinien**, keine absoluten Regeln. Bei kleinen Projekten kann strikte Einhaltung zu Over-Engineering führen. Wende sie an, wo sie echten Nutzen bringen!

---

## Weiterführende Links

- [Infoblatt Interfaces](interfaces.md) – Programmieren gegen Schnittstellen
- [Infoblatt Abstrakte Klassen](abstrakte-klassen.md) – Gemeinsame Basisklassen
- [Infoblatt Polymorphismus](polymorphismus.md) – Unterschiedliches Verhalten durch Vererbung
