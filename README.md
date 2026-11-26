# World Airport Database — Ready-to-Query Airport Data

86,389 airports, 48,449 runways, 30,403 radio frequencies, 11,008 navaids —
as a single indexed SQLite database plus flat CSV/JSON exports.
Refreshed automatically from daily OurAirports dumps and re-packaged every month.

**Public domain (CC0) — download, use, redistribute, no restrictions, no attribution required.**

[![License: CC0](https://img.shields.io/badge/License-CC0_1.0-lightgrey.svg)](LICENSE)
[![Latest release](https://img.shields.io/github/v/tag/Loggableim/world-airports-db?label=release)](https://github.com/Loggableim/world-airports-db/releases/tag/v2026.11)

## Why use this instead of raw dumps?

- **Zero schema surgery** — you get one flat, indexed `airports_searchable` table:
  ICAO/IATA/GPS/local codes, name, type (large/medium/small/heliport/closed),
  municipality, region + country (ISO codes *and* human-readable names),
  coordinates, elevation, runway count, frequency count, longest runway length (ft),
  home page and Wikipedia links. Ready to join, filter and sort.
- **Rollups included** — `country_rollup` (249 countries) and `region_rollup`
  (3,987 regions) so you don'"'"'t have to GROUP BY your way there.
- **Ready-made app subset** — `s1-airports-iata` gives you 8,799 airports with an
  IATA code (large/medium/small only, 0 duplicates), perfect for travel-app
  search boxes and pickers.
- **Stable, checksummed filenames** — `world_airports_export-YYYYMM.db`,
  sha256 printed in every release, monthly cadence.
- **Query-grade hygiene** — every release passes a 9-point verification suite
  (row-count rollups, referential integrity, spot checks like Innsbruck/LOWI).

## Current release

See the **[v2026.11 release](https://github.com/Loggableim/world-airports-db/releases/tag/v2026.11)** for the latest files.

| File | Contents | Size |
|---|---|---|
| `world_airports_export-202611.db` | SQLite database with `airports_searchable` (86,389 rows), `country_rollup` (249), `region_rollup` (3,987), indexed on `ident` | ~13 MB |
| `airports.csv` | Flat CSV export of `airports_searchable` (86,389 rows) | ~12 MB |
| `s1-airports-iata-202611.json` | JSON: 8,799 airports with an IATA code, pre-filtered for travel-app search (large/medium/small) | ~4.9 MB |
| `s1-airports-iata-202611.csv` | Same as the JSON, as flat CSV | ~1.6 MB |

## How to use it

- **SQLite:** download the `.db` and open it with any SQLite client,
  e.g. `sqlite3 world_airports_export-202611.db`.
- **CSV:** open in Excel/Python/QGIS or feed it straight into your
  tool'"'"'s import pipeline.
- **JSON:** compact ready-made subset of airports with IATA codes —
  handy for pickers and search boxes in apps.

Example queries:

```sql
-- Scenery candidates: medium+ airports in a given region
SELECT ident, name, municipality, region_name, country_name,
       longitude_deg, latitude_deg, elevation_ft, longest_runway_ft
FROM airports_searchable
WHERE iso_region = '"'"'US-CA'"'"' AND type IN ('"'"'large_airport'"'"','"'"'medium_airport'"'"')
ORDER BY longest_runway_ft DESC;

-- Runway finder: where can I put down a jet?
SELECT ident, name, country_name, longest_runway_ft
FROM airports_searchable
WHERE longest_runway_ft >= 9000
ORDER BY longest_runway_ft DESC;

-- Country dashboard from the rollup (no GROUP BY needed)
SELECT * FROM country_rollup ORDER BY airports_total DESC LIMIT 10;
```

Typical use cases: flight-simulator scenery planning, travel-app airport
pickers, geospatial analytics, map visualisations, ATC/aviation bots,
coverage research.

## Update cadence

New release **every month**. Fixed filename pattern (`world_airports_export-YYYYMM.db`,
`s1-airports-iata-YYYYMM.json/.csv`), each release tagged and checksummed
(sha256 in the release notes). The underlying dumps are pulled daily and
verified automatically.

## License

Data source: [OurAirports data dumps](https://davidmegginson.github.io/ourairports-data/)
(Public Domain). This project consumes those dumps, restructures them
into SQLite/CSV/JSON artefacts and republishes them — **also Public Domain
(CC0-equivalent)**. Attribution appreciated but not required.

Data corrections that belong to the underlying source should be reported
to OurAirports directly — this repo is a packaging layer, not a fork or
patch layer.
