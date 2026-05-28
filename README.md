# Extreme Temporal Clustering of Solar Flares in AR 13663/13664

[![DOI](https://img.shields.io/badge/DOI-10.1016%2Fj.asr.2026.04.125-blue)](https://doi.org/10.1016/j.asr.2026.04.125)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Data License: CC BY 4.0](https://img.shields.io/badge/Data%20License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)

This repository contains the data and code used to produce the figures, tables, and statistical analyses of the paper:

> **Garcia, B. F., Rezende Costa, J. E., Antunes Vieira, L. E., Dal Lago, A., do Vale Pena, C., da Silva Rocha, J. M., Raji, I.** (2026).
> *Extreme Temporal Clustering of Solar Flares in AR 13663/13664: Magnetic Topology and Recurrence Time in May 2024.*
> **Advances in Space Research**, COSPAR. DOI: [10.1016/j.asr.2026.04.125](https://doi.org/10.1016/j.asr.2026.04.125)

All authors are affiliated with the **Instituto Nacional de Pesquisas Espaciais (INPE)**, São José dos Campos, SP, Brazil.

---

## Summary of the study

We analyze the magnetic evolution and flare productivity of the two neighboring active regions **NOAA AR 13663** and **NOAA AR 13664** between 2024 May 1 and 2024 May 15 — the interval that produced the **Gannon Storm**, the first G5 geomagnetic storm since 2003. Using SDO/HMI SHARP vector magnetic indices and GOES/XRS soft X-ray data, we characterize:

- the spatial and temporal distribution of 162 C/M/X-class flares;
- flare waiting-time (recurrence time) statistics for each region;
- the evolution of SHARP non-potentiality proxies (`AREA_ACR`, `USFLUX`, `SHRGT45`, `R_VALUE`, `TOTPOT`);
- the topology of the photospheric Polarity Inversion Line (PIL).

The main result is that **flare recurrence time is decoupled from individual flare energetics** (ρ < 0.25 between peak X-ray flux and inter-event interval), and is instead controlled by PIL topology and the spatial distribution of magnetic shear.

---

## Repository structure

```
.
├── README.md                ← this file
├── LICENSE                  ← MIT license (code)
├── CITATION.cff             ← machine-readable citation
├── requirements.txt         ← Python dependencies
│
├── data/                    ← input data (small files only)
│   ├── gannon_storm_2024_flares.csv                  ← GOES/HEK flare catalog (Apr 28 – May 15, 2024)
│   ├── dados_analises_may_2024_flares.xlsx           ← per-day flare counts and recurrence-time tables
│   ├── may_05_2-24_flares(complete)(with_C)_ars.xlsx ← per-flare catalog (peak time, class, AR, recurrence) for Fig. 7
│   └── README_data.md                                ← description of each column and source
│
├── notebooks/               ← reproducible Jupyter notebooks (one per figure/table group)
│   ├── 01_download_GOES_flare_catalog.ipynb
│   ├── 02_download_SDO_HMI.ipynb
│   ├── 03_fig1_goes_xray_flux.ipynb
│   ├── 04_fig2_flare_disk_map.ipynb
│   ├── 05_fig3_fig4_flares_per_day.ipynb
│   ├── 06_fig5_recurrence_histograms.ipynb
│   ├── 07_fig7_flux_vs_recurrence.ipynb
│   └── 08_fig8_fig9_HMI_magnetic_morphology.ipynb
│
└── figures/                 ← output figures (PNG/PDF) reproduced by the notebooks
```

> **Note on large files:** the SHARP CEA FITS cubes used for Figures 8 and 9 (~hundreds of MB) are **not** stored in this repository. Notebook `02_download_SDO_HMI.ipynb` retrieves them from the JSOC. See [Data availability](#data-availability).

---

## Figure ↔ Notebook map

| Figure / Table | Description | Notebook |
|---|---|---|
| **Fig. 1**  | GOES/XRS 1–8 Å soft X-ray flux, May 01–14, 2024 | `03_fig1_goes_xray_flux.ipynb` |
| **Fig. 2**  | C/M/X flare locations on the solar disk (AR 13663 / AR 13664) | `04_fig2_flare_disk_map.ipynb` |
| **Fig. 3**  | Number of flares per day (both ARs) | `05_fig3_fig4_flares_per_day.ipynb` |
| **Fig. 4**  | Number of flares per day by class (C/M/X) | `05_fig3_fig4_flares_per_day.ipynb` |
| **Fig. 5**  | Histograms of flare recurrence time | `06_fig5_recurrence_histograms.ipynb` |
| **Fig. 6**  | Dst and Kp indices (EMBRACE/Kyoto WDC) | *external; figures reproduced from EMBRACE briefings* |
| **Fig. 7**  | Peak X-ray flux vs. recurrence time (energy–cadence decoupling) | `07_fig7_flux_vs_recurrence.ipynb` |
| **Figs. 8–9** | Bz / Bh and PIL contours (AR 13663 May 03–06; AR 13664 May 08–10) | `08_fig8_fig9_HMI_magnetic_morphology.ipynb` |
| **Table 1** | Comparative flare statistics per AR | derived from `gannon_storm_2024_flares.csv` |
| **Tables 3, 4** | Daily evolution of SHARP indices | manually extracted from [JSOC](http://jsoc.stanford.edu/) for HARPNUM 11142 (AR 13663) and HARPNUM 11149 (AR 13664) |

---

## Installation

The code is tested with **Python 3.10+**. We recommend creating a fresh virtual environment.

### Option 1 — `pip` + `venv`

```bash
git clone https://github.com/brunofernandesgarcia/Gannon-Storm-May-2024-Solar-Storms.git
cd Gannon-Storm-May-2024-Solar-Storms
python -m venv .venv
source .venv/bin/activate          # on Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter lab
```

### Option 2 — `conda` / `mamba` (recommended for SunPy + Cartopy)

```bash
conda create -n gannon python=3.11 jupyterlab sunpy astropy matplotlib numpy pandas scipy cartopy openpyxl -c conda-forge
conda activate gannon
```

---

## Reproducing the figures

After installation, open Jupyter Lab and run each notebook **top to bottom**. All file paths inside the notebooks are **relative** to the repository root (e.g. `../data/gannon_storm_2024_flares.csv`), so no editing is needed.

The notebooks are numbered in the order one would naturally run them: data acquisition (`01–02`) first, then figure generation (`03–08`).

---

## Data availability

| Data source | Provenance | Accessed via |
|---|---|---|
| GOES/HEK flare catalog | NOAA/SWPC + iSolSearch | `sunpy.net.Fido` (see notebook `01`) |
| SDO/HMI SHARP CEA vector magnetograms | JSOC (Stanford) | `sunpy.net.Fido` or [JSOC export](http://jsoc.stanford.edu/ajax/exportdata.html); **HARPNUM 11142** for AR 13663 and **HARPNUM 11149** for AR 13664 |
| GOES X-ray 1-min flux | NOAA SWPC | CSV included under `data/` |
| Dst index | World Data Center for Geomagnetism, Kyoto | EMBRACE/INPE briefings (cited in paper) |
| Kp nowcast | GFZ Helmholtz Centre Potsdam | EMBRACE/INPE briefings (cited in paper) |

The flare catalog `gannon_storm_2024_flares.csv` was obtained on Feb 13, 2025 via the SunPy/HEK interface and is provided here as-is for reproducibility.

---

## Open tasks

- [ ] Companion CSV/JSON of **Tables 3 and 4** (daily SHARP indices) so that they can be cross-checked programmatically rather than retyped from the PDF.
- [ ] Optional: archive a frozen release on **Zenodo** to get a citable DOI tied to a specific commit.

Contributions via pull request are welcome.

---

## License

- **Code** (`*.ipynb`, `*.py`): [MIT License](LICENSE) © 2026 Bruno Fernandes Garcia
- **Data** (`data/*.csv`, `data/*.xlsx`) and **figures**: [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/), matching the open-access license of the article.

You are free to reuse, modify, and redistribute these materials provided that you cite the article above.

---

## How to cite

If you use this code or data, please cite the article:

```bibtex
@article{Garcia2026Gannon,
  author  = {Garcia, Bruno Fernandes and Rezende Costa, Joaquim Eduardo and
             Antunes Vieira, Luis Eduardo and Dal Lago, Alisson and
             do Vale Pena, Christofher and da Silva Rocha, Jos\'e Matheus and Raji, Idowu},
  title   = {Extreme Temporal Clustering of Solar Flares in AR 13663/13664:
             Magnetic Topology and Recurrence Time in May 2024},
  journal = {Advances in Space Research},
  year    = {2026},
  doi     = {10.1016/j.asr.2026.04.125},
  note    = {Open access, CC BY 4.0}
}
```

A machine-readable [`CITATION.cff`](CITATION.cff) is also provided — GitHub will render a *"Cite this repository"* button automatically.

---

## Acknowledgments

This work was supported by the **Coordination for the Improvement of Higher Education Personnel (CAPES)**, grant number 88887.947301/2024-00 (B. F. Garcia, doctoral scholarship), and by the **Brazilian National Council for Scientific and Technological Development (CNPq)**, project 312377/2022-3 (A. Dal Lago).

We thank the SDO/HMI and GOES/XRS teams for the public availability of the data, the **EMBRACE/INPE Space Weather Program** for the geomagnetic indices, and the **iSolSearch** and **HEK** services for the flare catalogs.

---

## Contact

For questions or issues, please open an issue on this repository or contact the corresponding author:

**Bruno Fernandes Garcia** — Instituto Nacional de Pesquisas Espaciais (INPE)
