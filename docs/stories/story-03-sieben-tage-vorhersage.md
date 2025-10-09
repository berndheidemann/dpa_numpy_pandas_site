# Story 3 – 7-Tage-Vorhersage sehen

**Als** Besucher **möchte ich** eine 7-Tage-Vorhersage mit Min- und Max-Temperaturen sehen **damit** ich den Trend erkennen kann.

## Akzeptanzkriterien
- Für jeden Tag sind Datum sowie Min- und Max-Werte in °C sichtbar.
- Während des Ladens wird ein passender Ladeindikator angezeigt.
- Falls keine Daten vorliegen, erscheint eine Fehlermeldung statt eines Diagramms.

## Tasks
- [ ] TypeScript-Interface für Weather-Forecast-Daten erstellen und `weatherService.ts` mit API-Anbindung implementieren (Open-Meteo Weather API: https://open-meteo.com/en/docs)
- [ ] Recharts Library installieren (`npm install recharts`)
- [ ] Custom Hook `useWeatherForecast` für API-Calls und State-Management (Loading, Error, Data) erstellen
- [ ] Komponente `WeatherForecastChart` mit Recharts erstellen (X-Achse: Datum, Y-Achse: °C, zwei Linien für Min/Max)
- [ ] Chart-Komponente in `CityDetailPage` einbinden und Lade-/Fehlerzustände implementieren

## Umsetzungshilfe
- [Umsetzungshilfe – Story 3: 7-Tage-Vorhersage sehen](../umsetzungshilfen/story-03-sieben-tage-vorhersage.md)
