# Umsetzungshilfe – Story 7: Navigation nutzen

## Struktur
```
src/
  components/
    AppNavbar.tsx          # Navbar mit Navigation
    Layout.tsx             # Layout-Komponente mit Outlet
  pages/
    SearchPage.tsx
    FavoritesPage.tsx
    CityDetailPage.tsx
    AboutPage.tsx
  router/
    AppRouter.tsx          # Optional: zentraler Ort für Routen
```

## Router-Setup
- Definiere Routen wie `/`, `/favorites`, `/about` und `/city`.
- Verwende `BrowserRouter` (oder `HashRouter`, je nach Setup) im Wurzel-Tree.

## Exkurs: `NavLink` und aktive Styles
- `NavLink` liefert anhand der aktiven Route unterschiedliche Klassen.
- Nutze dies, um die aktive Seite visuell hervorzuheben (z. B. `active`-Klasse).

## Exkurs: Layout-Komponenten

### Warum Layout-Komponenten?
Ohne eine zentrale Layout-Komponente müsste jede einzelne Page-Komponente die Navbar und andere gemeinsame UI-Elemente selbst einbinden. Das führt zu:
- **Code-Duplikation** (Navbar-Code in jeder Page wiederholt)
- **Inkonsistenzen** (unterschiedliche Navbar-Implementierungen)
- **Schwere Wartbarkeit** (Änderungen müssen überall gemacht werden)

### Lösung: Layout-Komponente mit React Router Outlet
Eine Layout-Komponente umschließt alle Pages und definiert die gemeinsame Struktur (Header, Navbar, Footer, etc.). Der Page-spezifische Inhalt wird über das `<Outlet />`-Element von React Router dynamisch eingefügt.

**Allgemeines Beispiel:**
```typescript
// src/components/Layout.tsx
import { Outlet } from 'react-router-dom';
import { Navigation } from './Navigation';

export const Layout = () => {
  return (
    <div className="app-layout">
      <header>
        <h1>Meine App</h1>
      </header>
      
      <Navigation />
      
      <main className="content">
        {/* Outlet rendert die aktuelle Page-Komponente basierend auf der Route */}
        <Outlet />
      </main>
      
      <footer>
        <p>© 2025 Meine App</p>
      </footer>
    </div>
  );
};
```

**Router-Konfiguration mit verschachtelten Routes:**
```typescript
// src/App.tsx oder main.tsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import { Layout } from './components/Layout';
import { HomePage } from './pages/HomePage';
import { AboutPage } from './pages/AboutPage';
import { ContactPage } from './pages/ContactPage';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        {/* Layout als Parent-Route */}
        <Route path="/" element={<Layout />}>
          {/* Alle Child-Routes werden im <Outlet /> des Layouts gerendert */}
          <Route index element={<HomePage />} />
          <Route path="about" element={<AboutPage />} />
          <Route path="contact" element={<ContactPage />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}
```

**Wie es funktioniert:**
1. Das `Layout` wird für alle Child-Routes gerendert
2. Das `<Outlet />` im Layout wird durch die jeweilige Page-Komponente ersetzt
3. Bei `/` wird `<HomePage />` im Outlet gerendert
4. Bei `/about` wird `<AboutPage />` im Outlet gerendert
5. Header, Navigation und Footer bleiben **persistent** (werden nicht neu gemountet)

### Vorteile dieser Lösung:
- ✅ **Single Source of Truth**: Navbar, Header, Footer nur einmal definiert
- ✅ **Konsistenz**: Alle Pages haben die gleiche Grundstruktur
- ✅ **Einfache Wartung**: Änderungen nur an einer Stelle
- ✅ **Performance**: Gemeinsame Elemente werden nicht bei jedem Seitenwechsel neu gemountet
- ✅ **Saubere Trennung**: Page-Komponenten kümmern sich nur um ihren spezifischen Inhalt
