# Umsetzungshilfe – Story 4: Tagesdetails ansehen

## Struktur
```
src/
  components/
    HourlyChart.tsx             # Generische Chart-Komponente für stündliche Werte
  services/
    weatherService.ts           # Fetch für stündliche Daten
  types/
    weatherForecastAPI.ts       # Rohdaten-Typen der API
    weatherForecast.ts          # Gemappte Daten für Recharts
```

## Datenquelle (Open-Meteo, stündlich)
- Parameter: `hourly=temperature_2m,precipitation,wind_speed_10m`, `timezone=Europe/Berlin`.
- Filtere das Ergebnis auf den ausgewählten Tag (z. B. per Index oder Datum).

## Darstellung
- Nutze pro Metrik ein Diagramm oder kombiniere sinnvolle Werte (z. B. Temperatur als Linie, Niederschlag als Säule).
- Tooltips zeigen Zeit, Wert und Einheit verständlich an.
- Richte Layout responsiv aus: bei breiten Viewports zwei Charts nebeneinander, sonst untereinander.

## Zustände
- States: `isLoadingHourly`, `errorHourly`, `hourlyData`.
- Keine Daten → Hinweis "Keine Daten vorhanden".

## Exkurs: Datenmapping und Tooltip-Design
- Mappe die API-Arrays (`time`, `temperature`, `precipitation`, `wind`) in ein gemeinsames Array wie `{ timeISO, temp, rain, wind }`.
- Formatiere Zeitangaben lokal (z. B. `HH:mm`).
- Nutze klare Bezeichnungen und Einheiten im Tooltip.

## Exkurs: Responsive Layout
- Ein Grid- oder Flex-Layout mit Breakpoints (z. B. `col-12`, `col-lg-6`) sorgt für gute Darstellung auf allen Geräten.
