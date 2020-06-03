# Transient warming levels for CMIP5 and CMIP6

Document the year when a certain warming level was reached in cmip5 and cmip6 data

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.3591807.svg)](https://doi.org/10.5281/zenodo.3591807)

> :warning: The list of authors may still change: please check the zenodo reference.

## Example
``` yaml
# warming level: 2.0°C above 1850-1900
warming_level_20:
- {model: AWI-CM-1-1-MR, ensemble: r1i1p1f1, exp: ssp126, start_year: 2041, end_year: 2060}
# {model: BCC-CSM2-MR, ensemble: r1i1p1f1, exp: ssp126} -- did not reach 2.0°C
- {model: CESM2-WACCM, ensemble: r1i1p1f1, exp: ssp126, start_year: 2029, end_year: 2048}
...
```

## Notes
 * **Warming levels**: 1.5 °C, 2.0 °C, 3.0 °C, and 4.0 °C global-mean annual-mean warming with respect to 1850 to 1900. Let me know if you require an additional warming level
 * For low warming levels and some models the period does already start in the `historical` simulation, for example:
``` yaml
# warming level: 1.5°C above 1850-1900
- {model: IPSL-CM6A-LR, ensemble: r1i1p1f1, exp: ssp126, start_year: 2011, end_year: 2030}
```

## Data
 * Monthly temperature data (variable: `tas`, Table ID: `Lmon`) for `historical` and
   * `RCP2.6`, `RCP4.5`, `RCP6.0`, and `RCP8.5` (CMIP5)
   * `SSP1-1.9`, `SSP1-2.6`, `SSP2-4.5`, `SSP3-7.0`, and `SSP5-8.5` (CMIP6)
 * One ensemble member per model and scenario. Let me know if you need other ensemble members.

## Method
To calculate the years when a certain `warming_level` (e.g. 1.5°C above pre-industrial) was first reached, the following method is used for every individual model:
 1. Calculate global mean temperature (weighted with the `cos` of the latitude)
 2. Calculate annual mean (Note: currently all months are weighted equally)
 3. Concatenate historical data and projection (constrained to the historical/ future time period)
 4. Subtract the mean over 1850 to 1900 (inclusive)
    * Models that start after 1850 are excluded
 5. Calculate 20-year centered average
 6. Find the first year where `T > warming_level`, this is the `central_year`
 7. From this the `start_year` and `end_year` is calculated as:
 ``` python
start_year = int(central_year - 20 / 2)
end_year = int(central_year + (20 / 2 - 1))
```

## Format

The data is given in [yaml](https://en.wikipedia.org/wiki/YAML) format; one file for all warming levels. For cmip6 data two files are provided once including the grid info (e.g. `grid: gn`), and once without. Let me know if you need the output in any other format.

## Code
The code will be published in https://github.com/IPCC-WG1 as soon as I have the right to create a repository.

## Background

Discussion of the design decisions.

### Time window
Ideally we can use 20-year windows to calculate and display the average warming levels, e.g. 2036 to 2055 for 2° C in model XY. Reasoning: This is consistent with the near-, mid- and long-term period, and with the period, which is used to define the stippling. The stippling uses standard deviations of 20-year means of the pre-industrial control simulations. This allows us to use the same definition of stippling. We also discussed to use 21-yr windows but it turns out that we would not be communicating the median year when the warming level is reached but the period, so that there is no obvious advantage of using uneven numbers of years. We also discussed to use 10-year windows but this would be more affected by internal variability and if it is used for stippling, there would potentially be little stippling for low warming levels, simply due to variability.

### Definition of warming levels
Warming levels are defined as the 20-year period for which the 20-year running mean of GSAT (tas) first exceeds a certain level of warming relative to 1850 to 1900. There is a slight chance that it may marginally drop below the level again but with 20-year we should be reasonably safe.

### Use of SSPs/ RCPs
We decided to use as many lines of evidence as possible and average across all SSPs/ RCPs in which a certain warming level is crossed. e.g. for 2° all models for SSP5-8.5, SSP3-7.0, SSP2-4.5, SSP1-2.6, while it is probably not reached in SSP1-1.9. Sonia Seneviratne and others have demonstrated that most things scale very well with levels of warming but ideally it can be quickly checked again for different variables e.g. annual mean precipitation, which may be somewhat forcing-dependent and where the pattern may be somewhat sensitive to highly transient exceedance of 2° (e.g. SSP5-8.5) vs. nearly equilibrated exceedance (e.g. SSP1-2.6). This can be easily checked by averaging the models across individual SSPs first. As Sonia Seneviratne demonstrated potential differences may well be in the range of internal variability and model uncertainty anyway.

### Inconsistent sets of models for different warming levels:
There are more models exceeding 2° than 4°C. Also here guidance should be to use all lines of evidence and include as many models as possible. In AR5 CH12 the number of models used for a graph was always added to the figure.

## History
### 03.06.2020
 * Removed BCC-ESM1 (SSP3-7.0) and MPI-ESM-1-2-HAM (SSP3-7.0) from the list of CMIP6 models because the simulations end in 2055.
 * Added five new CMIP6 models (CIESM, CNRM-CM6-1-HR, GISS-E2-1-G, KACE-1-0-G, NorESM2-MM).

### 03.02.2020
 * Added new models from the CMIP6 archive. A total of 16 models were added.
 * Removed MIROC5, MIROC-ESM-CHEM, MIROC-ESMn from the list of CMIP5 models due to a spurious temperature jump in the Western United States.

### 23.12.2019
 * create release 0.1.0

### 13.12.2019 
 * renamed files: added years of the climatology (no other change to the 1850-1900 files)
 * added warming level years with respect to 1995-2014

### 03.12.2019
 * Added warming levels of +0.61°C and +1.00°C

### 26.11.2019

 * ANOMALY_YR_START: 1851 -> 1850
 * Added SSP1-1.9
 * Add grid info for cmip6 models
 * Only provide one file for all warming levels.
 * Forgot to call the 20-year running mean.

### 25.11.2019

 * Initial publication.
