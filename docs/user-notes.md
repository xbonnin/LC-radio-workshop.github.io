# A user guide to heliospheric radio data

This guide is for scientists who want to work with heliospheric radio dynamic spectra without having to learn every instrument's file format from scratch. The goal is simple: get you from "I want to look at radio data for this event" to "I have a plot" in as few steps as possible, using the existing Python tools that already do most of the work.

The most important message of this guide: **use a loader.** There are several mature Python packages that read radio data from ground based and space based instruments, hand you a sensible Python object, and let you plot and slice it without ever opening a CDF or FITS file directly. You almost never need to read raw files yourself. This guide tells you which loader to reach for, and how to use it.

---

## Pick a loader

For most heliospheric radio data, one of these four packages will read what you need:

| Loader | What it's good for |
|---|---|
| [`radiospectra`](https://docs.sunpy.org/projects/radiospectra/) | SunPy affiliated package. Good first choice for solar radio data. Handles e-CALLISTO, SWAVES, RFS (PSP), WAVES (Wind), Learmonth (RSTN) and more. Integrates with Fido for search and download. |
| [`MASER (maser4py)`](https://maser.readthedocs.io/) | Strong on space mission radio data. Covers Cassini, Juno, Solar Orbiter/RPW, STEREO/WAVES, Wind/WAVES, Nançay instruments. Uses xarray under the hood. |
| [`pyspedas`](https://pyspedas.readthedocs.io/) | Good for multi spacecraft work. Pulls data from many space missions in one consistent way via CDAWeb. |
| [`Speasy`](https://speasy.readthedocs.io/) / [`SciQLop`](https://sciqlop.github.io/) | Speasy gives format agnostic retrieval from AMDA, CDAWeb, SSCWeb, CSA, 3DView, HAPI. SciQLop is an interactive viewer built on top. Good when you want to explore visually first. |

A rough rule of thumb:

* Working with solar radio bursts and you already use SunPy → start with `radiospectra`.
* Working with space mission data and want consistent access across many missions → `pyspedas`, `MASER` or `Speasy`.
* Want to browse before you commit to a script → SciQLop.

You can use more than one. Many people use `radiospectra` for solar ground based data and `pyspedas` or `MASER` for in situ space data.

---

## Multi instrument plots

_To be filled in._

---

## Per instrument notes

These are the things that are useful to know before you start, organised by instrument. They are not exhaustive; they are the things people most commonly ask about.

### LOFAR (ground based, ~10 to 240 MHz)

* Solar LOFAR data is hosted at the [solar KSP web page](https://spaceweather.astron.nl/SolarKSP/data/atdb_process/solar_bf_compressing/).
* I-LOFAR data is hosted at [data.lofar.ie](https://data.lofar.ie/) and is accessible via `radiospectra` (Fido client). Note that the subdirectory layout has changed over time; the Fido client does not currently traverse every subdirectory automatically.
* Different observing modes produce different file formats (BST, H5, FITS). The mode determines which antennas were used and how the data is structured.
* IDOLS produces 0.25 s FITS files in the same format as e-CALLISTO.
* SciQLop can now read LOFAR data.
* For raw LTA data, an LTA account is required.

### e-CALLISTO (ground based, network)

* Many stations worldwide; pick the one with the best coverage for your event time. The [e-CALLISTO Globe Explorer](https://github.com/MohamedNedal/ecallisto-streamlit) and the [UAH dashboard](https://astrodoncel.uah.es/dashboard/stations.php) can help you choose.
* Quicklooks: [soleil.i4ds.ch/solarradio/callistoQuicklooks/](https://soleil.i4ds.ch/solarradio/callistoQuicklooks/).
* Accessible via `radiospectra` and Fido using `a.Instrument('eCALLISTO')` and `a.Observatory('STATION-NAME')`.
* Frequency range is not encoded in the filename, so you may need to load a file to check.

### NDA (Nançay Decameter Array, ground based, ~10 to 88 MHz)

* Three different web portals host NDA data in different formats (FITS and CDF). The structure differs between them.
* CDF: [realtime.obs-nancay.fr](https://realtime.obs-nancay.fr/dam/databf/web/soleil/data/)
* FITS: [cdn.obs-nancay.fr](https://cdn.obs-nancay.fr/repository/nda/newroutine/soleil/)
* No Fido support yet. You can load files manually with `maser4py` or with `cdflib`/`spacepy`.

### ORFEE (ground based, 144 to 1000 MHz)

* Quicklooks: [secchirh.obspm.fr](https://secchirh.obspm.fr/spip.php?article11).
* Data download is manual via [rsdb.obs-nancay.fr](https://rsdb.obs-nancay.fr/) (requires a login).
* No Fido support yet.

### Learmonth (ground based, RSTN, 25 to 180 MHz)

* Hosted at the Australian Bureau of Meteorology: [downloads.sws.bom.gov.au](https://downloads.sws.bom.gov.au/wdc/wdc_spec/data/learmonth/).
* SRS (Solar Radio Spectrograph) binary format.
* Readable with `radiospectra` (`RSTNSpectrogram`).
* Fido support is being added.

### OVSA / EOVSA / OVRO-LWA (ground based, US)

* EOVSA: [ovsa.njit.edu/eovsadata](https://ovsa.njit.edu/eovsadata/).
* OVRO-LWA: [ovsa.njit.edu/lwa](https://ovsa.njit.edu/lwa/).
* Both currently require credentials, which breaks automated access via `radiospectra`. A `radiospectra` client update to handle authentication is in progress.

### Solar Orbiter / RPW (space, ~4 kHz to 16 MHz)

* L3 (science calibrated) is available from [rpw-lira.obspm.fr](https://rpw-lira.obspm.fr/roc/data/pub/solo/rpw/data/) and via Fido using `sunpy_soar`.
* TNR covers 4 kHz to 1 MHz, HFR covers 425 kHz to 16 MHz.
* Important quirk: HFR data from 20 December 2022 to 21 January 2024 was acquired in a special configuration where five frequencies are sampled ten times, followed by a longer 50 frequency sampling. For this period, only every 10th spectrum has all 50 channels. For most analyses, use only the complete spectra. See Figure A1 in Vecchio et al. 2024 for details.

### Parker Solar Probe / FIELDS (space, ~10 kHz to 19 MHz)

* L2 and L3 are public. L1 is team only.
* For most users, start with L3 and use the variables `psp_fld_l3_rfs_hfr_PSD_SFU` (or `PSD_FLUX`) for HFR and `psp_fld_l3_rfs_lfr_PSD_SFU` for LFR. These are pre combined from the antenna pairs and converted to convenient units (SFU is normalised to 1 au; FLUX is at the spacecraft).
* L3 also includes spacecraft position, attitude, temperature, and bias current.
* Accessible via `radiospectra`, `pyspedas`, and `MASER`.
* PSP CDFs contain many variables; most are empty for any given day. The PSD_SFU and PSD_FLUX variables are what most users want.

### STEREO/WAVES (space)

* L3 is available for both STEREO-A and STEREO-B (HFR and LFR), already calibrated to SFU at 1 au.
* Accessible via Fido using the CDAWeb dataset strings `STA_L3_WAV_HFR`, `STA_L3_WAV_LFR`, `STB_L3_WAV_HFR`, `STB_L3_WAV_LFR`.
* Frequency coverage: HFR 0.125 to 16 MHz, LFR 2.6 kHz to 1.5 MHz.
* Prefer L3 over the "combined" files for higher resolution and calibrated units.

### Wind/WAVES (space)

* L2 and L3 are available. L3 is only available for RAD1; L2 covers RAD2 and TNR.
* Frequency coverage: TNR 4 to 256 kHz, RAD1 20 kHz to 1 MHz, RAD2 1.1 to 14 MHz.
* Hosted on SPDF: [cdaweb.gsfc.nasa.gov/pub/data/wind/waves](https://cdaweb.gsfc.nasa.gov/pub/data/wind/waves/).
* Accessible via `pyspedas`, `radiospectra`, and `MASER`.
* Note: the same data is mirrored at CDAWeb (USA) and CDPP (France); these are not always identical. The PI institution provider is the most authoritative.

---

## When there is no loader yet

_To be filled in._

---

## A few good practices

_To be filled in._

---

## Getting help

* For radiospectra: [issue tracker](https://github.com/sunpy/radiospectra/issues).
* For MASER: [issue tracker](https://github.com/maserlib/maser4py/-/issues).
* For pyspedas: [issue tracker](https://github.com/spedas/pyspedas/issues).
* For the SunPy ecosystem more broadly: [SunPy chat](https://app.element.io/#/room/#sunpy:openastronomy.org).
