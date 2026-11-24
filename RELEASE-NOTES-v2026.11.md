# Release v2026.11 — 24/25 Nov 2026

First public release. Built from OurAirports data dumps (public domain),
re-imported and verified as a monthly snapshot.

## Files

| File | Contents | Size | sha256 |
|---|---|---|---|
| `world_airports_export-202611.db` | SQLite: `airports_searchable` (86,387), `country_rollup` (249), `region_rollup` (3,987), index on `ident` | ~13 MB | `356b077dd6a9d6c610e0f10abc3779eb3c29a1ad5a08f8f5b5872febea529c5a` |
| `airports.csv` | Flat CSV of `airports_searchable` (86,387) | ~12 MB | `b4ff18848b8f62a9048fe45b46cf52bc5cc5926d609836d242bb9b56a789efbf` |
| `s1-airports-iata-202611.json` | JSON: 8,799 airports with IATA (travel-app search subset) | ~4.9 MB | `4386411640f3415225a28b331cabc5e794aa02ad2e6478de084498f66a61ecfe` |
| `s1-airports-iata-202611.csv` | Same, flat CSV | ~1.6 MB | `835cf57c1efb86697b627a9e6b45f7d234c1ca0fbe77b436c92f89931004a60c` |

## Contents

- 86,387 airports (large / medium / small / heliport / closed)
- 249 countries, 3,987 regions (pre-joined names + ISO codes)
- Runway count, frequency count and longest runway length pre-computed per airport

## Changelog

- Initial release.
- Source: OurAirports daily dumps, re-imported and verified 24 Nov 2026.

## License

Public Domain (CC0). Source data: OurAirports (public domain). Attribution
appreciated but not required.
