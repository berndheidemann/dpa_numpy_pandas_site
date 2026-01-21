# Die Welt von Zuul – Implizite Kopplung

## Lernziele

Nach Bearbeitung dieses Arbeitsblatts kannst du:

- Implizite Kopplung erkennen und erklären
- Befehle in getrennten Klassen verwalten (hohe Kohäsion)
- Abhängigkeiten durch geeignete Methoden auflösen

---

## Neuer Befehl: look

Es soll der Befehl `look` in den Satz der gültigen Befehle aufgenommen werden. `look` gibt die Beschreibung des Raums und seiner Ausgänge nochmals aus.

Dieser Befehl kann für den Spieler wichtig werden, wenn schon einige Aktionen im Raum getätigt wurden und die Beschreibung des Raums auf der Konsole nicht mehr zu sehen ist.

Die Stelle, an der der neue Befehl hinzugefügt werden muss, sticht unmittelbar ins Auge: die Klasse `CommandWords`.

!!! success "Gutes Beispiel für hohe Kohäsion"
    Statt die Befehlswörter in der Klasse `Parser` zu definieren, was durchaus passend wäre, aber der Klasse `Parser` neben der Interpretation der Benutzereingaben die zusätzliche Aufgabe der Verwaltung der gültigen Befehle aufhalst, geschieht letzteres in einer separaten Klasse.

## Aufgaben – Teil 1

- [ ] Füge den Befehl `look` dem Array in der Klasse `CommandWords` hinzu

- [ ] Füge der Klasse `Game` eine private Methode `look()` hinzu, die eine lange Beschreibung des aktuellen Raums auf der Konsole ausgibt

- [ ] Füge in der Klasse `Game` in der Methode `processCommand()` eine weitere Fallunterscheidung hinzu, damit der Befehl `look` verarbeitet werden kann

- [ ] Teste den neuen Befehl `look` und rufe den Befehl `help` auf

!!! question "Was fällt auf?"
    Die Ausgabe der zur Verfügung stehenden Befehle ist falsch: `look` ist nicht aufgeführt!

## Das Problem: Implizite Kopplung

Die Kopplung zwischen `Game`, `Parser` und `CommandWords` schien bisher in Ordnung zu sein. Die Änderung konnte recht leicht umgesetzt werden.

Beim Testen des Befehls `help` fällt jedoch die **falsche Ausgabe** auf:

```
Your command words are: go quit help
```

Das kann zwar leicht in der Methode `printHelp()` in `Game` geändert werden, ist aber ein generelles Problem.

!!! danger "Implizite Kopplung"
    Bei der Einführung neuer Befehle muss jedes Mal die Methode `printHelp()` angepasst werden, was leicht übersehen wird.

    So etwas nennt man **implizite Kopplung**: Wenn sich die Befehle ändern, muss der Hilfstext angepasst werden (Kopplung), aber im Quellcode gibt es keinen Hinweis darauf, dass diese Abhängigkeit besteht (deshalb implizit).

!!! warning "Besonders unangenehm"
    Implizite Kopplung ist eine besonders unangenehme Form der Kopplung. Fehler sind **nicht durch Compilermeldung zu erkennen**, was dazu führt, dass notwendige Änderungen unentdeckt bleiben und mögliche Fehler erst bei der Benutzung des Programms auftreten.

In diesem Fall stützt sich die Methode `printHelp()` in der Klasse `Game` auf interne Informationen der Klasse `CommandWords`, was eine **Verletzung des Entwurfs nach Zuständigkeiten** darstellt.

## Aufgaben – Teil 2

- [ ] Schreibe in der Klasse `CommandWords` eine Methode `showAll()`, die alle gültigen Befehle als String zurückgibt. Verwende zum Zusammenbauen des Ergebnisstrings wieder die Klasse `StringBuilder`

- [ ] Schreibe im `Parser` eine Methode `public String showCommands()`, die die entsprechende Methode in `CommandWords` aufruft

    !!! note "Hinweis"
        Der Parser leitet die Anfrage nach den gültigen Befehlen einfach an `CommandWords` weiter, was typisch für die Kompositionsbeziehung zwischen beiden Klassen ist.

- [ ] Ändere die Methode `printHelp()` der Klasse `Game` entsprechend ab

- [ ] Versioniere deinen aktuellen Projektstand:
    ```bash
    git status
    git add -A
    git commit -m "zuul 5"
    ```

---

## Zusammenfassung

!!! success "Das hast du gelernt"
    - **Implizite Kopplung**: Versteckte Abhängigkeiten ohne Compiler-Warnung
    - Die Klasse `CommandWords` verwaltet gültige Befehle zentral
    - **Hohe Kohäsion**: Jede Klasse hat eine klare Aufgabe
    - Der Parser delegiert Anfragen an die zuständige Klasse

---

??? question "Selbstkontrolle"
    1. Warum ist implizite Kopplung besonders problematisch?
    2. Warum ist es gut, dass `CommandWords` eine eigene Klasse ist?
    3. Was bedeutet "Delegation" im Kontext von Parser und CommandWords?
    
    ??? success "Antworten"
        1. Fehler werden nicht vom Compiler erkannt und treten erst zur Laufzeit auf
        2. Hohe Kohäsion – die Befehlsverwaltung ist getrennt von der Eingabeverarbeitung
        3. Der Parser ruft Methoden von CommandWords auf, statt die Logik selbst zu implementieren
