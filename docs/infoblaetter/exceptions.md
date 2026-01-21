# Exceptions

## 1) Behandlung von Ausnahmen

### Klassische Abfang-Abfragen

```java
int number1 = sc.nextInt();
int number2 = sc.nextInt();
if (number2 != 0) {
    int result = number1 / number2;
} else {
    System.out.println("Keine 0 eingeben!");
}
```

### Probleme mit Abfang-Abfragen

In vielen Situationen ist eine solche Abfang-Abfrage nicht möglich, weil sie gerade den Fehler produziert, der vermieden werden soll.

!!! example "Beispiel"
    Um zu verhindern, dass eine Berechnung den Zahlenbereich der ganzen Zahlen überschreitet, wäre eine Abfrage vom Typ `if(number * number <= biggestRepresentableNumber)` nötig. Sie führt aber gerade die Berechnung aus, die zum Fehler führt.

Außerdem haben Abfang-Abfragen den Nachteil, dass:

- der Programmieraufwand steigt
- der Quellcode sehr unübersichtlich wird, weil jedem möglichen Fehler eine Abfrage gewidmet wird

!!! success "Lösung"
    Java bietet deswegen ein eigenes Sprachkonstrukt an, das die Fehlerbehandlung erleichtert und damit das **defensive Programmieren** unterstützt.

### Exceptions behandeln mit try-catch

Tritt in Java eine Situation ein, die zum Absturz der Anwendung führt, löst Java eine sogenannte **Ausnahme (Exception)** aus. Der Programmablauf verzweigt von der Stelle, an der das Ausnahmeereignis aufgetreten ist, zu einer vom Programmierer anzugebenden Stelle, in der die Ausnahme behandelt werden muss.

```java
try {
    // Code, der eine Exception auslösen kann
} catch (Exceptiontyp parametername) {
    // Behandlung
} finally {
    // Abschlussarbeiten (optional)
}
```

!!! tip "Lesart"
    Diese try-catch-Behandlung lässt sich als „**Versuche dieses, wenn ein Fehler dabei auftritt, mache jenes.**" lesen.

### Syntax-Erklärung

| Element | Beschreibung |
|---------|--------------|
| `try` | Leitet einen Exception-Block ein |
| Code im try-Block | Quellcode, der eine Exception auslösen kann |
| `catch(Exceptiontyp e)` | Fängt eine ganz bestimmte Ausnahme ab. Bei `Exception` werden alle Ausnahmen abgefangen |
| Parametername | Ermöglicht Zugriff auf das Exception-Objekt |
| `finally` | Befehle, die in jedem Fall ausgeführt werden (z.B. Dateien schließen) |

### Beispiel

```java
try {
    result = number1 / number2;
} catch (ArithmeticException e) {
    System.out.println("Nicht durch 0 teilen!");
}
```

Bei einer Division durch 0 wirft Java eine `ArithmeticException`. In diesem Fall bekommt der Benutzer die Fehlermeldung „Nicht durch 0 teilen!".

## 2) Eigene Exceptionklassen definieren

Spätestens dann, wenn du größere Projekte angehst oder damit beginnst, eigene Klassenhierarchien aufzubauen, kann es sinnvoll sein, **eigene Exceptions** zu entwickeln und zu werfen.

!!! success "Vorteile eigener Exceptions"
    - Wesentlich gezieltere Fehlermeldungen für bestimmte Situationen
    - Programme können genauer auf bestimmte Fehlersituationen reagieren

### Aufbau einer eigenen Exception

```java
public class MyException extends Exception {
    
    public MyException() {
    }
    
    public MyException(String s) {
        super(s);
    }
}
```

Die `Throwable`-Klasse definiert unter anderem:

- einen Standard-Konstruktor
- einen mit einem String parametrisierten Konstruktor, um eine Fehlermeldung (die Exception-Message) anzunehmen und zu speichern

Definiert man eigene Exceptions, so sollten diese die entsprechenden Konstruktoren **redefinieren**.

## 3) Exceptions weitergeben

### Das Problem mit Rückgabewerten

In vielen Fällen ist es gar nicht sinnvoll, Fehler an der Stelle zu behandeln, an der sie aufgetreten sind. Oft kann eine aufgerufene Methode gar nicht wissen, was sie im Fehlerfall tun soll.

!!! question "Fragen"
    - Ist eine Fehlermeldung auf der Konsole oder in einem Fenster gewünscht?
    - Soll der Rückgabewert als Fehlerindikator dienen?

Eine Idee wäre, Fehler über den Rückgabewert der Methode anzuzeigen (z.B. `false` oder `-1`). Das ist allerdings **nicht sinnvoll**, weil:

1. Die aufrufende Methode muss immerzu Rückgabewerte auf Fehlerfälle hin testen
2. Normaler Code wird ständig mit Fehlerbehandlungscode gemischt
3. Häufig sind alle Parameterwerte gültige Rückgabewerte

### Lösung: Exceptions weitergeben mit throws

```java
public static void main(String[] args) {
    int number1, number2;
    Scanner sc = new Scanner(System.in);
    number1 = sc.nextInt();
    number2 = sc.nextInt();
    try {
        System.out.println(divide(number1, number2));
    } catch (ArithmeticException e) {
        System.out.println("Es darf nicht durch 0 geteilt werden!");
    }
}

public static int divide(int number1, int number2) throws ArithmeticException {
    return number1 / number2;
}
```

### Eigene Exception werfen mit throw

```java
public static int divide(int number1, int number2) throws ArithmeticException {
    if (number2 == 0) {
        throw new ArithmeticException("Nicht durch 0 teilen");
    } else {
        return number1 / number2;
    }
}
```

Der Aufruf kann dann die Fehlermeldung der Exception ausgeben:

```java
catch (ArithmeticException e) {
    System.out.println(e.getMessage());
}
```
