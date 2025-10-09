# Story 5 – Stadt als Favorit speichern

**Als** Nutzer **möchte ich** Städte als Favoriten speichern **damit** ich schnell auf sie zugreifen kann.

## Akzeptanzkriterien
- In der Stadt-Detailansicht ist ein Favoriten-Schalter sichtbar und klickbar.
- Der Favoritenstatus bleibt auch nach einem Neustart erhalten.

## Tasks
- [ ] TypeScript-Interface für Favoriten erstellen und Custom Hook `useFavorites` mit localStorage-Integration implementieren (addFavorite, removeFavorite, isFavorite, getAllFavorites)
- [ ] Komponente `FavoriteButton` mit Toggle-Logik und Icon (Herz gefüllt/ungefüllt) erstellen
- [ ] `FavoriteButton` in `CityDetailPage` einbinden und mit aktuellem Favoritenstatus synchronisieren

## Umsetzungshilfe
- [Umsetzungshilfe – Story 5: Stadt als Favorit speichern](../umsetzungshilfen/story-05-favorit-speichern.md)
