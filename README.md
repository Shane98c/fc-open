# fc-open

Open databases behind [Flyover Country](https://flyovercountry.app), published under the
[Open Database License (ODbL) 1.0](LICENSE). The files are attached to this repo's
[releases](../../releases), one release per dataset version.

## Route paths

`route_paths.parquet`: the paths airliners fly between two airports, as a centerline and a
band around it, for 13,470 airport pairs. Flyover Country uses the rows marked `served`.

Contains information from [ADSB.lol](https://adsb.lol), made available under the Open
Database License (ODbL). Built from a rolling window of ADSB.lol's daily `globe_history`
archives, filled over oceans from filed flight plans, oceanic position reports and North
Atlantic track messages. Airport coordinates are from
[vrs-standing-data](https://github.com/vradarserver/standing-data) (CC0).

One row per unordered pair. `pair_undirected` is `A|B` in IATA order, and every profile runs
from A (`a_lonlat`, `icao[0]`) to B (`b_lonlat`, `icao[1]`):

| Column | Meaning |
|---|---|
| `center_km`, `lo_km`, `hi_km` | 65 cross-track offsets in km at evenly spaced points along the A→B great circle; positive is left of travel from A to B. `lo_km` and `hi_km` are the `band_q` and `1 − band_q` quantiles of flown tracks at each point |
| `center_fwd_km`, `center_rev_km` | Per-direction centerlines, where served |
| `n_flights`, `n_fwd`, `n_rev`, `n_days` | Flights and days in the window |
| `window_start`, `window_end`, `end` | The window the row was fitted on, and the run date |
| `cfull_gc`, `cfull_env` | Share of held-out flights fully inside a great-circle corridor and inside this one |
| `center_err_km`, `gc_err_km` | Median distance of held-out flights from this centerline and from the great circle |
| `modelled_frac`, `planned_frac` | Share of the route filled in where no flight was seen, and share standing in from filed routes |
| `packs_gc`, `packs_env` | Map tiles an offline save needs for each corridor |
| `served`, `refused` | Whether the app uses the row, and why not |

## Waterbodies

`waterbodies.parquet`: lakes, rivers, seas and bays matched to their Wikipedia articles,
8,214 features with GeoParquet geometry.

Geometry © [OpenStreetMap contributors](https://www.openstreetmap.org/copyright), ODbL, with
[Natural Earth](https://www.naturalearthdata.com) 1:10m (public domain) where OpenStreetMap
draws no shape (`geometry_source = 'ne'`). Article titles and short descriptions are from
English Wikipedia (CC BY-SA 4.0), identifiers from Wikidata (CC0), and image links point to
Wikimedia Commons files under their own licenses.

## License

The databases are licensed under the [ODbL 1.0](LICENSE). Individual contents keep the
licenses named above.
