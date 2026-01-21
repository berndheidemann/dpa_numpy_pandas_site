# Die Klasse StringBuilder

Für den Umgang mit Zeichenketten stellt Java mehrere Klassen zur Verfügung. Die Klasse `String` hast du bereits kennengelernt. Sie dient dazu, Zeichenketten-Konstanten darzustellen.

## Das Problem mit String

Die Klasse `String` ist eine **immutable class**, d.h. die aus ihr erstellten Objekte können nachträglich nicht mehr verändert werden.

!!! warning "Performance-Problem"
    Werden manipulierende Methoden der Klasse `String` aufgerufen, z.B. zum Einfügen oder Zerlegen einer Zeichenkette, erzeugt Java immer **neue Zeichenkettenobjekte**, was Laufzeit kostet.

Daher sollte die Klasse `String` nicht benutzt werden, wenn man Zeichenketten nach und nach aufbauen oder häufiger verändern will.

## StringBuilder als Lösung

Für die Darstellung von **veränderbaren Zeichenketten** wird die Klasse `StringBuilder` verwendet:

- Dem Konstruktor kann optional ein Anfangswert übergeben werden
- `StringBuilder` stellt Methoden zum Einfügen und Anhängen zur Verfügung
- Die Größe der Zeichenkette wird dynamisch den Erfordernissen angepasst

## Beispiel: Das ABC aufbauen

Im folgenden Programm wird das ABC als Zeichenkette über eine Schleife aufgebaut:

### Schlechte Lösung (mit String)

```java
String abc = "";
for (char letter = 'a'; letter <= 'z'; letter++) {
    abc += letter;
}
```

!!! danger "Problem"
    Diese Lösung erzeugt in der Schleife **26 temporäre Stringobjekte**, die der GarbageCollector entsorgen muss.

### Gute Lösung (mit StringBuilder)

```java
StringBuilder abc = new StringBuilder("");
for (char letter = 'a'; letter <= 'z'; letter++) {
    abc.append(letter);
}
String s = abc.toString();
```

!!! success "Vorteil"
    Diese Lösung ist **performanter** und daher vorzuziehen.

## Wichtige Methoden

| Methode | Beschreibung |
|---------|--------------|
| `public StringBuilder append(String str)` | Hängt eine Zeichenkette `str` an das Ende der vorhandenen Zeichenkette an. |
| `public StringBuilder insert(int offset, String str)` | Fügt eine Zeichenkette `str` an der Stelle `offset` ein. |
| `public void setCharAt(int index, char ch)` | Ersetzt das Zeichen an der Position `index` durch das Zeichen `ch`. |
| `public int length()` | Liefert die Länge der aktuellen Zeichenkette. |
| `public String toString()` | Erzeugt aus der aktuellen Zeichenkette ein String-Objekt und gibt es zurück. |
| `public String substring(int start, int end)` | Erzeugt aus den Zeichen von `start` bis `end` eine neue Zeichenkette und gibt sie als String zurück. |
| `public StringBuilder deleteCharAt(int index)` | Löscht das Zeichen an der Stelle `index` der Zeichenkette. |
| `public StringBuilder reverse()` | Dreht die Zeichenkette um. |
