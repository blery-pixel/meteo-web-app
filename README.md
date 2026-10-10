# Aurora Weather — Weather Intelligence Dashboard

A premium, fully responsive weather dashboard built with **React**, **Vite**, and **Tailwind CSS**, powered entirely by the free **Open-Meteo** API (no API key required).

## Objective

Deliver a portfolio/client-ready weather intelligence dashboard that demonstrates clean React architecture, real-time API integration, thoughtful UI/UX, accessibility, and a polished premium visual identity.

## Features

- 🔍 **City search** against the Open-Meteo Geocoding API, with multi-result selection, empty-state handling, and a clear/reset action.
- 📍 **Geolocation** support (“Use my location”) with graceful handling of denied permissions, timeouts, and unavailable positions.
- 🌤️ **Weather hero** — city/country, live local clock, day/night indicator, current temperature, feels-like temperature, condition icon, sunrise/sunset, and a human-readable summary.
- 📊 **Current condition cards** — humidity, wind speed & direction, pressure, visibility, UV index, cloud cover, wind gusts, and apparent temperature, each using the correct Open-Meteo variable names.
- ⏱️ **24-hour forecast** — horizontally scrollable, with temperature, icon, and precipitation probability.
- 📅 **7-day forecast** — min/max temperature range bar, condition icon, and precipitation probability.
- ⭐ **Favorites & recent searches** — persisted in `localStorage`, with quick-switch chips and a dedicated dialog.
- 🌗 **Dark / light theme** toggle, applied consistently across the whole UI.
- 🎨 **Weather-reactive atmosphere** — hero background gradients and subtle motion (rain streaks, drifting clouds, twinkling stars, sun glow) change based on live conditions and day/night, and respect `prefers-reduced-motion`.
- ⏳ **Robust states** — skeleton loaders, network/API error alerts with retry, empty states, and non-blocking toast notifications (via `sonner`).
- ♿ **Accessibility** — keyboard-operable search and "Search" text action, visible focus rings, `aria-live`/`aria-busy` regions, labelled icon buttons.
- 📱 **Fully responsive** — single-column, touch-friendly mobile layout; adaptive grids for tablet and desktop.

## Technology Stack

- [React 19](https://react.dev/) + [TypeScript](https://www.typescriptlang.org/)
- [Vite](https://vitejs.dev/)
- [Tailwind CSS v4](https://tailwindcss.com/)
- Radix UI primitives (Dialog, Tabs, Tooltip, Separator, Switch) styled as shadcn/ui-style components
- [Lucide](https://lucide.dev/) icons
- [sonner](https://sonner.emilkowal.ski/) toasts
- [Open-Meteo](https://open-meteo.com/) Geocoding & Weather Forecast APIs

## Open-Meteo Integration

No API key or environment variables are required — Open-Meteo's free endpoints are used directly from the browser:

- **Geocoding:** `https://geocoding-api.open-meteo.com/v1/search?name={query}`
- **Forecast:** `https://api.open-meteo.com/v1/forecast?latitude={lat}&longitude={lon}&current=...&hourly=...&daily=...&timezone=auto`

Service modules:

- `src/services/geocodingApi.ts` — location search, error handling for network/HTTP failures.
- `src/services/weatherApi.ts` — current/hourly/daily forecast fetch using the exact Open-Meteo variable names (e.g. `temperature_2m`, `apparent_temperature`, `weather_code`, `wind_speed_10m`, `uv_index`, `precipitation_probability`).

Only data actually present in the API response is rendered; current-hour visibility/UV values are read from the closest matching hourly timestamp and clearly separated from the `current` block.

### Attribution

Per Open-Meteo's terms, attribution is displayed in the app footer:

> Weather data by [Open-Meteo.com](https://open-meteo.com/)

The public API is free for non-commercial use. If you plan to deploy this project commercially, review Open-Meteo's current commercial terms before launch.

## Typography

Design spec requires **Norse Bold by Joel Carrouche** for all headings. Norse Bold is a commercial display typeface and is **not available on free font CDNs**, so it cannot be bundled in this repository without a purchased license.

To use the real typeface:

1. Purchase/download **Norse Bold** from its official distributor.
2. Place the font files in `public/fonts/` (e.g. `NorseBold.woff2`, `NorseBold.woff`).
3. In `src/index.css`, replace the fallback `@import` with:

   ```css
   @font-face {
     font-family: "Norse Bold";
     src:
       url("/fonts/NorseBold.woff2") format("woff2"),
       url("/fonts/NorseBold.woff") format("woff");
     font-weight: 700;
     font-style: normal;
     font-display: swap;
   }
   ```

4. No further code changes are needed — `--font-display` already resolves to `"Norse Bold"` first.

Until the licensed font is added, the app falls back to **Unbounded**, a free, open-license geometric display font chosen for a similarly bold, carved identity, so the interface still matches the intended premium look.

## Project Structure

```
src/
  components/
    layout/        Header, Footer
    weather/        WeatherHero, WeatherStats, WeatherCard, WeatherSearch,
                     HourlyForecast, DailyForecast, WeatherSkeleton,
                     FavoritesDialog, CityChips, StateMessages
    ui/              Card, Badge, Skeleton, Tooltip, Separator, Tabs,
                     Dialog, Alert, Switch
  hooks/             useWeather, useGeolocation, useTheme, useLocalStorage
  services/          geocodingApi.ts, weatherApi.ts
  lib/               weatherCodes.ts, format.ts, constants.ts
  types/             weather.ts
  App.tsx
  main.tsx
```

## Installation & Development

```bash
git clone YOUR_REPOSITORY_URL
cd YOUR_PROJECT
npm install
npm run dev
```

### Build

```bash
npm run build
npm run preview
```

## Known Limitations

- Reverse geocoding (coordinates → city name) is not provided by Open-Meteo, so "Use my location" labels the result as **Current Location** rather than a resolved city name.
- The free Open-Meteo endpoints are rate-limited and intended for non-commercial use; heavy traffic may require self-hosting Open-Meteo or a commercial plan.
- "Norse Bold" is substituted with "Unbounded" until a licensed font file is supplied (see Typography section above).
