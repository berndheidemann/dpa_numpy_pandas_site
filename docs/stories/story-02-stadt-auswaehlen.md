# Story 2 – Stadt auswählen

**Als** Besucher **möchte ich** eine Stadt aus der Trefferliste auswählen **damit** deren Wetterdaten angezeigt werden.

## Akzeptanzkriterien
- Ein Klick auf einen Treffer führt zur Detailseite.
- Auf der Detailseite wird der Stadtname prominent angezeigt.
- Auf der Detailseite werden Land, Einwohnerzahl, Breiten- und Längengrad sowie die Höhe über NN dargestellt.

## Tasks
- [ ] TypeScript-Interface für City-Details erweitern (name, country, population, latitude, longitude, elevation)
- [ ] `CityContext` mit Provider erstellen (speichert die aktuell ausgewählte Stadt)
- [ ] Funktion `setSelectedCity` im Context zum Setzen der Stadt implementieren
- [ ] Context-Provider in der App-Root einbinden (z.B. in `main.tsx` oder `App.tsx`)
- [ ] React Router konfigurieren (Route `/city` definieren)
- [ ] Click-Handler in `SearchResultItem` hinzufügen (Stadt im Context setzen + Navigation zu `/city`)
- [ ] `CityDetailPage.tsx` als neue Page-Komponente erstellen
- [ ] Komponente `CityHeader` für Stadtname bauen
- [ ] Komponente `CityInfo` für Stadt-Metadaten (Land, Einwohnerzahl, Koordinaten, Höhe) erstellen
- [ ] Stadt-Daten aus `CityContext` in `CityDetailPage` mit `useContext` abrufen und anzeigen
- [ ] Fallback implementieren, falls keine Stadt im Context vorhanden ist (Hinweis + Link zur Suche)

## Umsetzungshilfe
- [Umsetzungshilfe – Story 2: Stadt auswählen](../umsetzungshilfen/story-02-stadt-auswaehlen.md)
