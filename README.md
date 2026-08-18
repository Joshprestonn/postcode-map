# Postcode Locator

Enter one or more UK postcodes and get a pin on a street-level map and a wider
regional map, plus the administrative hierarchy for each (ward, district,
county, region, constituency). Built for dropping asset locations into credit
papers.

Single self-contained HTML file. No build step, no install, no server.

## Use

Open `index.html` in a browser, or visit the hosted page.

- **Add** — look up a postcode and pin it. Add as many as you need.
- **Asset** — jump to one asset, or "All assets" to frame the whole portfolio.
- **Remove** — drop the selected asset.
- **Regional** — how far out the right-hand map sits (County / Region / Wide / Nation).
- **Copy** — put that map on the clipboard as an image, ready to paste into a
  document. If the browser blocks clipboard images it saves a PNG instead.

Exported images are the same size as the map on screen, so maximising the
window before copying gives a larger, sharper picture.

## Changing the map style

Edit one line near the top of the script:

```js
const STYLE = 'osm';        // 'osm' | 'esristreet' | 'positron'
```

Everything else, including the image export, reads from the `BASEMAPS` table
below it. Notes on the options:

- `osm` — standard OpenStreetMap. English place names.
- `esristreet` — Esri World Street Map. Conventional road-atlas look.
- `positron` — CARTO Positron. Pale grey, minimal.

Deliberately absent: OSM France (`osmfr`) renders French exonyms at low zoom,
labelling London as "Londres", which is wrong for a UK document.

## How the image export works

It composites the tiles Leaflet has **already** drawn on screen, so it issues no
network requests of its own. An earlier version re-fetched tiles at a higher
zoom for a sharper image, but that is the bulk fetching OpenStreetMap's [tile
usage policy](https://operations.osmfoundation.org/policies/tiles/) forbids, and
it got the app blocked. Resolution is traded for never being blocked again.

Tile layers set `crossOrigin` so the canvas stays exportable, and the export
measures what fraction of the output is blank, retrying once and then warning
rather than quietly producing a holey image.

## Dependencies

All external, all keyless. The page needs network access to:

| Host | For | If blocked |
|---|---|---|
| `unpkg.com` | Leaflet library | fatal, no map renders |
| `api.postcodes.io` | postcode lookup | lookups fail |
| `tile.openstreetmap.org` | map imagery | blank grey maps |

## Attribution and licensing

Map data © OpenStreetMap contributors (ODbL). Postcode data © ONS and Royal Mail
under the Open Government Licence. Attribution is burned into every exported
image so pasted maps stay compliant.

## Known limitations

- Nothing is persisted. The asset list lives in the open tab only.
- Postcodes resolve to a centroid, not an area polygon.
- Northern Ireland coverage is patchy in some ONS fields.
