# Story 1 – Stadt suchen

**Als** Besucher **möchte ich** nach Städten suchen **damit** ich Wetterdaten für eine gewünschte Stadt ansehen kann.

## Akzeptanzkriterien
- Gegeben ich tippe weniger als 3 Zeichen, dann erhalte ich den Hinweis "Bitte mindestens 3 Zeichen eingeben".
- Gegeben ich tippe mindestens 3 Zeichen und es gibt Treffer, dann werden maximal 5 Ergebnisse mit Name, Land und Einwohnerzahl angezeigt.
- Gegeben während des Ladens, dann sehe ich einen Ladehinweis.
- Gegeben ein Fehler tritt auf, dann erhalte ich eine verständliche Fehlermeldung und keine Ergebnisse.

## Tasks
- [ ] TypeScript-Typen für City-Daten und API-Response definieren
- [ ] Geocoding-Service mit API-Anbindung erstellen (fetch zur Open-Meteo API: https://open-meteo.com/en/docs/geocoding-api)
- [ ] Custom Hook `useDebounce` für verzögerte Eingabe implementieren (500ms delay)
- [ ] Custom Hook `useCitySearch` für API-Calls und State-Management erstellen
- [ ] Komponente `SearchInput` mit Controlled Input und 3-Zeichen-Validierung bauen
- [ ] Komponente `SearchResults` für Ergebnisliste (max. 5 Treffer) erstellen
- [ ] Komponente `SearchResultItem` für einzelnen Treffer (Name, Land, Einwohnerzahl) bauen
- [ ] SearchPage als Container zusammenbauen und alle Komponenten integrieren
- [ ] Lade-Indikator während API-Request anzeigen
- [ ] Fehlerbehandlung und benutzerfreundliche Fehlermeldungen implementieren

## Umsetzungshilfe
- [Umsetzungshilfe – Story 1: Stadt suchen](../umsetzungshilfen/story-01-stadt-suchen.md)
