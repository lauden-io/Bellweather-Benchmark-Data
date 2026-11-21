# Bellweather Benchmark: raw data

Every forecast and every measurement behind the public benchmark of
[Bellweather](https://github.com/lauden-io/Bellweather), the weather
integration for Home Assistant. Six providers are asked for the same 24
random addresses on six continents every 4 hours, and scored against what
official weather stations then measured. The method was fixed and published
before the first capture:
[docs/benchmark.md](https://github.com/lauden-io/Bellweather/blob/main/docs/benchmark.md).

A benchmark is only worth what anyone can check. So every report comes with
a release here, and nothing in it is picked: every forecast of every
capture, as stored, and every report of every scoring station. Before a
release is published, its report is recomputed from its files alone, and
the tables have to match.

## Releases

| Report | Raw data | Captures |
|---|---|---|
| [1 week, 10–17 November 2026](https://github.com/lauden-io/Bellweather/blob/main/docs/benchmark/2026-11-17-1-week.md) | [2026-11-17-1-week](https://github.com/lauden-io/Bellweather-Benchmark-Data/releases/tag/2026-11-17-1-week) | 10 November 23:00 to 17 November 19:00 UTC |

Next: 2 weeks (25 November 2026), 30 days (11 December 2026), 3 months,
6 months and 1 year.

## What a release holds

| File | What it holds | Licence |
|---|---|---|
| `forecasts.csv.gz` | every capture's forecasts from Bellweather, its control, ECMWF IFS, Open-Meteo, MET Norway and DWD | CC BY 4.0 |
| `forecasts-openweather.csv.gz` | every capture's OpenWeather forecasts | ODbL 1.0 |
| `observations-esoh.csv.gz` | the European scoring stations' reports (E-SOH) | CC BY 4.0 |
| `observations-metar.csv.gz` | the airport scoring stations' reports (METAR) | public domain in the United States |
| `captures.csv.gz` | the outcome of every request at every capture | CC BY 4.0 |
| `MANIFEST.json` | the period, every file's row count and SHA-256, and the commit of the code | |
| `ATTRIBUTION.md` | sources, licences, the changes made, and every column explained | |

- **Forecasts:** the captures made in the report's period. A capture is
  never changed or made up later, so the releases add up to every capture.
- **Station reports and request outcomes:** everything recorded from the
  start up to the export. A station report can arrive, or be fetched again,
  hours after its time, so each release holds all of them.
- The files are gzipped CSV, the same bytes for the same data, so the
  SHA-256 in the manifest identifies the data, not the moment of the export.

## Recompute a report

Python 3 and nothing else (tested with Python 3.12):

```sh
git clone https://github.com/lauden-io/Bellweather
cd Bellweather
git checkout COMMIT        # code.commit in the release's MANIFEST.json
python benchmark/app/export.py load bench.sqlite ../2026-11-17-1-week
python benchmark/app/report.py bench.sqlite benchmark/points.json report.md --days 7 --end 2026-11-17T23:00
```

`load` checks every file against the manifest first. For a later report,
load every release up to it, oldest first; `--days` and `--end` are the
report's period (the benchmark started on 10 November 2026 at 23:00 UTC).

The first week was exported from a copy of the add-on's database, with the
station reports up to the moment its report was written; the manifest says
so, and every table recomputes exactly. From the second report on, the
benchmark add-on exports the data itself when it writes the report.

## Licences and attribution

Each file keeps its source's licence; `ATTRIBUTION.md` in every release has
the full credits and the changes made (units, the hour a precipitation
amount belongs to).

- **CC BY 4.0:** weather data by Open-Meteo.com (ECMWF IFS, Open-Meteo, and
  the models Bellweather blends); based on data from MET Norway; based on
  data from Deutscher Wetterdienst; EUMETNET E-SOH station reports; the
  benchmark's own files. https://creativecommons.org/licenses/by/4.0/
- **ODbL 1.0:** weather data © OpenWeather, kept in a file of its own, so
  the share-alike terms stay with that file.
  https://opendatacommons.org/licenses/odbl/1-0/
- **Public domain in the United States:** airport reports as served by
  NOAA's Aviation Weather Center. Not official NOAA material; no
  endorsement implied.

This README is CC BY 4.0. The benchmark's code is in the Bellweather
repository, under the GNU AGPL 3.0 or later.
