# weather-widget

A single-file weather widget for a site header. It shows an icon, the current temperature in °F, and a short location label. No build step, no dependencies, no server code. The browser calls [Open-Meteo](https://open-meteo.com/) directly, which needs no API key.

Live demo: https://clintmcmahon.github.io/weather-widget/

## Use it

Open `index.html` in a browser, or copy the widget into your own page. You need three pieces:

1. The `.weather-widget` markup and its CSS.
2. The `<script>` block at the bottom.
3. A `data-*` configuration on the widget element.

## Configure

Set the city with data attributes on `#weather-widget`:

```html
<div
  class="weather-widget"
  id="weather-widget"
  data-lat="44.9778"
  data-lon="-93.2650"
  data-city="Minneapolis"
  data-label="MSP"
  data-timezone="America/Chicago"
  hidden
></div>
```

| Attribute | Purpose |
| --- | --- |
| `data-lat`, `data-lon` | Coordinates passed to Open-Meteo |
| `data-city` | Full name, shown in the hover tooltip ("Overcast in Minneapolis") |
| `data-label` | Short text shown next to the temperature |
| `data-timezone` | IANA timezone for the request |

Temperatures are Fahrenheit. For Celsius, change `temperature_unit` in `fetchWeather()` to `"celsius"` and the `°F` suffix in `render()`.

## How it behaves

- **Hidden until ready.** The widget starts with `hidden` and only appears after data loads. If the request fails or times out (5 seconds), it stays hidden and the page is unaffected.
- **Cached for 20 minutes.** Results are stored in `localStorage` per coordinate pair, so repeat page views don't hit the API. Storage errors (private mode, blocked storage) are swallowed.
- **No layout shift.** The widget reserves a `min-width` so the header doesn't jump when it appears.
- **Weather codes.** Open-Meteo's WMO codes map to a description and emoji in the `add(...)` calls. Unknown codes fall back to a generic thermometer.

## Hosting

The repo is served with GitHub Pages from the `main` branch root.
