# Das Command-Pattern

## Motivation

In vielen Anwendungen gibt es **Befehle**, die der Benutzer auslösen kann – zum Beispiel durch Menüs, Buttons oder Texteingabe. Oft werden diese Befehle direkt im Code behandelt:

```java
// Problematischer Code
if (commandWord.equals("help")) {
    printHelp();
} else if (commandWord.equals("go")) {
    goRoom(command);
} else if (commandWord.equals("quit")) {
    quit();
} else if (commandWord.equals("look")) {
    look();
}
// ... und so weiter für jeden neuen Befehl
```

!!! warning "Probleme mit diesem Ansatz"
    1. **Geringe Kohäsion**: Die Klasse macht zu viele verschiedene Dinge
    2. **Schwer erweiterbar**: Für jeden neuen Befehl muss die if-else-Kette erweitert werden
    3. **Kein Undo möglich**: Befehle können nicht rückgängig gemacht werden
    4. **Keine Wiederverwendung**: Befehle sind fest an die Klasse gebunden

---

## Die Lösung: Command-Pattern

Das **Command-Pattern** kapselt einen Befehl als Objekt. Dadurch können Befehle:

- **Gespeichert** werden
- **Weitergegeben** werden
- **Rückgängig gemacht** werden (Undo)
- **Protokolliert** werden

!!! success "Definition"
    Das Command-Pattern **kapselt einen Methodenaufruf** in einem Objekt. Dadurch können Clients mit verschiedenen Anfragen parametrisiert werden.

---

## Struktur des Patterns

```kroki-plantuml
@startuml
skinparam style strictuml
skinparam classAttributeIconSize 0

class Command <<interface>> {
    +execute(): void
    +undo(): void
}

class ConcreteCommand {
    -receiver: Receiver
    +execute(): void
    +undo(): void
}

class Invoker {
    -command: Command
    +setCommand(Command): void
    +executeCommand(): void
}

class Receiver {
    +action(): void
}

class Client

ConcreteCommand ..|> Command
ConcreteCommand --> Receiver : ruft auf
Invoker --> Command : hält
Client --> ConcreteCommand : erstellt
Client --> Receiver : erstellt
@enduml
```

### Rollen im Pattern

| Rolle | Beschreibung | Beispiel: Texteditor |
|-------|--------------|----------------------|
| **Command** | Interface für alle Befehle | `Command` Interface |
| **ConcreteCommand** | Implementiert einen konkreten Befehl | `CopyCommand`, `PasteCommand`, `BoldCommand` |
| **Invoker** | Löst Befehle aus | `Toolbar`, `Menu` |
| **Receiver** | Führt die eigentliche Aktion aus | `TextDocument`, `Cursor` |
| **Client** | Erstellt und konfiguriert Befehle | `EditorApplication` (bei Initialisierung) |

---

## Implementierung

### 1. Das Command-Interface

```java
public interface Command {
    /**
     * Führt den Befehl aus
     */
    void execute();
    
    /**
     * Macht den Befehl rückgängig
     */
    void undo();
}
```

### 2. Ein konkreter Befehl: CopyCommand

```java
public class CopyCommand implements Command {
    private TextDocument document;
    private Clipboard clipboard;
    
    public CopyCommand(TextDocument document, Clipboard clipboard) {
        this.document = document;
        this.clipboard = clipboard;
    }
    
    @Override
    public void execute() {
        // Markierten Text in Zwischenablage kopieren
        String selectedText = document.getSelectedText();
        if (selectedText != null && !selectedText.isEmpty()) {
            clipboard.setContent(selectedText);
            System.out.println("Text kopiert: " + selectedText);
        } else {
            System.out.println("Kein Text markiert!");
        }
    }
    
    @Override
    public void undo() {
        // Copy hat keinen Effekt zum Rückgängigmachen
    }
}
```

### 3. Weitere Befehle

```java
public class PasteCommand implements Command {
    private TextDocument document;
    private Clipboard clipboard;
    private String previousText;  // Für Undo
    private int insertPosition;
    
    public PasteCommand(TextDocument document, Clipboard clipboard) {
        this.document = document;
        this.clipboard = clipboard;
    }
    
    @Override
    public void execute() {
        insertPosition = document.getCursorPosition();
        previousText = document.getText();
        
        String clipboardContent = clipboard.getContent();
        document.insertText(insertPosition, clipboardContent);
        System.out.println("Text eingefügt: " + clipboardContent);
    }
    
    @Override
    public void undo() {
        document.setText(previousText);
        System.out.println("Einfügen rückgängig gemacht.");
    }
}
```

```java
public class BoldCommand implements Command {
    private TextDocument document;
    private int selectionStart;
    private int selectionEnd;
    private boolean wasBold;
    
    public BoldCommand(TextDocument document) {
        this.document = document;
    }
    
    @Override
    public void execute() {
        selectionStart = document.getSelectionStart();
        selectionEnd = document.getSelectionEnd();
        wasBold = document.isBold(selectionStart, selectionEnd);
        
        document.toggleBold(selectionStart, selectionEnd);
        System.out.println("Fettschrift umgeschaltet.");
    }
    
    @Override
    public void undo() {
        document.setBold(selectionStart, selectionEnd, wasBold);
    }
}
```

### 4. Der Invoker (Toolbar)

