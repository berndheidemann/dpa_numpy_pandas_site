# Unit-Tests mit JUnit

## Warum testen?

Beim Entwickeln von Software schleichen sich Fehler ein. Je später ein Fehler entdeckt wird, desto teurer ist seine Behebung.

!!! danger "Das Problem"
    - Manuelles Testen ist **zeitaufwändig** und **fehleranfällig**
    - Bei Änderungen muss **alles erneut** getestet werden
    - Entwickler vergessen oft, **alle Fälle** zu testen

!!! success "Die Lösung: Automatisierte Tests"
    **Unit-Tests** sind automatisierte Tests, die einzelne Komponenten (Units) wie Klassen oder Methoden isoliert prüfen.

---

## Was ist JUnit?

**JUnit** ist das Standard-Framework für Unit-Tests in Java:

- Tests werden als normale Java-Methoden geschrieben
- Spezielle **Annotationen** markieren Testmethoden
- **Assertions** prüfen erwartete Ergebnisse
- Tests laufen automatisch und zeigen Erfolg/Fehler an

---

## JUnit einrichten

### In IntelliJ IDEA

1. Rechtsklick auf das Projekt → **Add Framework Support**
2. **JUnit 5** auswählen (oder über Maven/Gradle hinzufügen)

### Maven (pom.xml)

```xml
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.10.0</version>
    <scope>test</scope>
</dependency>
```

### Gradle (build.gradle)

```groovy
testImplementation 'org.junit.jupiter:junit-jupiter:5.10.0'
```

---

## Der erste Test

### Zu testende Klasse

```java
public class Product {
    private String name;
    private double price;
    
    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }
    
    public String getName() {
        return name;
    }
    
    public double getPrice() {
        return price;
    }
}
```

### Testklasse

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class ProductTest {
    
    @Test
    void testProductCreation() {
        // Arrange: Vorbereitung
        Product product = new Product("Laptop", 999.99);
        
        // Assert: Prüfung
        assertEquals("Laptop", product.getName());
        assertEquals(999.99, product.getPrice(), 0.01);
    }
}
```

---

## Wichtige Annotationen

| Annotation | Beschreibung |
|------------|--------------|
| `@Test` | Markiert eine Testmethode |
| `@BeforeEach` | Wird **vor jedem** Test ausgeführt |
| `@AfterEach` | Wird **nach jedem** Test ausgeführt |
| `@BeforeAll` | Wird **einmal vor allen** Tests ausgeführt (static) |
| `@AfterAll` | Wird **einmal nach allen** Tests ausgeführt (static) |
| `@DisplayName` | Gibt dem Test einen lesbaren Namen |
| `@Disabled` | Deaktiviert einen Test temporär |

### Beispiel mit Setup

```java
class BankAccountTest {
    private BankAccount account;
    private BankAccount targetAccount;
    
    @BeforeEach
    void setUp() {
        // Wird vor jedem Test ausgeführt
        account = new BankAccount("DE123456", 1000.0);
        targetAccount = new BankAccount("DE789012", 500.0);
    }
    
    @Test
    @DisplayName("Überweisung sollte Betrag korrekt übertragen")
    void testTransfer() {
        account.transfer(targetAccount, 200.0);
        assertEquals(800.0, account.getBalance(), 0.01);
        assertEquals(700.0, targetAccount.getBalance(), 0.01);
    }
    
    @Test
    @DisplayName("Abhebung mit zu wenig Guthaben sollte fehlschlagen")
    void testWithdrawInsufficientFunds() {
        boolean result = account.withdraw(2000.0);
        assertFalse(result);
        assertEquals(1000.0, account.getBalance(), 0.01);
    }
}
```

---

## Wichtige Assertions

### Gleichheit prüfen

```java
// Werte vergleichen
assertEquals(expected, actual);
assertEquals(expected, actual, "Fehlermeldung");

