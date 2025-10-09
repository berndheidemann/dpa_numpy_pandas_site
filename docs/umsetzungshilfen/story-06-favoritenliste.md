# Umsetzungshilfe – Story 6: Favoritenliste verwalten

## Struktur
```
src/
  pages/
    FavoritesPage.tsx      # Liste + Entfernen-Button
  components/
    FavoritesList.tsx      # Container für die Liste
    FavoriteListItem.tsx   # Einzelner Eintrag (klickbar)
  context/
    FavoritesContext.tsx   # Liefert Daten und Aktionen
```

## Darstellung und Interaktion
- Zeige Name, Land, Einwohnerzahl, Höhe über NN, Breitengrad, Längengrad sowie eine Entfernen-Aktion.
- Ein Klick auf den Stadtnamen navigiert zur Detailseite (Router-Link verwenden).

## State-Synchronisation
- Lies die Daten aus dem `FavoritesContext`.
- Entfernen aktualisiert den Context und persistiert die neue Liste sofort.

## Exkurs: Immutability und Performance
- Erzeuge bei Änderungen neue Arrays (z. B. `favorites.filter(...)`).
- Verwende stabile Keys, damit React effizient rendern kann.
- Für umfangreichere Listen kann `React.memo` helfen, unnötige Re-Renders zu vermeiden.
