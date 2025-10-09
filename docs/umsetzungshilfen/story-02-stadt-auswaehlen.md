# Umsetzungshilfe – Story 2: Stadt auswählen

## Projektstruktur
```
src/
  pages/
    SearchPage.tsx        # Trefferliste mit klickbaren Einträgen
    CityDetailPage.tsx    # Detailseite mit allen Daten
  components/
    SearchResultItem.tsx  # Einzelner Treffer (klickbar)
    CityHeader.tsx        # Stadtname prominent dargestellt
    CityInfo.tsx          # Metadaten (Land, Einwohner, Koordinaten, Höhe)
  context/
    CityContext.tsx  # Globale Auswahl der Stadt
```

## Navigation mit React Router
- Stelle sicher, dass React Router eingerichtet ist.
- Definiere eine Route `/city` für die Detailseite.
- **Wichtig:** Die Geocoding-API unterstützt keine ID-basierte Suche, daher nutzen wir keine dynamischen Parameter wie `/city/:id`.
- Verwende `useNavigate()`, um bei einem Klick auf ein Suchergebnis zur Detailseite zu navigieren: `navigate('/city')`.

## State-Management für die aktuelle Stadt
- Implementiere `CityContext`, der die ausgewählte Stadt global hält.
- Der Context stellt sowohl `selectedCity` (aktueller Wert) als auch `setSelectedCity` (Setter-Funktion) bereit.
- Vorteil: Auch weitere Stories (Favoriten, Diagramme) können später darauf zugreifen.

**Wichtiger Hinweis:** Bei dieser Lösung gehen die Stadt-Daten verloren, wenn die Seite neu geladen wird oder die `/city`-URL direkt aufgerufen wird. Das ist für diese Story akzeptabel - in späteren Stories kann localStorage als Backup ergänzt werden.

## Exkurs: React Context
- Context vermeidet "Props-Drilling" quer durch viele Ebenen.
- Erstelle mit `createContext` einen Provider, der `selectedCity` und `setSelectedCity` anbietet.
- Komponenten wie `SearchPage` setzen den Wert, `CityDetailPage` liest ihn über `useContext` aus.

### Allgemeines Beispiel: Context erstellen und verwenden

```typescript
// 1. Context erstellen (z.B. ThemeContext.tsx)
import { createContext, useContext, useState, ReactNode } from 'react';

// Type für den Context
type ThemeContextType = {
  theme: string;
  setTheme: (theme: string) => void;
};

// Context erstellen (mit undefined als Default)
const ThemeContext = createContext<ThemeContextType | undefined>(undefined);

// Provider-Komponente: Umschließt die App und stellt Werte bereit
export const ThemeProvider = ({ children }: { children: ReactNode }) => {
  const [theme, setTheme] = useState('light');

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

// Custom Hook für einfacheren Zugriff
export const useTheme = () => {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme muss innerhalb von ThemeProvider verwendet werden');
  }
  return context;
};
```

```typescript
// 2. Provider in der App einbinden (z.B. in main.tsx oder App.tsx)
<ThemeProvider>
  <App />
</ThemeProvider>
```

```typescript
// 3. Context in Komponenten verwenden
const Header = () => {
  const { theme, setTheme } = useTheme();
  
  return (
    <div>
      Aktuelles Theme: {theme}
      <button onClick={() => setTheme('dark')}>Dark Mode</button>
    </div>
  );
};
```

## Komponentenaufteilung
- `SearchPage` zeigt die Liste der Treffer und delegiert die Darstellung an `SearchResultItem`.
- `SearchResultItem` löst beim Klick die Navigation aus und übergibt die Stadt an den Context.
- `CityDetailPage` liest die Daten aus dem Context und zeigt Name, Land, Einwohnerzahl, Koordinaten und Höhe über NN an.
