# `data/` — input data files

This folder holds the small input files consumed by the notebooks under
`../notebooks/`. Larger files (SHARP FITS cubes) are downloaded on demand
into `data/sharp/` by notebook `02_download_SDO_HMI.ipynb` and are **not**
tracked by git.

All files in this folder are released under the
[Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)
license, matching the open-access licence of the associated article.

---

## `gannon_storm_2024_flares.csv`

GOES C-, M-, and X-class flare catalogue for the interval
**2024-04-28 → 2024-05-15**, produced by querying the Heliophysics Event
Knowledgebase (HEK) via SunPy. Reproduced by notebook
`01_download_GOES_flare_catalog.ipynb`.

| Column | Type | Description |
|---|---|---|
| `event_starttime` | ISO-8601 datetime | Flare start time (UTC) from GOES/XRS detection. |
| `event_peaktime`  | ISO-8601 datetime | Time of peak X-ray flux — used as reference for the peak-to-peak recurrence-time definition (Equation 1 of the paper). |
| `event_endtime`   | ISO-8601 datetime | Flare end time (UTC). |
| `fl_goescls`      | string | GOES class string (e.g. `C7.8`, `M9.1`, `X4.5`). |
| `ar_noaanum`      | integer | NOAA active-region number that produced the flare (e.g. `13663`, `13664`, `13654`, …). |

Notes:
* The catalogue is **not filtered** to AR 13663/13664. Filter on `ar_noaanum`
  to isolate the two regions of interest.
* HEK may report multiple entries for the same physical event when several
  pipelines have submitted overlapping detections. The paper de-duplicates
  these by collapsing entries that share the same `event_peaktime`.

Source: NOAA/SWPC + iSolSearch, retrieved via
[`sunpy.net.Fido`](https://docs.sunpy.org/en/stable/code_ref/net.html) on
2025-02-13.

---

## `goes_xray_flux_may2024.csv`

1-minute cadence GOES 1–8 Å longwave channel exported from
[NOAA SWPC](https://www.swpc.noaa.gov/products/goes-x-ray-flux) covering
2024-05-01 through 2024-05-14. Used as input by
`03_fig1_goes_xray_flux.ipynb` to reproduce **Figure 1**.

* Separator: `;` (semicolon).
* Decimal mark: `,` (comma).
* Column 1 — timestamp (UTC).
* Column 2 — longwave 1–8 Å flux in W m⁻².

---

## `dados_analises_may_2024_flares.xlsx`

Curated workbook with the **flare-by-flare recurrence-time tabulations**
used to produce Figure 5.

| Sheet | Purpose |
|---|---|
| `time_flare_interval_AR13663` | Per-bin histogram counts (`Recurrence Time` in hours, `Quantities` = number of intervals in that bin) for AR 13663. |
| `time_flare_interval_AR13664` | Same as above, for AR 13664. |

The histograms in Figure 5 of the paper were built from these tables.

---

## `dados_analises_may_2024_flares_pt-br_novo.xlsx`

A second workbook (Brazilian-Portuguese column names) used by
`05_fig3_fig4_flares_per_day.ipynb` to draw the per-day and per-class
breakdowns.

| Sheet | Purpose |
|---|---|
| `qtd_flare_per_day` | Daily flare counts per AR (Fig. 3). |
| `Class_flare_per_day` | Daily flare counts per AR **per class** C/M/X (Fig. 4). |

Both workbooks contain the same underlying data; they are kept side-by-side
because the per-class workbook still uses the original column names
(`Classe C`, `Classe M`, `Classe X`) that the notebook reads.

---

## `may_05_2-24_flares(complete)(with_C)_ars.xlsx`

Per-flare working catalogue (161 rows) used by
`07_fig7_flux_vs_recurrence.ipynb` to reproduce **Figure 7**. The notebook
reads the following columns by position (0-indexed):

| Col | Header | Used as |
|---:|---|---|
| 1 | `Event_peak time` | flare peak time (matched against the GOES series) |
| 3 | `fl_goescls` | GOES class string (`C`/`M`/`X…`) — sets marker size |
| 4 | `ar_noaanum` | NOAA active-region number (13663 / 13664) — sets colour |
| 7 | `Interval Time` | peak-to-peak recurrence time (parsed as a timedelta) |

Other columns in the workbook are auxiliary/scratch and are not read by the
notebook.

---

## `flares_with_flux.csv` (generated — not tracked)

Intermediate product written by the first code cell of
`07_fig7_flux_vs_recurrence.ipynb`: one row per flare with the GOES flux
mapped to its peak time. Columns: `Peak_Time`, `GOES_Class`, `AR_Number`,
`Recurrence_Time_h`, `X_ray_Flux_Wm2`. It is rebuilt every run and is listed
in `.gitignore`, so it is **not** committed.

---

## `sharp/` (not tracked)

After running `02_download_SDO_HMI.ipynb` you will find the SDO/HMI SHARP
Cylindrical-Equal-Area FITS cubes here, with filenames following the
pattern:

```
hmi.sharp_cea_720s.<HARPNUM>.<YYYYMMDD>_000000_TAI.<Br|Bt|Bp>.fits
```

| HARPNUM | Active region | Dates required |
|---:|---|---|
| 11142 | AR 13663 | 2024-05-03, 2024-05-05, 2024-05-06 (Fig. 8) |
| 11149 | AR 13664 | 2024-05-08, 2024-05-09, 2024-05-10 (Fig. 9) |

Source: [JSOC](http://jsoc.stanford.edu/), Stanford University.
