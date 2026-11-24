# World Airport Database — Export Artefacts

Ready-to-query airport data for flight simulator tooling, scenery work,
ATC bots and anything that needs a searchable airport table.

**Public domain (CC0) — download, use, redistribute, no restrictions.**

## Current release

See the **[v2026.11 release](https://github.com/Loggableim/world-airports-db/releases/tag/v2026.11)** for the latest files.

| File | Contents | Size |
|---|---|---|
| `world_airports_export-202611.db` | SQLite database with `airports_searchable` (86,387 rows), `country_rollup` (249), `region_rollup` (3,987), indexed on `ident` | ~13 MB |
| `airports.csv` | Flat CSV export of `airports_searchable` (86,387 rows) | ~12 MB |
| `s1-airports-iata-202611.json` | JSON: 8,799 airports with an IATA code, pre-filtered for travel-app search (large/medium/small) | ~4.9 MB |
| `s1-airports-iata-202611.csv` | Same as the JSON, as flat CSV | ~1.6 MB |

`airports_searchable` includes: ICAO/IATA/GPS/local codes, name, type
(large/medium/small/heliport/closed), municipality, region + country
(ISO codes and human-readable names), coordinates, elevation,
runway count, frequency count, longest runway length (ft), home page
and Wikipedia links.

## How to use it

- **SQLite:** download the `.db` and open it with any SQLite client,
  e.g. `sqlite3 world_airports_export-202611.db`.
- **CSV:** open in Excel/Python/QGIS or feed it straight into your
  tool's import pipeline.
- **JSON:** compact ready-made subset of airports with IATA codes —
  handy for pickers and search boxes in apps.

Example queries:

```sql
-- Scenery candidates: medium+ airports in a given region
SELECT ident, name, municipality, region_name, country_name,
       longitude_deg, latitude_deg, elevation_ft, longest_runway_ft
FROM airports_searchable
WHERE iso_region = 'US-CA' AND type IN ('large_airport','medium_airport')
ORDER BY longest_runway_ft DESC;

-- Runway finder: where can I put down a jet?
SELECT ident, name, country_name, longest_runway_ft
FROM airports_searchable
WHERE longest_runway_ft >= 9000
ORDER BY longest_runway_ft DESC;
```

## Update cadence

New release **every month**. Fixed filename pattern (`world_airports_export-YYYYMM.db`,
`s1-airports-iata-YYYYMM.json/.csv`), each release tagged and checksummed
(sha256 in the release notes).

## License

Data source: [OurAirports data dumps](https://davidmegginson.github.io/ourairports-data/)
(Public Domain). This project consumes those dumps, restructures them
into SQLite/CSV/JSON artefacts and republishes them — **also Public Domain
(CC0-equivalent)**. Attribution appreciated but not required.

Data corrections that belong to the underlying source should be reported
to OurAirports directly — this repo is a packaging layer, not a fork or
patch layer.
