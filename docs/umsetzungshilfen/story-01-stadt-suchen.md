# Umsetzungshilfe – Story 1: Stadt suchen

## Projektstruktur
```
src/
  pages/
    SearchPage.tsx        # Seite: Suchfeld + Ergebnisliste
  components/
    SearchInput.tsx       # Controlled Input mit onChange
    SearchResults.tsx     # Liste (max. 5) + Leermeldungen
    SearchResultItem.tsx  # Einzelner Treffer
  hooks/
    useDebounce.ts        # Debounce für searchTerm
    useCitySearch.ts      # API-Aufruf für debouncedTerm
  services/
    geocodingService.ts   # Fetch-Funktion zur Open-Meteo API
  types/
    citySearchResult.ts   # Typdefinition für die API-Response
```

## Datenquelle (Geocoding)
- Nutze die Open-Meteo-Geocoding-API: `https://geocoding-api.open-meteo.com/v1/search`.
- Parameter: `name`, `count` (z. B. 5), `language=de`, `format=json`.
- Relevante Felder: `name`, `country`, `population`, `id`, sowie Koordinaten.

## Eingabefeld
- Verwende ein controlled Input mit `onChange`.
- Der `onChange`-Handler aktualisiert `searchTerm` und reicht ihn an die Elternkomponente weiter.
- Solange `searchTerm` kürzer als 3 Zeichen ist, zeige nur den Hinweis und starte keinen Request.

## Ergebnisliste
- Zeige maximal fünf Treffer mit Stadtname, Land und Einwohnerzahl.
- Nutze `SearchResults` als Container und `SearchResultItem` für eine einzelne Zeile.
- Stelle sicher, dass die Liste leere Ergebnisse sinnvoll abbildet (z. B. "Keine Ergebnisse gefunden").

## Lade- und Fehlermeldungen
Halte in `SearchPage.tsx` folgende States vor:
- `searchTerm`: aktueller Eingabetext
- `debouncedTerm`: verzögerter Wert aus `useDebounce`
- `results`: Trefferliste
- `isLoading`: Ladezustand
- `errorMessage`: Fehlerhinweis

Der Hook `useCitySearch` sollte API-Aufrufe kapseln und States aktualisieren. Die Seite koordiniert `searchTerm`, `debouncedTerm` und ruft `searchCities(...)` nur auf, wenn `debouncedTerm` mindestens drei Zeichen hat.

### Exkurs: Custom Hooks
Custom Hooks sind Funktionen, die mit `use` beginnen und andere Hooks verwenden können. Sie helfen dabei, Logik zwischen Komponenten wiederzuverwenden.

**Allgemeines Beispiel: useLocalStorage Hook**
```typescript
import { useState, useEffect } from 'react';

/**
 * Custom Hook zum Speichern und Laden von Daten in localStorage
 * @param key - Schlüssel für localStorage
 * @param initialValue - Startwert, falls nichts gespeichert ist
 */
export function useLocalStorage<T>(key: string, initialValue: T) {
  // State mit Wert aus localStorage initialisieren (falls vorhanden)
  const [value, setValue] = useState<T>(() => {
    const stored = localStorage.getItem(key);
    return stored ? JSON.parse(stored) : initialValue;
  });

  // Bei Änderung des Wertes in localStorage speichern
  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);

  return [value, setValue] as const;
}
```

**Verwendung in einer Komponente:**
```typescript
const UserSettings = () => {
  // Wie useState, aber automatisch in localStorage gespeichert
  const [name, setName] = useLocalStorage('userName', 'Gast');
  const [theme, setTheme] = useLocalStorage('theme', 'light');

  return (
    <div>
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Theme wechseln
      </button>
    </div>
  );
};
```

**Vorteile von Custom Hooks:**
- Wiederverwendbare Logik (ein Hook in vielen Komponenten nutzen)
- Trennung von UI und Geschäftslogik
- Bessere Testbarkeit
- Sauberer, lesbarer Code

## Performance per Debounce
- Debounce bedeutet: Warte z. B. 500 Millisekunden, nachdem der Nutzer aufgehört hat zu tippen, bevor du die API anfragst.
- Der Hook `useDebounce` nimmt `searchTerm` entgegen und liefert eine verzögerte Version zurück.
- So verhinderst du unnötige Requests bei schnellem Tippen.

### Codebeispiel: useDebounce Hook

```typescript
import { useEffect, useState } from 'react';

/**
 * Custom Hook für Debouncing eines Wertes
 * @param value - Der zu debouncende Wert
 * @param delay - Verzögerung in Millisekunden (Standard: 500ms)
 * @returns Der verzögerte Wert
 */
export function useDebounce<T>(value: T, delay: number = 500): T {
  const [debouncedValue, setDebouncedValue] = useState<T>(value);

  useEffect(() => {
    // Setze einen Timer, der nach 'delay' Millisekunden den Wert aktualisiert
    const timer = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    // Cleanup: Lösche den Timer, wenn sich der Wert ändert (neuer Tastendruck)
    // oder die Komponente unmountet
    return () => {
      clearTimeout(timer);
    };
  }, [value, delay]);

  return debouncedValue;
}
```

**Verwendung:**
```typescript
const SearchPage = () => {
  const [searchTerm, setSearchTerm] = useState('');
  const debouncedSearchTerm = useDebounce(searchTerm, 500);

  // debouncedSearchTerm ändert sich erst 500ms nach dem letzten Tastendruck
  useEffect(() => {
    if (debouncedSearchTerm.length >= 3) {
      // Jetzt API-Call durchführen
      searchCities(debouncedSearchTerm);
    }
  }, [debouncedSearchTerm]);

  return (
    <input 
      value={searchTerm} 
      onChange={(e) => setSearchTerm(e.target.value)} 
    />
  );
};
```

