# weather-widget

A small weather widget for a site header. It shows an icon, the current temperature in °F, and a short location label. No build step, no dependencies, no server code. The browser calls [Open-Meteo](https://open-meteo.com/) directly, which needs no API key.

Live demo: https://clintmcmahon.github.io/weather-widget/

## Use it

Add the stylesheet, the markup, and the script to your page:

```html
<link rel="stylesheet" href="https://clintmcmahon.github.io/weather-widget/weather-widget.css">

<div
  class="weather-widget"
  id="weather-widget"
  data-lat="44.9778"
  data-lon="-93.2650"
  data-city="Minneapolis"
  data-label="MSP"
  data-timezone="America/Chicago"
  hidden
>
  <span class="weather-icon" aria-hidden="true"></span>
  <span class="weather-temp"></span>
  <span class="weather-loc"></span>
</div>

<script src="https://clintmcmahon.github.io/weather-widget/weather-widget.js" defer></script>
```

The element must have `id="weather-widget"`, and only one widget per page is supported.

To pin a version, serve the files from jsDelivr after tagging a release:
`https://cdn.jsdelivr.net/gh/clintmcmahon/weather-widget@v1.0.0/weather-widget.js`. Pointing at the Pages URL tracks `main`, so every push changes every embed.

You can also download `weather-widget.css` and `weather-widget.js` and host them yourself.

## Configure

Set the city with data attributes on `#weather-widget`:

| Attribute | Purpose |
| --- | --- |
| `data-lat`, `data-lon` | Coordinates passed to Open-Meteo |
| `data-city` | Full name, shown in the hover tooltip ("Overcast in Minneapolis") |
| `data-label` | Short text shown next to the temperature |
| `data-timezone` | IANA timezone for the request |

Temperatures are Fahrenheit. For Celsius, self-host `weather-widget.js` and change `temperature_unit` in `fetchWeather()` to `"celsius"` and the `°F` suffix in `render()`.

## How it behaves

- **Hidden until ready.** The widget starts with `hidden` and only appears after data loads. If the request fails or times out (5 seconds), it stays hidden and the page is unaffected.
- **Cached for 20 minutes.** Results are stored in `localStorage` per coordinate pair, so repeat page views don't hit the API. Storage errors (private mode, blocked storage) are swallowed.
- **No layout shift.** The widget reserves a `min-width` so the header doesn't jump when it appears.
- **Weather codes.** Open-Meteo's WMO codes map to a description and emoji in the `add(...)` calls. Unknown codes fall back to a generic thermometer.

## Hosting

The repo is served with GitHub Pages from the `main` branch root. `index.html` is the demo page, and `weather-widget.css` and `weather-widget.js` are the files to embed.
