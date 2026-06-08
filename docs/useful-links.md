# Useful links

A curated list of resources for working with heliospheric radio data. More can be added. 
---

## Python packages

The maintained loaders and tools you should reach for first.

* [**radiospectra**](https://github.com/sunpy/radiospectra). SunPy affiliated package for radio dynamic spectra. Supports search, download, reading and plotting for: e-CALLISTO, EOVSA, I-LOFAR (mode 357 BST), PSP/RFS, Solar Orbiter/RPW, RSTN (Learmonth and others), STEREO/SWAVES, Wind/WAVES, plus custom spectra (minimum required: time, frequency, and data arrays).
* [**maser-data (maser4py)**](https://pypi.org/project/maser-data/). Python package for reading low frequency radio data from many space missions. Returns xarray objects.
* [**pyspedas**](https://pyspedas.readthedocs.io/). Multi mission space physics data access. Good for retrieving PSP/RFS, Wind/WAVES, STEREO/WAVES and Solar Orbiter/RPW spectra consistently.
* [**Speasy**](https://speasy.readthedocs.io/). Format agnostic data retrieval from AMDA, CDPP, CDAWeb, SSCWeb.
* [**SciQLop**](https://github.com/SciQLop). Interactive visualisation environment for space physics time series. GUI based with a Python API.
* [**sunpy-soar**](https://docs.sunpy.org/projects/soar/). Solar Orbiter Archive client for SunPy Fido.
* [**SolarViewer**](https://pypi.org/project/solarviewer/). Visualisation and download tool for Learmonth, RSTN, e-CALLISTO and related solar data.

---

## Ground based instruments

### LOFAR
* Solar KSP preview data: [spaceweather.astron.nl/SolarKSP](https://spaceweather.astron.nl/SolarKSP/data/atdb_process/solar_bf_compressing/)
* LOFAR Long Term Archive: [lta.lofar.eu](https://lta.lofar.eu/)
* I-LOFAR data: [data.lofar.ie](https://data.lofar.ie/)

### e-CALLISTO
* Main data archive: [soleil.i4ds.ch/solarradio/data](https://soleil.i4ds.ch/solarradio/data/2002-20yy_Callisto/)
* Daily quicklooks per station: [soleil.i4ds.ch/solarradio/callistoQuicklooks](https://soleil.i4ds.ch/solarradio/callistoQuicklooks/)
* UAH interactive dashboard with burst reports and statistics: [astrodoncel.uah.es/dashboard](https://astrodoncel.uah.es/dashboard/index.php)
* Station selector and quicklooks: [astrodoncel.uah.es/dashboard/stations](https://astrodoncel.uah.es/dashboard/stations.php)

### Nançay (NDA, ORFEE, NenuFAR)
* NDA real time data (CDF): [realtime.obs-nancay.fr/dam](https://realtime.obs-nancay.fr/dam/databf/web/soleil/data/)
* NDA FITS archive: [cdn.obs-nancay.fr/repository/nda](https://cdn.obs-nancay.fr/repository/nda/newroutine/soleil/)
* Nançay radio spectrograph database (NDA, ORFEE): [rsdb.obs-nancay.fr](https://rsdb.obs-nancay.fr/)
* ORFEE quicklooks: [secchirh.obspm.fr](https://secchirh.obspm.fr/spip.php?article11)
* NDA documentation and DOI: [maser-lira.obspm.fr](https://maser-lira.obspm.fr/publications/doi/nancay-decameter-array-nda-routine-102.html)

### RSTN (Learmonth and other stations)
* Australian Bureau of Meteorology archive: [downloads.sws.bom.gov.au/wdc/wdc_spec/data/learmonth](https://downloads.sws.bom.gov.au/wdc/wdc_spec/data/learmonth/)
* World Data Centre Solar Radio Spectrograph: [sws.bom.gov.au/World_Data_Centre/1/1](https://www.sws.bom.gov.au/World_Data_Centre/1/1)
* SRS format description: [sws.bom.gov.au/World_Data_Centre/2/8/8](https://www.sws.bom.gov.au/World_Data_Centre/2/8/8)

### OVSA / EOVSA / OVRO-LWA
* EOVSA data portal: [ovsa.njit.edu/eovsadata](https://ovsa.njit.edu/eovsadata/)
* OVRO-LWA data portal: [ovsa.njit.edu/lwa](https://ovsa.njit.edu/lwa/)
* OVSA realtime: [ovsa.njit.edu/live](https://ovsa.njit.edu/live/)

---

## Space based instruments

### Solar Orbiter / RPW
* Solar Orbiter Archive (ESAC): [soar.esac.esa.int](https://soar.esac.esa.int/)
* RPW public data server (Paris Observatory): [rpw-lira.obspm.fr](https://rpw-lira.obspm.fr/roc/data/pub/solo/rpw/data/)
* RPW Web portal (Paris Observatory): [rpw-datacenter.obspm.fr](https://rpw-datacenter.obspm.fr)
* Tutorials and user-supplied software to read and plot RPW data: [rpw-datacenter.obspm.fr](https://rpw-datacenter.obspm.fr/spip.php?article15)

### Parker Solar Probe / FIELDS
* FIELDS data: [fields.ssl.berkeley.edu/data](https://fields.ssl.berkeley.edu/data/)
* PSP L3 (Berkeley): [research.ssl.berkeley.edu/data/psp](https://research.ssl.berkeley.edu/data/psp/data/sci/fields/l3/)
* PSP RFS on SPDF (HFR): [spdf.gsfc.nasa.gov/pub/data/psp/fields/l3/rfs_hfr](https://spdf.gsfc.nasa.gov/pub/data/psp/fields/l3/rfs_hfr/)
* PSP RFS on SPDF (LFR): [spdf.gsfc.nasa.gov/pub/data/psp/fields/l3/rfs_lfr](https://spdf.gsfc.nasa.gov/pub/data/psp/fields/l3/rfs_lfr/)

### STEREO / WAVES
* SWAVES resources page: [stereo-ssc.nascom.nasa.gov](https://stereo-ssc.nascom.nasa.gov/ins_resources/swaves_resources.shtml)
* STEREO L3 on SPDF: [spdf.gsfc.nasa.gov/pub/data/stereo/ahead/l3/waves](https://spdf.gsfc.nasa.gov/pub/data/stereo/ahead/l3/waves/)
* CDPP STEREO/WAVES portal: [cdpp-archive.cnes.fr (STEREO/WAVES)](https://cdpp-archive.cnes.fr/user/cdpp/modules/1756?mmv=MODE_3D&rd=TABLE&rf=URN%3AAIP%3ADATASET%3Acdpp%3A833edae2-8eef-48ca-864a-553837c985aa%3AV1&rt=DATA&t=MAIN_RESULTS)

### Wind / WAVES
* Wind/WAVES on CDAWeb: [cdaweb.gsfc.nasa.gov/pub/data/wind/waves](https://cdaweb.gsfc.nasa.gov/pub/data/wind/waves/)
* Pre 2008 Wind data: [themis.ssl.berkeley.edu/data/wind/wav](https://themis.ssl.berkeley.edu/data/wind/wav/)
* CDPP Wind/WAVES portal: [cdpp-archive.cnes.fr (Wind/WAVES)](https://cdpp-archive.cnes.fr/user/cdpp/modules/1766?d=URN%3AAIP%3ADATASET%3Acdpp%3A072b7f00-e15c-42c2-ae57-2301e9e714bc%3AV1&eds=PARAMETERS&mmv=MODE_3D&rd=TABLE&rf=URN%3AAIP%3ADATASET%3Acdpp%3A072b7f00-e15c-42c2-ae57-2301e9e714bc%3AV1&rt=DATA&t=MAIN_RESULTS)

---

## Quicklook and event browsers

When you want to see what was happening on a given day before committing to a download.

* [**CROCS**](https://parker.gsfc.nasa.gov/crocs.html). Combined Radio Observations of CMEs and Shocks. Multi instrument quicklooks for major events.
* [**e-CALLISTO quicklooks**](https://soleil.i4ds.ch/solarradio/callistoQuicklooks/). Daily, per station.
* [**UAH e-CALLISTO dashboard**](https://astrodoncel.uah.es/dashboard/index.php). Burst reports, statistics, cross matches.
* [**Solar KSP LOFAR previews**](https://spaceweather.astron.nl/SolarKSP/data/atdb_process/solar_bf_compressing/).
* [**ORFEE quicklooks**](https://secchirh.obspm.fr/spip.php?article11).

---

## Community portals and broader resources

* [**CDAWeb**](https://cdaweb.gsfc.nasa.gov/). NASA Coordinated Data Analysis Web. Central archive for many space physics CDF datasets.
* [**SPDF**](https://spdf.gsfc.nasa.gov/). NASA Space Physics Data Facility.
* [**CDPP**](https://cdpp-archive.cnes.fr/). French national space plasma data centre.
* [**IHDEA**](https://ihdea.net/). International Heliophysics Data Environment Alliance.
* [**DASH**](https://dash.heliophysics.net/). Distributed Archive for Solar and Heliospheric data.
* [**SunPy**](https://sunpy.org/). Python ecosystem for solar physics.

---

## Standards and metadata references

For instrument teams and data providers.

* [**FITS standard**](https://fits.gsfc.nasa.gov/standard40/fits_standard40aa-le.pdf). The FITS 4.0 specification.
* [**CDF documentation**](https://spdf.gsfc.nasa.gov/sp_use_of_cdf.html). Use of CDF in space physics.
* [**ISTP metadata guidelines**](https://github.com/IHDE-Alliance/ISTP_metadata). Conventions for CDF metadata in space physics.

---

## Workshop resources

* GitHub organisation: [github.com/LC-radio-workshop](https://github.com/LC-radio-workshop)
