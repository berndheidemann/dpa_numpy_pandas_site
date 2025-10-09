# Umsetzungshilfe – Story 3: 7-Tage-Vorhersage sehen

## Struktur
```
src/
  pages/
    CityDetailPage.tsx    # Zeigt auch die 7-Tage-Vorhersage
  components/
    ForecastChart.tsx     # Diagramm für Min-/Max-Temperaturen
  services/
    weatherService.ts     # Fetch für tägliche Vorhersagen
  types/
    forecast.ts           # Typen für Tageswerte
```

## Datenquelle (Open-Meteo, täglich)
- Nutze die Forecast-API mit `daily=min_temperature_2m,max_temperature_2m`.
- Parameter: `latitude`, `longitude`, `timezone=Europe/Berlin`.
- Lade genau sieben Tage und mappe Daten in ein Array wie `{ date, tMin, tMax }`.

## Komponentenaufteilung
- `CityDetailPage` kennt die Koordinaten und ruft den Service, sobald die Stadt gesetzt ist.
- `ForecastChart` visualisiert die Min-/Max-Werte; Datum sinnvoll formatiert anzeigen.

## Lade- und Fehlerzustände
- States: `isLoadingForecast`, `errorForecast`, `forecastDays`.
- Während des Ladens einen Spinner oder Skeleton zeigen.
- Bei Fehlern eine klare Meldung ausgeben.
- Bei leeren Ergebnissen einen Hinweis wie "Keine Vorhersage verfügbar" anzeigen.

## Exkurs: Recharts kurz erklärt
- Recharts arbeitet mit einfachen Arrays von Objekten.
- Nutze für jede Serie (`tMin`, `tMax`) einen eigenen `Line`-Eintrag und setze `dataKey` entsprechend.
- `ResponsiveContainer` sorgt für ein flexibles Layout.
- Tooltips und klare Achsenbeschriftungen erhöhen die Lesbarkeit.

## Exkurs: Zeitzonen und Einheiten
- Setze `timezone=Europe/Berlin`, damit Tagesgrenzen passen.
- Achte darauf, Temperaturen als °C darzustellen und Datumslayout (z. B. `dd.MM.`) zu vereinheitlichen.
