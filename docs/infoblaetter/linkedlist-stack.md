# LinkedList und Stack

## Das Problem: Welche Collection?

Bei der Auswahl einer Collection-Klasse stellt sich die Frage: **Welche Datenstruktur passt zu meinem Problem?**

| Anforderung | Geeignete Collection |
|-------------|---------------------|
| Zugriff über Index (0, 1, 2, ...) | `ArrayList` |
| Zugriff über Schlüssel (Name, ID) | `HashMap` |
| Elemente in umgekehrter Reihenfolge | `Stack` / `LinkedList` |
| Reihenfolge des Einfügens wichtig | `LinkedList` / `ArrayList` |

!!! question "Warum keine HashMap für den Browser-Verlauf?"
    In einem Webbrowser wollen wir uns die **besuchten Seiten merken**, um mit dem Zurück-Button zur vorherigen Seite zu gelangen.
    
    Eine `HashMap` ist hier **nicht geeignet**, weil:
    
    1. **Kein natürlicher Schlüssel**: Welcher Key? Die URL? Dann könnte man jede Seite nur einmal besuchen!
    2. **Keine Reihenfolge**: HashMaps speichern Einträge unsortiert
    3. **Kein "letztes Element"**: Es gibt keine Methode, um den zuletzt hinzugefügten Eintrag zu holen

---

## Das Stack-Prinzip (LIFO)

Ein **Stack** (Stapel) arbeitet nach dem **LIFO-Prinzip**:

!!! info "LIFO = Last In, First Out"
    Das **zuletzt** hinzugefügte Element wird **zuerst** wieder entnommen – wie bei einem Stapel Teller.

```
    ┌─────────────┐
    │  Seite 3    │  ← zuletzt hinzugefügt (top)
    ├─────────────┤
    │  Seite 2    │
    ├─────────────┤
    │  Seite 1    │  ← zuerst hinzugefügt (bottom)
    └─────────────┘
```

### Operationen

| Operation | Beschreibung |
|-----------|--------------|
| `push(element)` | Element oben auf den Stack legen |
| `pop()` | Oberstes Element entfernen und zurückgeben |
| `peek()` | Oberstes Element ansehen (ohne zu entfernen) |
| `isEmpty()` | Prüfen, ob der Stack leer ist |

---

## Die Klasse Stack

Java bietet eine eigene `Stack`-Klasse:

```java
import java.util.Stack;

Stack<String> browserHistory = new Stack<>();

// Seite hinzufügen
browserHistory.push(currentUrl);

// Letzte Seite holen und entfernen
String previousUrl = browserHistory.pop();

// Letzte Seite ansehen (ohne zu entfernen)
String lastUrl = browserHistory.peek();

// Prüfen ob leer
if (!browserHistory.isEmpty()) {
    // ...
}
```

!!! warning "Veraltete Klasse"
    Die Klasse `Stack` gilt als **veraltet** (legacy). Sie erbt von `Vector`, was zu unnötigem Overhead führt.
    
    **Empfohlen:** Verwende stattdessen `Deque` mit `LinkedList` oder `ArrayDeque`.

---

## LinkedList als Stack

Die `LinkedList` implementiert das Interface `Deque` (Double-Ended Queue) und kann als Stack verwendet werden:

```java
import java.util.LinkedList;

LinkedList<String> browserHistory = new LinkedList<>();

// Seite hinzufügen (entspricht push)
browserHistory.addFirst(currentUrl);
// oder: browserHistory.push(currentUrl);

// Letzte Seite holen und entfernen (entspricht pop)
String previousUrl = browserHistory.removeFirst();
// oder: String previousUrl = browserHistory.pop();

// Letzte Seite ansehen
String lastUrl = browserHistory.getFirst();
// oder: String lastUrl = browserHistory.peek();

// Prüfen ob leer
if (!browserHistory.isEmpty()) {
    // ...
}
```

### Warum LinkedList?

