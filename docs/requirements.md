# Requirements for a Heliospheric Radio Dynamic Spectra Standard

*A tidy summary of the requirements gathered at the Lorentz Center workshop "Bridging Gaps in Heliospheric Radio Data Analyses" (Leiden, 18 to 22 May 2026).*

---

## 1. Purpose and scope

This document captures the requirements for a standardised approach to heliospheric radio dynamic spectra, covering both ground based and space based instruments. It spans three related things: the metadata that data files should carry, the file format choices for storing that data, and the in-memory container (and surrounding software) that loads it. It is the result of discussions among workshop participants and reflects a shared understanding of what is needed to enable consistent, multi instrument analysis.

The document is organised into three sections:

1. **Metadata requirements**: what information should be carried in the data files themselves.
2. **User wish list**: what scientists want to do with the data once loaded.
3. **Container, file format, and software requirements**: what the in-memory data object, the on-disk file format, and the surrounding tooling must provide.

A note on layering at the end distinguishes between the container itself and the scientific analysis routines that consume it.

---

## 2. Metadata requirements

Every dynamic spectra data file should carry the following metadata, with consistent keyword names across instruments where possible. This is the information that the file format must accommodate and that the in-memory container will expose to the user.

### 2.1 Identification

* Observer or instrument name
* Project or instrument URL
* Contact information for attribution or publication
* Source filename
* Processing level
* Data file version

### 2.2 Time

A time axis with explicit format and epoch system is essential.

* Observation date
* Start time and end time of the observation
* Temporal resolution, both raw and current (post processing), as a scalar or an array if variable

### 2.3 Frequency

A frequency axis with units is essential.

* Start and end frequencies of the observation
* Frequency resolution, both raw and current, as a scalar or an array if variable

### 2.4 Calibration and signal

* Units of the data (for example arbitrary, V²/Hz, sfu, dB)
* Calibration state (raw, calibrated, normalised to a reference distance)
* For spacecraft: signal as measured at the observer position **and** signal normalised to a reference distance such as 1 au (both retained)
* Stokes parameter convention (for example IQUV, or left and right handed circular polarisation)
* Background subtraction state and method, where applied
* Dynamic range or saturation thresholds

### 2.5 Quality

* Data mask
* Quality flags describing degraded or limited data
* Interpolation flags indicating which data points have been interpolated, in time and in frequency

### 2.6 Position and pointing

All instruments should record the observer location, in a representation compatible with standard coordinate frames in the Python scientific ecosystem.

**Ground based instruments additionally require:**

* RA and Dec pointing
* Beam area
* Time at solar peak elevation

**Spacecraft additionally require:**

* Observer position (typically heliocentric x, y, z coordinates)
* Attitude or orientation (roll, pitch, yaw, or sun pointing angle)

Spacecraft attitude and position typically come from SPICE kernels and the container should accommodate this.

---

## 3. User wish list

This section describes what scientists want to do with the data. It is intentionally broader than what the container itself provides, and includes operations that may live in higher level analysis routines that consume the container (see section 5).

### 3.1 Visualisation

Users want to plot a dynamic spectrum with sensible defaults (axes, colorbar, labels), handle missing data and NaN values gracefully on display, normalise the colour scale (for example log scaling), stretch the colour table, control minimum and maximum values, and log either axis. They also want to produce stack plots of multiple spectra with harmonised dynamic range across panels.

### 3.2 Basic operations

Users want to:

* Slice a spectrum in time at a chosen frequency, or in frequency at a chosen time
* Crop to a sub region in time, frequency, or both
* Rebin in time or frequency
* Subtract a background using common methods (constant, scalar, per channel quiet time, running median)
* Handle masks and interpolate over bad data
* Flag radio frequency interference

The underlying data array should be accessible (for example via a `.data` attribute) but users should be encouraged to use container methods rather than operating on the array directly.

### 3.3 Multi instrument operations

Users want to:

* Combine spectra from multiple instruments onto a common grid
* Produce stack plots with a shared time axis
* Time shift spectra by light travel time using observer position
* Renormalise to a reference distance such as 1 au

The container should handle frequency and temporal overlaps when merging sources in a defined and documented way. In practice, the most common workflow is to prefer the better source over the overlap region and then slice or crop, rather than averaging.