// Objekte auf Identität prüfen
assertSame(expected, actual);     // Gleiche Referenz
assertNotSame(unexpected, actual); // Verschiedene Referenzen
```

### Boolean prüfen

```java
assertTrue(condition);
assertFalse(condition);
```

### Null prüfen

```java
assertNull(object);
assertNotNull(object);
```

### Exceptions prüfen

```java
@Test
void testExceptionThrown() {
    BankAccount account = new BankAccount("DE123", 100.0);
    
    // Prüfen, dass Exception geworfen wird
    assertThrows(InsufficientFundsException.class, () -> {
        account.withdraw(500.0);
    });
}
```

### Arrays und Collections

```java
assertArrayEquals(expectedArray, actualArray);
assertEquals(expectedList, actualList);
assertTrue(collection.contains(element));
```

---

## AAA-Pattern

Tests folgen dem **AAA-Pattern**:

```java
@Test
void testAddProductToCart() {
    // Arrange: Testobjekte vorbereiten
    ShoppingCart cart = new ShoppingCart();
    Product laptop = new Product("Laptop", 999.99);
    Product mouse = new Product("Maus", 29.99);
    
    // Act: Die zu testende Aktion ausführen
    cart.addProduct(laptop);
    cart.addProduct(mouse);
    
    // Assert: Ergebnis prüfen
    assertEquals(2, cart.getItemCount());
    assertEquals(1029.98, cart.getTotalPrice(), 0.01);
}
```

| Phase | Beschreibung |
|-------|--------------|
| **Arrange** | Testobjekte und Testdaten vorbereiten |
| **Act** | Die zu testende Methode aufrufen |
| **Assert** | Das Ergebnis überprüfen |

---

## Praxisbeispiele

### Product-Tests

```java
class ProductTest {
    
    @Test
    void testProductHasCorrectName() {
        Product product = new Product("Laptop", 999.99);
        assertEquals("Laptop", product.getName());
    }
    
    @Test
    void testProductHasCorrectPrice() {
        Product product = new Product("Maus", 29.99);
        assertEquals(29.99, product.getPrice(), 0.01);
    }
    
    @Test
    void testApplyDiscount() {
        Product product = new Product("Laptop", 100.0);
        product.applyDiscount(10); // 10% Rabatt
        assertEquals(90.0, product.getPrice(), 0.01);
    }
}
```

### ShoppingCart-Tests

```java
class ShoppingCartTest {
    private ShoppingCart cart;
    
    @BeforeEach
    void setUp() {
        cart = new ShoppingCart();
    }
    
    @Test
    void testCartStartsEmpty() {
        assertTrue(cart.isEmpty());
        assertEquals(0, cart.getItemCount());
    }
    
    @Test
    void testAddProduct() {
        Product laptop = new Product("Laptop", 999.99);
        cart.addProduct(laptop);
        
        assertFalse(cart.isEmpty());
        assertEquals(1, cart.getItemCount());
    }
    
    @Test
    void testRemoveProduct() {
        Product laptop = new Product("Laptop", 999.99);
        cart.addProduct(laptop);
        
        cart.removeProduct("Laptop");
        
        assertTrue(cart.isEmpty());
    }
    
    @Test
    void testRemoveNonExistentProduct() {
        assertThrows(ProductNotFoundException.class, () -> {
            cart.removeProduct("Nicht vorhanden");
        });
    }
    
    @Test
    void testTotalPrice() {
        cart.addProduct(new Product("Laptop", 999.99));
        cart.addProduct(new Product("Maus", 29.99));
        cart.addProduct(new Product("Tastatur", 79.99));
        
        assertEquals(1109.97, cart.getTotalPrice(), 0.01);
    }
}
```

### BankAccount-Tests

```java
class BankAccountTest {
    private BankAccount account;
    
    @BeforeEach
    void setUp() {
        account = new BankAccount("DE123456789", 1000.0);
    }
    
    @Test
    void testInitialBalance() {
        assertEquals(1000.0, account.getBalance(), 0.01);
    }
    
    @Test
    void testDeposit() {
        account.deposit(500.0);
        assertEquals(1500.0, account.getBalance(), 0.01);
    }
    
    @Test
    void testWithdraw() {
        boolean success = account.withdraw(300.0);
        assertTrue(success);
        assertEquals(700.0, account.getBalance(), 0.01);
    }
    
