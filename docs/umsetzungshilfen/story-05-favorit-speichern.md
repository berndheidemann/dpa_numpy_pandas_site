# Umsetzungshilfe – Story 5: Stadt als Favorit speichern

## Struktur
```
src/
  context/
    FavoritesContext.tsx   # Globale Liste der Favoriten + Aktionen
  services/
    storage.ts             # Helpers für localStorage
  components/
    FavoriteToggle.tsx     # Herz-Button in der CityDetailPage
```

## Persistenz mit localStorage

### Was ist localStorage?
`localStorage` ist ein Browser-API, der es ermöglicht, Daten dauerhaft im Browser zu speichern. Die Daten bleiben auch nach dem Schließen des Browsers oder einem Neustart erhalten.

**Wichtige Eigenschaften:**
- Speichert nur **Strings** (daher JSON-Serialisierung nötig)
- Daten sind **pro Domain** isoliert
- Maximale Speichergröße: ca. 5-10 MB (je nach Browser)
- **Synchrone API** (blockiert kurz beim Lesen/Schreiben)
- Kein Ablaufdatum (bleibt bis zum manuellen Löschen)

### Grundlegende localStorage-Operationen

```typescript
// 1. Daten SPEICHERN (setItem)
localStorage.setItem('username', 'Max');

// Objekte müssen als JSON gespeichert werden
const user = { name: 'Max', age: 25 };
localStorage.setItem('user', JSON.stringify(user));

// 2. Daten LESEN (getItem)
const username = localStorage.getItem('username'); // "Max"

// JSON zurück zu Objekt parsen
const userString = localStorage.getItem('user');
const userObject = userString ? JSON.parse(userString) : null;

// 3. Daten LÖSCHEN (removeItem)
localStorage.removeItem('username');

// 4. ALLE Daten löschen (clear)
localStorage.clear();

// 5. Prüfen ob ein Key existiert
if (localStorage.getItem('username') !== null) {
  console.log('Username ist gespeichert');
}
```

### Best Practices für localStorage
- Verwende **Namespaces** für Keys (z. B. `weather-dashboard:favorites`) um Konflikte zu vermeiden
- **Fehlerbehandlung** mit try-catch beim JSON.parse()
- **Default-Werte** bereitstellen, falls nichts gespeichert ist
- Nur **relevante Daten** speichern (nicht zu viel, da Größenbeschränkung)

## UI
- Der Schalter befindet sich in der `CityDetailPage`.
- Visualisiere den Status deutlich (gefülltes vs. leeres Icon).

## Exkurs: Context für Favoriten
- `FavoritesContext` stellt `favorites` sowie Aktionen wie `add`, `remove` und `toggle` bereit.
- Komponenten können den Context konsumieren, anstatt Props weiterzureichen.

## Exkurs: Serialisierung und Datenkonsistenz
- Speichere nur relevante Felder (z. B. `id`, `name`, `country`, `latitude`, `longitude`).
- Arbeite bei Updates mit neuen Arrays, um Immutability in React zu respektieren.
- Fang JSON-Parsing-Fehler ab und nutze sinnvolle Defaults.
