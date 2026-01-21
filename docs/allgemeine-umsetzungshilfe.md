# Allgemeine Umsetzungshilfe

Diese Umsetzungshilfe beschreibt einen Fahrplan, wie du aus einer User Story Schritt für Schritt ein funktionierendes Feature entwickelst.

<!-- 
HINWEIS: Passe diese Datei an die Technologien deiner Lernsituation an.
Die folgenden Abschnitte sind Beispiele und sollten entsprechend 
deinem Technologie-Stack angepasst werden.
-->

## 0. Installation und Setup

Bevor du mit der Umsetzung der User Stories beginnst, musst du dein Projekt aufsetzen.

### Projekt erstellen

<!-- Hier die spezifischen Setup-Anweisungen für dein Projekt einfügen -->

```bash
# Beispiel: Neues Projekt erstellen
# npm create vite@latest projekt-name -- --template react-ts
```

### Dependencies installieren

<!-- Abhängigkeiten für dein Projekt auflisten -->

```bash
# Basis-Dependencies installieren
npm install
```

### Development Server starten

```bash
npm run dev
```

## 1. Von der Story zum Mock
- Starte jede Story mit einem UI-Mock (Platzhalter oder Dummy-Daten reichen).
- Ziel: Oberfläche sichtbar machen, bevor Logik, Datenanbindung und Styling folgen.
- Ergänze das Feature anschließend iterativ:
  1. State hinzufügen
  2. API-Calls ergänzen
  3. Lade- und Fehlerzustände gestalten
  4. Styling verfeinern

## 2. Code-Strukturierung

<!-- Passe die folgenden Kategorien an deinen Technologie-Stack an -->

### Page / View
- Entspricht einer Route oder Hauptansicht.
- Enthält das große Ganze einer Funktionalität und orchestriert Datenflüsse.
- Frage: Braucht der Teil eine eigene URL? Wenn ja, bau eine Page.

### Component
- Wiederverwendbarer UI-Baustein ohne globale Logik.
- Bekommt Daten und Callbacks über Props.
- Frage: Wird der Teil mehrfach oder an verschiedenen Stellen benötigt? Wenn ja, baue eine Komponente.

### Service
- Kapselt externe Zugriffe wie API- oder Storage-Zugriffe.
- Hält UI-Komponenten fokussiert.
- Frage: Greifst du auf externe Daten oder Storage zu? Wenn ja, baue einen Service.

## 3. Vorgehensweise pro Story
1. Story lesen und UI grob skizzieren (Mock).
2. Entscheiden, welche Komponenten und Services nötig sind.
3. Dateien und Boilerplate anlegen.
4. Dummy-Daten einsetzen, um das UI sichtbar zu machen.
5. Funktionalität iterativ ergänzen:
   - State anlegen
   - API- oder Storage-Anbindung herstellen
   - Lade- und Fehlerzustände umsetzen
6. Akzeptanzkriterien als Checkliste prüfen.

## 4. Tipps und Best Practices
- Klein anfangen, dann erweitern.
- Klare Ordnerstruktur einhalten.
- Verantwortlichkeiten trennen.
- Iterativ arbeiten: Jede Story liefert ein Feature, am Ende wächst alles zusammen.
