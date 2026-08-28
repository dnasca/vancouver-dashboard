# Vancouver WA Citizen Dashboard

An all-encompassing live map for Vancouver, Washington — crime, collisions,
property values, construction, transit, rail, parks, restaurants, culture,
weather, and more. One HTML file, no build step, no API keys.

**Proudly built with Claude (claude.ai).**

## Run it
Open `index.html` in a browser. That's it.

## Data sources
- Clark County GIS (`gis.clark.wa.gov` — parcels/taxlots, collisions, parks,
  trails, rail, C-TRAN, schools, fire stations, 6-yr TIP, census tracts)
- City of Vancouver GIS (VPD crime 2021–2024, hosted on ArcGIS Online)
- National Weather Service (`api.weather.gov`)
- OpenStreetMap (Overpass API for POIs, Nominatim for search, OSM tiles)

## Architecture notes
- Single file: CSS + HTML + JS in `index.html`
- ArcGIS fetch with automatic JSONP fallback (county server sends no CORS
  headers, so `file://` use depends on JSONP)
- Session cache per layer: padded-bbox reuse, toggle off = hide only
- Property values: full-county parcel download (~188k) persisted in
  IndexedDB → canvas "value surface" rendered client-side at all zooms;
  parcel polygons from the server at z≥16
- Roadmap: WSDOT real-time traffic (needs free access key), events feed,
  $/sqft + land-only value modes, visual design pass, hosting

## Branches
`main` = stable. Experiments live on feature branches.
