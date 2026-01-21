# HashMap

## Das Konzept einer Map

Eine **Map** ist eine Sammlung von **Schlüssel-Wert-Paaren**. Sie ist damit wie ein Telefonbuch oder ein Wörterbuch aufgebaut.

!!! example "Beispiel: Telefonbuch"
    Ein Telefonbuch enthält Einträge, die Paare aus einem **Namen** und einer **Telefonnummer** sind. Es wird benutzt, indem ein Name nachgeschlagen und die zugeordnete Nummer verwendet wird. Ein Name dient dabei als **Schlüssel**, der mit einer Telefonnummer, dem **Wert**, verknüpft ist.

Ein Eintrag in einer Map besteht daher immer aus einem Objektpaar:

- **Schlüssel-Objekt** (Key)
- **Wert-Objekt** (Value)

Um auf ein Wert-Objekt zuzugreifen, wird also nicht etwa – wie bei einer `ArrayList` – ein Index verwendet. Stattdessen wird der **Schlüssel** verwendet, um das Wert-Objekt zu erhalten.

!!! info "Abbildung"
    Die deutsche Bezeichnung für dieses Konzept heißt **Abbildung**: Ein Schlüssel wird auf einen Wert abgebildet.

### Eigenschaften einer Map

- Beliebig viele Objekt-Paare können hinzugefügt oder entfernt werden
- Jeder Schlüssel darf in einer Map nur **genau einmal** vorhanden sein
- Beim Einfügen eines bereits vorhandenen Schlüssels wird der alte Wert durch den neuen ersetzt
- Die Reihenfolge der Daten ist nicht spezifiziert

!!! warning "Einseitig gerichtete Suche"
    Die umgekehrte Suche (anhand einer Telefonnummer den Namen finden) ist mit einer Map nicht so einfach. Abbildungen sind ideal für **einseitig gerichtete Suchen**, bei der der Schlüssel bekannt ist.

## Funktionsweise einer HashMap

Zwei wichtige Implementierungen von Maps sind:

| Implementierung | Eigenschaften |
|-----------------|---------------|
| `HashMap` | Ideal für viele Elemente unsortiert; schneller Zugriff durch Hashing; keine Sortierung möglich |
| `TreeMap` | Etwas langsamer im Zugriff; Elemente liegen sortiert vor (interner Binärbaum) |

Bei `HashMap` müssen die Schlüsselobjekte „hashbar" sein, also `equals()` und `hashCode()` implementieren:

- `equals()` wird beim Eintragen und Zugriff benutzt, um die Gleichheit von Schlüsselobjekten festzustellen
- `hashCode()` wird intern verwendet, um den Speicherplatz in der Tabelle herauszusuchen

## Grundlegende Methoden: put und get

Die wichtigsten Methoden einer Map sind:

- `put` – fügt der Map ein Schlüssel-Wert-Paar hinzu
- `get` – liefert für einen gegebenen Schlüssel einen Wert

### Beispiel: Telefonbuch

```java
import java.util.HashMap;

HashMap<String, String> telephonBook = new HashMap<String, String>();
telephonBook.put("Meier, Günter", "(0421) 3924587");

// Später...
String number = telephonBook.get("Meier, Günter");
```

!!! note "Hinweis"
    Für das Telefonbuch wird `String` sowohl als Schlüsseltyp als auch als Werttyp verwendet. Im Allgemeinen müssen die Typen nicht gleich sein. Es müssen lediglich **Klassentypen** sein. Die einfachen Datentypen sind keine Objekte und können daher in eine Map nicht aufgenommen werden. Stattdessen müssen die jeweiligen **Wrapper-Klassen** verwendet werden.

## Eine Map durchlaufen

Um eine Map vollständig zu durchlaufen, eignet sich die **for-each-Schleife**:

```java
for (String key : telephonBook.keySet()) {
    System.out.println(key + ": " + telephonBook.get(key));
}
```

Die Methode `keySet()` der Klasse `HashMap` liefert ein `Set` mit allen Schlüsselwerten. Das Set wird mittels for-each-Schleife durchlaufen, um die Schlüsselwerte nacheinander auszugeben.

## Wichtige Methoden der Klasse HashMap

| Methode | Beschreibung |
|---------|--------------|
| `public void clear()` | Entfernt alle Einträge der HashMap |
| `public boolean containsKey(Object key)` | Liefert `true`, wenn `key` als Schlüssel in der HashMap enthalten ist |
| `public boolean containsValue(Object value)` | Liefert `true`, wenn die Map ein oder mehrere Schlüssel zum übergebenen Wert speichert |
| `public boolean isEmpty()` | Liefert `true`, wenn die Map leer ist |
| `public V get(Object key)` | Liefert den Wert zum Schlüssel `key` |
| `public Set keySet()` | Liefert ein Set mit allen Schlüsseln der Map |
| `public V remove(Object key)` | Löscht das Schlüssel-Wert-Paar zum übergebenen Schlüssel und gibt es zurück |
| `public V put(K key, V value)` | Fügt das Schlüssel-Wert-Paar ein. Falls der Schlüssel bereits vorhanden ist, wird der alte Wert zurückgegeben, sonst `null` |
| `public int size()` | Liefert die Anzahl der Einträge der HashMap |
