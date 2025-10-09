# Story 4 – Tagesdetails ansehen

**Als** Besucher **möchte ich** die stündliche Entwicklung von Temperatur, Niederschlag und Wind sehen **damit** ich den Tagesverlauf nachvollziehen kann.

## Akzeptanzkriterien
- Stündliche Werte werden als Diagramm angezeigt.
- Tooltips zeigen Zeit, Wert und Einheit.
- Wenn keine Daten verfügbar sind, erscheint der Hinweis "Keine Daten vorhanden".

## Tasks
- [ ] TypeScript-Interface für stündliche Wetterdaten erstellen und `weatherService` um `getHourlyWeather(latitude, longitude, date)` erweitern (hourly: temperature_2m, precipitation, wind_speed_10m)
- [ ] Custom Hook `useHourlyWeather` für API-Calls und State-Management erstellen
- [ ] Komponente `HourlyWeatherCharts` erstellen mit drei Recharts-Diagrammen (Temperatur, Niederschlag, Wind)
- [ ] Tooltips für jedes Diagramm konfigurieren (Zeit, Wert, Einheit anzeigen)
- [ ] Responsive Layout implementieren (CSS Grid/Flexbox: 2 Spalten ab Tablet, 1 Spalte auf Mobile)
- [ ] Charts in `CityDetailPage` einbinden und Lade-/Fehlerzustände implementieren

## Umsetzungshilfe
- [Umsetzungshilfe – Story 4: Tagesdetails ansehen](../umsetzungshilfen/story-04-tagesdetails.md)
