# Die Welt von Zuul – Lose Kopplung durch Entwurf nach Zuständigkeiten

## Responsibility-Driven Design

**Entwurf nach Zuständigkeiten** ist ein Entwicklungsprozess, bei dem jeder Klasse eine klare Verantwortung zugewiesen wird. Dieser Prozess basiert auf der Idee, dass jede Klasse **für den Umgang mit ihren Daten verantwortlich** sein sollte.

Responsibility-driven design kann dazu benutzt werden, um festzulegen, welche Klasse für welche Funktionen in einer Anwendung zuständig sein soll. Wenn man stark nach Zuständigkeiten entwirft, beeinflusst dieses den Grad der Kopplung, was den Aufwand für Erweiterungen und Änderungen verringert.

!!! info "Grundprinzip"
    Wenn eine Klasse für bestimmte Daten verantwortlich ist, dann ist sie auch für den Umgang mit diesen Daten verantwortlich.

## Das Problem

Ziel dieser Übung ist nach wie vor die Reduzierung der Kopplung zwischen der Klasse `Room` und `Game`. Änderungen der Klasse `Room` sollen möglichst keinen Einfluss auf die Klasse `Game` haben.

Das geht allerdings immer noch besser: In der Klasse `Game` in der Methode `printRoomInformation()` ist immer noch die Ausgabe von Informationen über den entsprechenden Raum fest verdrahtet.

!!! question "Was passiert, wenn..."
    ...wir den Räumen neue Gegenstände wie Monster, Nahrung, Gegenstände, andere Spieler hinzufügen wollen? Wiederum wären Änderungen in der Klasse `Room` **und** der Klasse `Game` vonnöten.

Hier ist das **Prinzip des Entwurfs nach Zuständigkeiten** verletzt, weil die Klasse `Game` dafür zuständig gemacht worden ist, Informationen über den Raum zu liefern.

Die Klasse `Room` hält aber alle Informationen über einen Raum. Daher ist auch die Klasse `Room` dafür verantwortlich, diese Informationen zu liefern. Der Raum kann diese Aufgabe viel besser erfüllen als jedes andere Objekt, da er alle Details der internen Speicherung der Ausgänge kennt.

## Aufgaben

### Aufgabe 1 – Methode getLongDescription()

- [ ] Füge der Klasse `Room` eine Methode `getLongDescription()` hinzu, die eine lange Beschreibung des Raumes in der Form zurückgibt:
    ```
    You are on the market square
    Exits: north east west
    ```

### Aufgabe 2 – Anpassung in Game

- [ ] Ändere die Methode `printRoomInformation()` in der Klasse `Game` entsprechend ab

!!! success "Gewinn durch diese Methode"
    Diese lange Beschreibung eines Raums schließt die Beschreibung des Raums und dessen Ausgänge ein und kann bei späteren Erweiterungen um weitere Informationen ergänzt werden. **Betroffen von solchen Erweiterungen ist nur noch die Klasse `Room`.**

### Aufgabe 3 – Versionierung

- [ ] Versioniere deinen aktuellen Projektstand:
    ```bash
    git status
    git add -A
    git commit -m "zuul 4"
    ```