    @Test
    void testWithdrawInsufficientFunds() {
        assertThrows(InsufficientFundsException.class, () -> {
            account.withdraw(2000.0);
        });
    }
    
    @Test
    void testTransfer() {
        BankAccount target = new BankAccount("DE987654321", 0.0);
        
        account.transfer(target, 250.0);
        
        assertEquals(750.0, account.getBalance(), 0.01);
        assertEquals(250.0, target.getBalance(), 0.01);
    }
    
    @Test
    void testNegativeDepositThrowsException() {
        assertThrows(IllegalArgumentException.class, () -> {
            account.deposit(-100.0);
        });
    }
}
```

### OrderState-Tests (State-Pattern)

```java
class OrderStateTest {
    private Order order;
    
    @BeforeEach
    void setUp() {
        order = new Order("ORD-12345");
    }
    
    @Test
    void testOrderStartsPending() {
        assertEquals("wartend auf Zahlung", order.getStateDescription());
    }
    
    @Test
    void testPaymentChangeStateToPaid() {
        order.pay();
        assertEquals("bezahlt", order.getStateDescription());
    }
    
    @Test
    void testShipmentRequiresPayment() {
        order.ship(); // Sollte fehlschlagen
        assertEquals("wartend auf Zahlung", order.getStateDescription());
    }
    
    @Test
    void testSuccessfulShipment() {
        order.pay();
        order.ship();
        assertEquals("versendet", order.getStateDescription());
    }
    
    @Test
    void testShippedOrderCannotBeCancelled() {
        order.pay();
        order.ship();
        order.cancel(); // Sollte im gleichen Zustand bleiben
        assertEquals("versendet", order.getStateDescription());
    }
}
```

---

## Tests ausführen

### In IntelliJ IDEA

- **Einzelner Test**: Grüner Pfeil neben der Methode
- **Alle Tests einer Klasse**: Grüner Pfeil neben dem Klassennamen
- **Alle Tests**: Rechtsklick auf `test`-Ordner → **Run All Tests**
- **Shortcut**: ++ctrl+shift+f10++ (Windows/Linux) / ++cmd+shift+r++ (macOS)

### Über die Kommandozeile

```bash
# Maven
mvn test

# Gradle
gradle test
```

---

## Testabdeckung (Code Coverage)

IntelliJ kann anzeigen, welcher Code durch Tests abgedeckt ist:

1. Rechtsklick auf Testklasse → **Run with Coverage**
2. Grün = getestet, Rot = nicht getestet

!!! tip "Ziel"
    - **80%+ Coverage** für kritische Klassen
    - Nicht jede Zeile muss getestet werden
    - Fokus auf **wichtige Logik** und **Randfälle**

---

## Best Practices

### 1. Aussagekräftige Testnamen

```java
// Schlecht
@Test void test1() { ... }

// Gut
@Test void testPlayerCannotCarryItemExceedingMaxWeight() { ... }
```

### 2. Ein Assert pro Test (wenn möglich)

```java
// Besser aufteilen
@Test void testItemName() { assertEquals("Schwert", item.getName()); }
@Test void testItemWeight() { assertEquals(5, item.getWeight()); }
```

### 3. Tests sind unabhängig

Kein Test sollte vom Ergebnis eines anderen abhängen.

### 4. Tests sind wiederholbar

Gleiches Ergebnis bei jedem Durchlauf (keine Zufallswerte ohne Seed).

### 5. Schnelle Tests

Unit-Tests sollten in Millisekunden laufen.

---

## Zusammenfassung

| Konzept | Beschreibung |
|---------|--------------|
| **JUnit** | Framework für automatisierte Tests |
| **@Test** | Markiert eine Testmethode |
| **Assertions** | Prüfen erwartete Ergebnisse |
| **AAA-Pattern** | Arrange → Act → Assert |
| **Coverage** | Prozent des getesteten Codes |

!!! success "Vorteile von Unit-Tests"
    - Fehler werden **früh** entdeckt
    - **Refactoring** wird sicherer
    - Tests dienen als **Dokumentation**
    - **Vertrauen** in den Code steigt
