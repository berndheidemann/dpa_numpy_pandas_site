# Story 6 – Favoritenliste verwalten

**Als** Nutzer **möchte ich** meine gespeicherten Favoriten anzeigen und verwalten **damit** ich schnell zwischen Städten wechseln kann.

## Akzeptanzkriterien
- Die Liste zeigt Stadtname, Land, Einwohnerzahl, Höhe über NN, Breiten- und Längengrad sowie einen Schalter zum Entfernen.
- Ein Klick auf den Namen öffnet die Detailseite.
- Beim Entfernen wird die Liste sofort aktualisiert.

## Tasks
- [ ] `FavoritesPage.tsx` als neue Page erstellen und Route `/favorites` im Router konfigurieren
- [ ] Komponente `FavoritesList` erstellen, die alle Favoriten aus dem `useFavorites` Hook anzeigt
- [ ] Komponente `FavoriteListItem` erstellen mit allen Stadt-Infos (Name, Land, Einwohnerzahl, Koordinaten, Höhe) und Löschen-Button
- [ ] Click-Handler für Stadtname implementieren (Stadt in Context setzen + Navigation zu `/city`)
- [ ] Löschen-Funktion mit sofortiger UI-Aktualisierung und localStorage-Synchronisation implementieren
- [ ] Empty State für "Keine Favoriten vorhanden" mit Link zur Suche hinzufügen

## Umsetzungshilfe
- [Umsetzungshilfe – Story 6: Favoritenliste verwalten](../umsetzungshilfen/story-06-favoritenliste.md)