### 3.4 Scientific analysis

Users want to:

* Extract the maximum frequency as a function of time
* Fit drift rates (one or two dimensional)
* Characterise flux variation per frequency and per time
* Fit rise and decay times of emission
* Determine burst start times per frequency
* Compute ΔI/I as a function of frequency
* Identify peak intensity per frequency over a chosen interval
* Define regions of interest (for example by drawing around features)
* Calculate fluence for frequency specific lightcurves
* Track the plasma line
* Detect periodicities or wave like patterns in extracted lightcurves
* Fit physical models such as gyrosynchrotron emission to extracted regions

### 3.5 Cross dataset and larger scale studies

Users want to:

* Compare radio data with external lightcurves (for example X ray or EUV)
* Perform triangulation and directivity studies using multiple spacecraft
* Run sliding window operations across long time spans
* Compute large scale statistics across many events

### 3.6 A note on these wish list items

The items in sections 3.4 and 3.5 are real user needs. However, most of them are best implemented as analysis routines that consume the container rather than as part of the container itself. The container should make these analyses easy to write, without owning them all. This distinction is developed further in section 5.

---

## 4. Container, file format, and software requirements

This section captures what the in-memory container, the on-disk file format, and the surrounding software must provide, written generically.

### 4.1 Data model

* Multi dimensional, WCS aware object capable of holding at least a two dimensional array indexed by time and frequency, with the possibility of additional axes (for example polarisation or Stokes).
* Quantity aware: units must be carried through operations.
* Time aware: proper handling of astronomical time formats.
* Mask aware: consistent NaN and explicit mask handling across operations.
* Metadata as a first class attribute, accessible through a consistent interface.

### 4.2 Construction

* A single entry point that takes a file and returns a populated container, dispatching on file format (FITS, CDF, HDF5, DAT, and binary formats).
* Construction from arrays (data, time axis, frequency axis, metadata) for programmatic use and testing.
* Construction from a URL where streaming is preferable to permanent download.

### 4.3 Core operations

The container should support the following as native methods that preserve the container type:

* Slicing by world coordinate in time, frequency, or both
* Cropping in time, frequency, or both
* Rebinning in time and frequency, with unit aware aggregation
* Quantity aware arithmetic (supporting background subtraction via standard operators)
* Mask handling (setting, modifying, interpolating)
* Reprojection or resampling onto a target grid (essential for multi instrument work)
* Round trip input and output to a standard format (FITS or HDF5) preserving metadata losslessly

### 4.4 Heterogeneous and multi resolution data

* Accommodate datasets where the same source produces multiple resolutions in the same file (for example certain WIND data products), most likely via a sequence of containers rather than a single combined array.
* Behaviour when combining containers with mismatched time or frequency grids must be defined and documented.
* Regridding onto a common grid should not be forced unless the user explicitly requests it.

### 4.5 Search and retrieval

* Discoverable through a unified search interface (for example Fido style) for instruments whose archives support this.
* For instruments without such support, the manual or scripted retrieval path should be documented.
* The path from "I want data for this event" to "loaded container" should be one or two lines of code wherever archive support exists.

### 4.6 Integration

* Interoperable with the broader scientific Python ecosystem (Astropy, NumPy, SciPy, xarray where appropriate).
* Compatible with downstream analysis tools for plotting, fitting, and machine learning workflows.

### 4.7 Documentation and quality

* An example gallery covering the main use cases (identified as a major community need).
* Per instrument notes documenting quirks, data levels, version differences, and known issues.
* Clear distinction in documentation between container operations and scientific analysis routines.

---

## 5. A note on layering
The user wish list mixes container level features (slicing, cropping, plotting, masking) with scientific analysis features (drift fitting, fluence calculation, periodicity detection). When implementing, these should be kept separate:

* **The container** holds the data and metadata and provides the basic operations described in section 4. Its scope is finite and well defined.
* **Analysis routines** consume the container to perform science such as fitting, region of interest extraction, and statistics. These belong in a higher level module (or modules) that use the container rather than within the container itself. This layer can grow organically as the community contributes new methods.

This separation keeps each layer maintainable and makes it possible for scientists with different specialisms to contribute methods at the analysis layer without needing to touch the core container.
