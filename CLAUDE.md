# Project context for Claude sessions

Single-file web app: `index.html` (Leaflet, no build step).
Public-data map for Vancouver, WA. Owner: Derrik (junior full-stack,
prefers concise answers, wants "visually stunning"). Branding is
deliberately stripped during the dev cycle - title is just
"Vancouver, WA - open data map". Do not re-add product branding
without being asked.

Hosted on GitHub Pages from `main`. `index.html` IS the site, so any
commit to main goes live. Keep `.nojekyll` - without it Pages runs the
file through Jekyll and mangles `{{ }}` in the inline JS.

Key constraints learned the hard way:
- `gis.clark.wa.gov` (Clark County ArcGIS) sends NO CORS headers → all
  requests need the JSONP fallback already in `arcgisRequest()`. Verified
  the server honors `callback=`.
- City crime layer: layer id 1 (not 0) on the hosted FeatureServer.
- Tract stats join was flaky (only 31/~100 tracts matched); the canvas
  ValueSurface (from the IndexedDB county download) is the primary
  property-value visual — prefer extending it over the tract path.
- Parcel queries: use `resultOffset` pagination (2000/page) and
  `maxAllowableOffset` to shrink geometry payloads.
- Test any new county layer with a 1-record query first; layer field
  names vary wildly.

Style: dark navy theme, CSS vars in `:root`, gradient `valColor()` for
values, big accessible controls (A−/A+ buttons).

Layout (as of the Aug 2026 rebuild):
- Fixed top command bar owns all controls; group flyouts hang from their
  own button (JS-positioned). The left edge is reserved for the
  active-layer stack - do not put anything else there.
- `#active-stack` (bottom-left): one card per active layer, each with an
  X that dispatches `change` on the real `#cb-<id>` checkbox.
- `#load-stack` (right): per-layer loading bubbles, hover for source.
- `#guide` (bottom-centre): contextual tip engine, rules in `guideRules()`.
- Popups go through `buildPopup(title, props, spec, opts)`; `opts.tip` is
  the amber footer. Use `pick(props, [names])` for county field lookups -
  field naming is inconsistent across layers.

Testing: extract the inline script and `node --check` it, cross-reference
`$('id')` calls against `id=` attributes, and run the jsdom unit tests
against `buildPopup`, `guideRules()` and `chipUpdate()` before shipping.
