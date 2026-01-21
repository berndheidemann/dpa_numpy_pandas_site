# Die Welt von Zuul – Exceptions

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- Eigene Exception-Klassen erstellen
- Exceptions mit `throw` werfen und mit `try-catch` abfangen
- Packages zur Strukturierung des Projekts verwenden
- Den Unterschied zwischen Fehlerbehandlung mit Rückgabewerten und Exceptions erklären

---

## Das Problem

In der Methode `takeItem(Command command)` der Klasse `Game` muss mit Hilfe einer if-Anweisung geprüft werden, ob der Aufruf der Methode `dropItem(String name)` erfolgreich war und den Gegenstand tatsächlich entfernt oder ob `null` zurückgegeben worden ist.

!!! warning "Probleme mit diesem Ansatz"
    - Code, der zum Abfangen von Fehlern zuständig ist, mischt sich mit Logikcode
    - Das kann mit **Exceptions** sehr viel eleganter und lesbarer geschehen
    - Außerdem stürzt das Programm ab, wenn der Spieler einen Gegenstand ablegen will, den er gar nicht besitzt...

## Aufgaben

### Aufgabe 1 – Eigene Exception erstellen

- [ ] Schreibe eine eigene Exception mit dem Namen `ItemNotFoundException`

!!! tip "Hilfe"
    Informiere dich dazu mithilfe des [Infoblatts Exceptions](../infoblaetter/exceptions.md).

### Aufgabe 2 – Exception in Room werfen

- [ ] Die Methode `Item removeItem(String name)` der Klasse `Room` soll eine Exception des Typs `ItemNotFoundException` werfen, wenn der gewünschte Gegenstand im Raum nicht gefunden wird. Die Fehlermeldung soll lauten: **"This item does not exist!"**

### Aufgabe 3 – Exception abfangen

- [ ] Fange die Exception in der Klasse `Game` in den Methoden `takeItem(Command command)` und `eatMuffin(Command command)` geeignet ab und gib die Fehlermeldung der Exception aus

### Aufgabe 4 – ItemTooHeavyException

- [ ] Schreibe eine weitere Exceptionklasse mit dem Namen `ItemTooHeavyException`

- [ ] Die Methode `takeItem(Item item)` in der Klasse `Player` soll diese werfen, sobald der Gegenstand zu schwer ist

- [ ] Ändere den Rückgabetyp der Methode `takeItem` von `boolean` auf `void`

### Aufgabe 5 – ItemTooHeavyException abfangen

- [ ] Fange die Exception in der Methode `takeItem(Command command)` der Klasse `Game` geeignet ab

### Aufgabe 6 – dropItem absichern

- [ ] Die Methode `dropItem(String name)` in der Klasse `Player` soll eine Exception vom Typ `ItemNotFoundException` mit der Fehlermeldung **"You don't own this item!"** werfen

- [ ] Fange diese in der Klasse `Game` geeignet ab

### Aufgabe 7 – Package-Struktur

- [ ] Lege folgende Packages an:
    - `de.szut.zuul.exceptions`
    - `de.szut.zuul.model`
    - `de.szut.zuul.gamecontrol`

- [ ] Verschiebe die Klassen entsprechend:

| Package | Klassen |
|---------|---------|
| `exceptions` | Exceptionklassen |
| `gamecontrol` | `ZuulUI`, `Game`, `Parser`, `CommandWords`, `Command` |
| `model` | Übrige Klassen |

!!! tip "IntelliJ Refactor"
    Nutze die **Refactor-Funktion** der rechten Maustaste, um die Klassen zu verschieben. Das erspart dir eine Menge `import`-Anweisungen.
    
    - **Windows/Linux:** Rechtsklick → Refactor → Move oder ++f6++
    - **macOS:** Rechtsklick → Refactor → Move oder ++f6++

### Aufgabe 8 – Versionierung

- [ ] Versioniere deinen aktuellen Projektstand:
    ```bash
    git status
    git add -A
    git commit -m "zuul 8"
    ```

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **Eigene Exceptions** ermöglichen präzise Fehlermeldungen
    - **try-catch** trennt Fehlerbehandlung von Logikcode
    - **throws** deklariert, dass eine Methode Exceptions weitergeben kann
    - **Packages** strukturieren das Projekt logisch

---

??? question "Selbstkontrolle"
    1. Was ist der Vorteil von Exceptions gegenüber Rückgabewerten wie `null` oder `-1`?
    2. Was bedeutet `throws` in einer Methodensignatur?
    3. Warum sollte man eigene Exception-Klassen erstellen?
    
    ??? success "Antworten"
        1. Code ist lesbarer, Fehlerbehandlung ist getrennt, Compiler kann prüfen
        2. Die Methode kann diese Exception werfen und der Aufrufer muss sie behandeln
        3. Präzisere Fehlermeldungen, spezifischere Behandlung möglich
