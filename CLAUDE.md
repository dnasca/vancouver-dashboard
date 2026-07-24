# Project context for Claude sessions

Single-file web app: `vancouver-dashboard.html` (Leaflet, no build step).
Citizen dashboard for Vancouver, WA. Owner: Derrik (junior full-stack,
prefers concise answers, wants "visually stunning", brands everything
"Built with Claude").

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