| Eigenschaft | ArrayList | LinkedList |
|-------------|-----------|------------|
| Zugriff über Index | ✅ Schnell O(1) | ❌ Langsam O(n) |
| Einfügen am Anfang | ❌ Langsam O(n) | ✅ Schnell O(1) |
| Einfügen am Ende | ✅ Schnell O(1) | ✅ Schnell O(1) |
| Entfernen am Anfang | ❌ Langsam O(n) | ✅ Schnell O(1) |
| Speicherverbrauch | ✅ Gering | ❌ Höher (Zeiger) |

!!! success "Fazit"
    Für Stack-Operationen (Einfügen/Entfernen am Anfang) ist `LinkedList` effizienter als `ArrayList`.

---

## Beispiel: Browser-Navigation

### Implementierung einer Browser-Klasse

```java
public class WebBrowser {
    private String currentPage;
    private LinkedList<String> backHistory = new LinkedList<>();
    
    public void visitPage(String url) {
        // Aktuelle Seite auf den Stack legen
        if (currentPage != null) {
            backHistory.push(currentPage);
        }
        currentPage = url;
        System.out.println("Besuche: " + url);
    }
    
    public boolean goBack() {
        if (backHistory.isEmpty()) {
            System.out.println("Keine vorherige Seite!");
            return false;
        }
        currentPage = backHistory.pop();
        System.out.println("Zurück zu: " + currentPage);
        return true;
    }
}
```

### Ablauf visualisiert

```
Browser startet auf "google.de"
backHistory: []

Benutzer besucht "wikipedia.org"
backHistory: [google.de]

Benutzer besucht "youtube.com"
backHistory: [wikipedia.org, google.de]

Benutzer klickt "Zurück"
→ pop() liefert "wikipedia.org"
backHistory: [google.de]

Benutzer klickt "Zurück"
→ pop() liefert "google.de"
backHistory: []

Benutzer klickt "Zurück"
→ Stack ist leer, Fehlermeldung
```

---

## Das Deque-Interface

`Deque` (Double-Ended Queue) erlaubt Operationen an **beiden Enden**:

```java
import java.util.Deque;
import java.util.LinkedList;
import java.util.ArrayDeque;

// Als LinkedList
Deque<String> history1 = new LinkedList<>();

// Als ArrayDeque (oft schneller)
Deque<String> history2 = new ArrayDeque<>();
```

### Wichtige Methoden

| Stack-Operation | Deque-Methode | Beschreibung |
|-----------------|---------------|--------------|
| push | `addFirst(e)` | Element am Anfang einfügen |
| pop | `removeFirst()` | Erstes Element entfernen |
| peek | `peekFirst()` | Erstes Element ansehen |

| Queue-Operation | Deque-Methode | Beschreibung |
|-----------------|---------------|--------------|
| enqueue | `addLast(e)` | Element am Ende einfügen |
| dequeue | `removeFirst()` | Erstes Element entfernen |

!!! tip "Best Practice"
    Programmiere gegen das **Interface**, nicht die Implementierung:
    
    ```java
    // Gut: Interface als Typ
    Deque<String> history = new LinkedList<>();
    
    // Weniger flexibel: Konkrete Klasse als Typ
    LinkedList<String> history = new LinkedList<>();
    ```

---

## Zusammenfassung

| Konzept | Beschreibung |
|---------|--------------|
| **Stack** | LIFO-Datenstruktur (Last In, First Out) |
| **LinkedList** | Doppelt verkettete Liste, effizient für Stack-Operationen |
| **Deque** | Interface für beidseitige Operationen |
| **push/pop** | Standardoperationen für Stacks |

!!! success "Wann Stack/LinkedList verwenden?"
    - Reihenfolge ist wichtig
    - Letztes Element muss zuerst verarbeitet werden
    - Undo/Back-Funktionalität
    - Klammerprüfung, Ausdrucksauswertung
