# Die Welt von Zuul – Kohäsion von Klassen

## Wiederholung: Kohäsion von Methoden

Die **Kohäsion von Methoden** war bereits auf dem ersten Arbeitsblatt Thema. Eine Methode weist einen hohen Grad an Kohäsion auf, wenn sie nur für **eine** wohl definierte Aufgabe zuständig ist.

Dies gilt beispielsweise für die Methode `printWelcome()` in der Klasse `Game`, die für die Darstellung des Eröffnungstextes zuständig ist. Diese Methode wird in der Methode `play()` aufgerufen.

!!! tip "Vorteile hoher Kohäsion"
    - Leicht verständlicher Quellcode
    - Leicht änderbar
    - Kurze, verständliche Methoden, deren Namen ihre Funktion deutlich wiedergeben

## Kohäsion von Klassen

Das Konzept der Kohäsion gilt ebenso für **Klassen**. Eine Klasse weist einen hohen Grad an Kohäsion auf, wenn sie genau **eine** wohl definierte Einheit aus dem Anwendungsbereich modelliert, also für einen ganz bestimmten Aufgabenbereich zuständig ist.

Das Beispiel vom letzten Aufgabenblatt war die Klasse `CommandWords`, die man auch hätte weglassen und die entsprechenden Attribute und Methoden in der Klasse `Parser` unterbringen können. Dies hätte jedoch eine schlechte Wiederverwendbarkeit sowie einen deutlich schwierigeren Quellcode zur Folge!

## Neue Erweiterung: Gegenstände

Das Ziel der neuen Erweiterung besteht in der Einführung von **Gegenständen**:

- Jeder Raum kann einen Gegenstand enthalten.
- Jeder Gegenstand hat eine **Beschreibung** und ein **Gewicht**.
- Das Gewicht entscheidet später darüber, ob der Gegenstand vom Spieler aufgehoben werden kann oder nicht.

### Naiver Ansatz (nicht empfohlen!)

Naiv, aber durchaus möglich, wäre es, zwei neue Datenfelder in der Klasse `Room` zu deklarieren.

!!! danger "Probleme"
    - Die Klasse würde sowohl einen Raum als auch einen Gegenstand beschreiben und modellieren → **geringe Kohäsion**
    - Der Gegenstand wäre an einen bestimmten Raum gebunden → **verhindert spätere Erweiterungen** wie Aufnahme und Ablage

### Besserer Entwurf

Ein besserer Entwurf mit hohem Grad an Kohäsion wäre die Einführung einer **neuen Klasse `Item`** mit den Attributen `name`, `description` und `weight`.

Die Klasse `Room` bekommt dann eine Referenz auf ein Gegenstandsobjekt.

## Aufgaben

### Aufgabe 1 – Klasse Item erstellen

- [ ] Erstelle eine Klasse `Item` mit den Attributen `name`, `description`, `weight`
- [ ] Erstelle einen Konstruktor, der die Attribute als Parameter mitgegeben bekommt
- [ ] Erstelle eine `toString()`-Methode, die die Attribute als String zurückgibt

### Aufgabe 2 – Room anpassen

- [ ] Modifiziere das Projekt so, dass ein Raumobjekt **beliebig viele** Gegenstände aufnehmen kann

!!! tip "Datenstruktur"
    Verwende dafür eine `Map`, bei der die Namen der Gegenstände als Schlüssel dienen. Geeignet wäre auch eine Liste oder ein Set, allerdings ist es für das spätere Entnehmen der Gegenstände aus dem Raum günstig, auf die Gegenstände mittels ihres Namens zuzugreifen.

### Aufgabe 3 – Methode putItem()

- [ ] Füge der Klasse `Room` eine Methode `void putItem(Item newItem)` hinzu, mit der ein Gegenstand in einem Raum abgelegt werden kann

!!! note "Hinweis"
    Dafür benötigst du einen Getter für den Namen des Gegenstandes!

### Aufgabe 4 – Anzeige der Gegenstände

- [ ] Sorge in der Klasse `Room` dafür, dass alle Gegenstände angezeigt werden, wenn die lange Raumbeschreibung abgerufen wird. Beachte dabei das Prinzip der Kohäsion!

Die Ausgabe sollte ungefähr so aussehen:

```
You are in the tavern at the market square
Exits: east south
Items in this room:
 - beer stein, a delicious pils in a fine stein, 1kg
 - plate, a plate with a roast wild boar and gravy, 1kg
```

### Aufgabe 5 – Gegenstände platzieren

In der Methode `createRooms()` der Klasse `Game` ist der richtige Platz, bestimmte Gegenstände in bestimmten Räumen abzulegen. Füge folgenden Räumen die folgenden Gegenstände hinzu:

| Raum | Name | Beschreibung | Gewicht |
|------|------|--------------|---------|
| Marktplatz | bow | a bow made of wood | 0.5 |
| Höhle | treasure | a small treasure chest with coins | 7.5 |
| Zaubererzimmer | arrows | a quiver with various arrows | 1 |
| Dschungel | herb | a healing herb | 0.5 |
| Dschungel | cacao | a small cacao tree | 5 |
| Opferstätte | knife | a very sharp, large knife | 1 |
| Wohnhütte | spear | a spear with a thrower | 5.0 |
| Taverne | food | a plate with hearty meat and corn mash | 0.5 |
| Keller | jewelry | a very pretty headdress | 1 |

- [ ] Platziere alle Gegenstände in den entsprechenden Räumen
- [ ] Teste dein Programm

!!! success "Beachte"
    Die Gegenstände werden auch beim Willkommenstext schon angezeigt, obwohl du keine Änderung an der Methode `printWelcome()` vorgenommen hast. Dies ist ein Ergebnis der **besseren Kohäsion** deines Programms!

### Aufgabe 6 – Versionierung

- [ ] Versioniere deinen aktuellen Projektstand:
    ```bash
    git status
    git add -A
    git commit -m "zuul 6"
    git log --oneline
    ```
