# Die Welt von Zuul – Enge Kopplung wegen fehlender Kapselung

## Das Problem

Ich hoffe, dass dich die Erweiterung der Anwendung um die Befehle `up` und `down` vom letzten Arbeitsblatt nicht in den Wahnsinn getrieben hat!

Es mussten allen Ernstes Änderungen in:

- der Klasse `Room` (neue Ausgänge als Instanzvariablen, Änderung in `setExits()`)
- der Klasse `Game` (Methoden `createRooms()`, `printRoomInformation()` und `goRoom()`)

vorgenommen werden.

!!! danger "Enge Kopplung"
    Änderungen an so vielen Stellen verweisen auf **enge Kopplung** und einen **schlechten Softwareentwurf**! Eine lose Kopplung hätte vorgelegen, wenn wir nur die Klasse `Room` hätten anfassen müssen.

## Ursache: Fehlende Kapselung

Der Grund für die enge Kopplung liegt daran, dass die Klasse `Room` das **Prinzip der Kapselung** verletzt.

Dadurch, dass die Klasse `Room` ihre Datenfelder **öffentlich** anbietet, hat sie nicht nur den Umstand offengelegt, dass sie über Ausgänge verfügt, sondern auch **wie** diese Ausgänge implementiert sind.

!!! info "Prinzip der Kapselung"
    Es sollen nur Informationen über das **WAS** einer Klasse (was leistet sie?) nach außen sichtbar sein, nicht aber das **WIE** (ihre Realisierung).

Wenn die Klasse `Room` so schlecht entworfen ist, ist es kein Wunder, dass die Klasse `Game` die konkrete Implementierung der Klasse `Room` benutzt. So greift sie z.B. direkt auf die öffentlichen Eigenschaften der Klasse `Room` zu!

Im Folgenden soll daher die enge Kopplung der Klassen `Game` und `Room` aufgelöst werden. Wenn das realisiert ist, können wir die Implementierung der Klasse `Room` ändern, ohne dass andere Klassen davon beeinträchtigt werden.

## Aufgaben

### Änderungen in der Klasse Room

- [ ] Kapsle die Attribute der Klasse `Room`, indem du den Zugriffsmodifizierer `private` verwendest

- [ ] Schreibe eine Methode `public Room getExit(String direction)`, der die Richtung als String (`north`, `south`, etc.) übergeben wird und die den entsprechenden Raum zurückgibt

- [ ] Schreibe eine Methode `exitsToString()`, die alle vorhandenen Ausgänge als String zurückgibt. Ein Ausgang ist vorhanden, wenn er nicht `null` ist!

!!! tip "StringBuilder verwenden"
    Lies dafür das [Infoblatt StringBuilder](../infoblaetter/stringbuilder.md) und verwende die Klasse `StringBuilder` zum Zusammenbauen des Strings.

### Änderungen in der Klasse Game

Ein direkter Zugriff auf die Instanzvariablen der Klasse `Room` ist nun nicht mehr möglich.

- [ ] Ersetze in der Methode `goRoom()` die if-Anweisungen durch Aufruf der Methode `getExit()`

- [ ] Ersetze in der Methode `printRoomInformation()` die if-Anweisungen durch Aufruf der Methode `exitsToString()`

- [ ] Teste dein Programm!

### Versionierung

- [ ] Versioniere deinen aktuellen Projektstand:
    ```bash
    git status
    git add -A
    git commit -m "zuul 2"
    ```

- [ ] Lasse dir die Versionshistorie anzeigen:
    ```bash
    git log --oneline
    ```