```java
public class Toolbar {
    private Map<String, Command> commands = new HashMap<>();
    private Deque<Command> history = new LinkedList<>();
    
    public void registerCommand(String buttonName, Command command) {
        commands.put(buttonName, command);
    }
    
    public void buttonClicked(String buttonName) {
        Command command = commands.get(buttonName);
        if (command != null) {
            command.execute();
            history.push(command); // Für Undo speichern
        } else {
            System.out.println("Unbekannter Button: " + buttonName);
        }
    }
    
    public void undoLastCommand() {
        if (!history.isEmpty()) {
            Command lastCommand = history.pop();
            lastCommand.undo();
        } else {
            System.out.println("Nichts zum Rückgängigmachen!");
        }
    }
}
```

### 5. Der Client (TextEditor)

```java
public class TextEditor {
    private Toolbar toolbar;
    private TextDocument document;
    private Clipboard clipboard;
    
    public TextEditor() {
        document = new TextDocument();
        clipboard = new Clipboard();
        
        toolbar = new Toolbar();
        // Befehle registrieren
        toolbar.registerCommand("copy", new CopyCommand(document, clipboard));
        toolbar.registerCommand("paste", new PasteCommand(document, clipboard));
        toolbar.registerCommand("bold", new BoldCommand(document));
        toolbar.registerCommand("undo", new UndoCommand(toolbar));
    }
    
    public void onButtonClick(String buttonName) {
        if (buttonName.equals("undo")) {
            toolbar.undoLastCommand();
        } else {
            toolbar.buttonClicked(buttonName);
        }
    }
}
```

---

## Vorteile des Command-Patterns

### 1. Entkopplung

Der **Invoker** (Parser) muss nicht wissen, **wie** ein Befehl ausgeführt wird:

```java
// Invoker kennt nur das Interface
Command cmd = commands.get("go");
cmd.execute(); // Egal welcher konkrete Command
```

### 2. Einfache Erweiterung

Neue Befehle hinzufügen, ohne bestehenden Code zu ändern:

```java
// Neuen Befehl erstellen
public class ItalicCommand implements Command {
    // ...
}

// Registrieren
toolbar.registerCommand("italic", new ItalicCommand(document));
```

!!! success "Open/Closed-Prinzip"
    Die Klasse ist **offen für Erweiterungen** (neue Commands), aber **geschlossen für Modifikationen** (kein Ändern der if-else-Kette).

### 3. Undo-Funktionalität

Jeder Befehl speichert seinen Zustand und kann sich selbst rückgängig machen:

```java
public class DeleteTextCommand implements Command {
    private TextDocument document;
    private String deletedText;
    private int deletePosition;
    
    @Override
    public void execute() {
        deletePosition = document.getSelectionStart();
        deletedText = document.getSelectedText();
        document.deleteSelection();
    }
    
    @Override
    public void undo() {
        document.insertText(deletePosition, deletedText);
    }
}
```

### 4. Befehlshistorie und Makros

```java
// Mehrere Befehle als Makro speichern
public class MacroCommand implements Command {
    private List<Command> commands = new ArrayList<>();
    
    public void addCommand(Command cmd) {
        commands.add(cmd);
    }
    
    @Override
    public void execute() {
        for (Command cmd : commands) {
            cmd.execute();
        }
    }
    
    @Override
    public void undo() {
        // Rückwärts durchlaufen
        for (int i = commands.size() - 1; i >= 0; i--) {
            commands.get(i).undo();
        }
    }
}
```

---

## Vergleich: Vorher vs. Nachher

### Vorher (ohne Command-Pattern)

```java
// In der Editor-Klasse: 100+ Zeilen
private void handleAction(String action) {
    if (action.equals("copy")) {
        // 15 Zeilen Copy-Logik
    } else if (action.equals("paste")) {
        // 20 Zeilen Paste-Logik
    } else if (action.equals("bold")) {
        // 15 Zeilen Bold-Logik
    }
    // ... und so weiter
}
```

### Nachher (mit Command-Pattern)

```java
// Editor-Klasse: Nur Registrierung
toolbar.registerCommand("copy", new CopyCommand(document, clipboard));
toolbar.registerCommand("paste", new PasteCommand(document, clipboard));
toolbar.registerCommand("bold", new BoldCommand(document));

// Jeder Command: Eine fokussierte Klasse
public class CopyCommand implements Command {
    // Nur Copy-Logik, ca. 15 Zeilen
}
```

---

## Wann Command-Pattern verwenden?

| Situation | Command-Pattern? |
|-----------|------------------|
| Undo/Redo benötigt | ✅ Ja |
| Viele verschiedene Befehle | ✅ Ja |
| Befehle protokollieren | ✅ Ja |
| Befehle zeitversetzt ausführen | ✅ Ja |
| Nur 2-3 einfache Aktionen | ❌ Overkill |

---

## Zusammenfassung

| Konzept | Beschreibung |
|---------|--------------|
| **Command** | Kapselt einen Methodenaufruf als Objekt |
| **Invoker** | Löst Befehle aus (z.B. Toolbar, Menü, Button) |
| **Receiver** | Führt die eigentliche Arbeit aus |
| **Undo** | Jeder Command kann sich selbst rückgängig machen |

!!! tip "Merksatz"
    Das Command-Pattern verwandelt **Aktionen** in **Objekte** – dadurch werden sie speicherbar, weitergebar und rückgängig machbar.
