# Transient warming levels for CMIP5 and CMIP6

Document the year when a certain warming level was reached in cmip5 and cmip6 data

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.3591807.svg)](https://doi.org/10.5281/zenodo.3591807)

*Mathias Hauser*<sup>1</sup>, *Francois Engelbrecht*<sup>2</sup>, and *Erich Fischer*<sup>1</sup>

<sup>1</sup>Institute for Atmospheric and Climate Science, ETH Zurich, Zurich, Switzerland

<sup>2</sup>Global Change Institute, University of the Witwatersrand, Johannesburg, Gauteng 2193, South Africa


:warning: 13.01.2020: Re-processed all data: some numbers have changed (see History section below).

## Example
``` yaml
# warming level: 2.0°C above 1850-1900
warming_level_20:
- {model: AWI-CM-1-1-MR, ensemble: r1i1p1f1, exp: ssp126, start_year: 2041, end_year: 2060}
# {model: BCC-CSM2-MR, ensemble: r1i1p1f1, exp: ssp126} -- did not reach 2.0°C
- {model: CESM2-WACCM, ensemble: r1i1p1f1, exp: ssp126, start_year: 2029, end_year: 2048}
...
```

The data is also available in csv files:
``` csv
model, ensemble, exp, warming_level, start_year, end_year
CAMS-CSM1-0, r1i1p1f1, ssp119, 1.0, 2017, 2036
CNRM-ESM2-1, r1i1p1f2, ssp119, 1.0, 2007, 2026
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

### 19.01.2021

* CMIP5: removed CESM1-CAM5-1-FV2 (r1i1p1, RCP4.5 and RCP8.5) because of a missing month in the data. Found two more models with changes du to the weighting with "areacella" - CCSM4 (r1i1p1, RCP85) and CSIRO-Mk3-6-0 (r4i1p1, RCP45). They were previously missed because the grid of the weights and the data does not exactly align.

* CMIP6: added EC-Earth3-CC (r1i1p1f1, several SSPs)



### 13.01.2021

* Recomputed all models & used areacella as area-weights (instead of the cosine of the latitude). This lead to differences for some models between the old numbers and the new ones. See the tables below:

#### CMIP6

Model         | Ensemble     | Scenario | Reason
------------- | ---------    | -------- | ------
CanESM5       | r17i1p2f1    | SSP2-4.5 | small difference in weights<sup>\*</sup>
CanESM5       | r18i1p2f1    | SSP2-4.5 | small difference in weights<sup>\*</sup>
CanESM5       | r2i1p2f1     | SSP3-7.0 | small difference in weights<sup>\*</sup>
CanESM5       | r5i1p1f1     | several  | small difference in weights<sup>\*</sup>
CanESM5-CanOE | r2i1p2f1     | SSP3-7.0 | small difference in weights<sup>\*</sup>
CESM2         | r10i1p1f1    | SSP1-2.6 | missing data
CIESM         | **r1i1p1f1** | several  | historical simulation was replaced
CNRM-CM6-1-HR | r1i1p1f2     | SSP5-8.5 | small difference in weights<sup>\*</sup>
EC-Earth3     | r7i1p1f1     | SSP2-4.5 | missing data
EC-Earth3-Veg | r3i1p1f1     | SSP3-7.0 | small difference in weights<sup>\*</sup>
FGOALS-g3     | r2i1p1f1     | SSP1-2.6 | data after 2015 was NA
FIO-ESM-2-0   | r2i1p1f1     | several  | historical simulation was replaced
IITM-ESM      | **r1i1p1f1** | several  | missing data
IPSL-CM6A-LR  | r6i1p1f1     | SSP5-8.5 | small difference in weights<sup>\*</sup>
KACE-1-0-G    | r3i1p1f1     | several  | historical simulation was replaced
NorESM2-LM    | r2i1p1f1     | SSP2-4.5 | small difference in weights<sup>\*</sup>
TaiESM1       | **r1i1p1f1** | several  | historical simulation was replaced
UKESM1-0-LL   | r18i1p1f2    | SSP1-2.6 | missing data

* New ensemble members CESM2, EC-Earth3, GISS-E2-1-G, UKESM1-0-LL, EC-Earth3-Veg, and MIROC-ES2L.

#### CMIP5

Model      | Ensemble   | Scenario | Reason
---------- | --------   | -------- | ------
FGOALS-g2  | **r1i1p1** | several  | large difference in weights<sup>\*</sup>
GFDL-ESM2G | **r1i1p1** | RCP2.6   | small difference in weights<sup>\*</sup>
NorESM1-M  | **r1i1p1** | RCP8.5   | small difference in weights<sup>\*</sup>

 <sup>\*</sup>at high latitudes

* Removed EC-EARTH (r3i1p1, RCP4.5 & RCP8.5) due to missing years in the data.

### 11.12.2020
 * Add Models EC-Earth3-Veg-LR, TaiESM1, E3SM-1-1 (new scenarios) CMIP6 models. Add ensemble members for CNRM-ESM2-1, EC-Earth3-Veg-LR, TaiESM1, and E3SM-1-1.

### 23.11.2020
 * Add IITM-ESM, EC-Earth3-AerChem, TaiESM1 (new scenarios) CMIP6 models. Add ensemble members for ACCESS-ESM1-5, FGOALS-g3, CNRM-ESM2-1, GISS-E2-1-G.

### 08.10.2020
 * Add CMCC-CM2-SR5, KIOST-ESM, and TaiESM1 CMIP6 models. Also more ensemble members for GISS-E2-1-G, MIROC6, and UKESM1-0-LL. 

### 05.10.2020
 * Add the inofficial (not-cmorized, not on ESGF) EC-EARTH r3i1p1 ensemble (source: http://ensemblesrt3.dmi.dk/data/CMIP5/r3i1p1/).

### 01.10.2020
 * Adding MIROC5 to CMIP5 again (per request).

### 21.09.2020
 * Added the start years of all models starting after 1850.

### 18.09.2020
 * added warming level years w.r.t. 1850-1900 for CMIP5 including models starting *after* 1850 (no_bounds_check)
 * added warming level years w.r.t. 1861-1900 for CMIP5 (because a some models only start after 1850)
 * Some models starting after 1850 were missing from the 1995-2014-files because they were already filtered when loading the data
   (and not when calculating the anomaly)
 * Added the reference period to the header of the files

### 17.09.2020
 * Added all yml and csv files for all avaliable ensemble members in cmip5_all_ens and cmip6_all_ens.

### 15.09.2020
 * Note, the following CMIP5 models are not included because they start after 1850: GFDL-CM3 (1860), GFDL-ESM2G (1861), GFDL-ESM2M (1861), HadGEM2-AO (1860), HadGEM2-CC (1859), HadGEM2-ES (1859), and MRI-ESM1 (1851).

### 27.08.2020
 * added csv files with the same information

### 27.07.2020

 * Added FGOALS-g3 (SSP1-1.9), HadGEM3-GC31-MM, and CMCC-CM2-SR5 (several SSPs)
 * Added acknowledgment and licensing of CMIP6 models.

### 26.07.2020

 * Added acknowledgment, licensing and list of CMIP5 models.

### 09.06.2020
 * Added AWI-CM-1-1-MR as it now includes the year 1850 (was 1851 previously).

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

## License
 
### CMIP5 Data License Statement
 
The underlying raw data are published under the license of CMIP5. Terms of use for CMIP5 are applied for the derived data presented here. They are provided at https://pcmdi.llnl.gov/mips/cmip5/terms-of-use.html. Data from some modelling centres are licensed for use in non-commercial research and for educational purposes, other for unrestricted use. Please refer to the [terms of use for the CMIP5 modeling groups](https://pcmdi.llnl.gov/mips/cmip5/docs/CMIP5_modeling_groups.pdf) for details.
The data should be cited by its DOI and according to the [citation recommendation of CMIP5](https://pcmdi.llnl.gov/mips/cmip5/citation.html). 

### CMIP6 Data License Statement

The raw CMIP6 data are licensed under Creative Commons [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0), following the [CMIP6 Terms of Use](https://pcmdi.llnl.gov/CMIP6/TermsOfUse). Note that some models publish the data under a non-commercial license ([CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)). The license for each particular model dataset can be obtained at the [ESGF CoG portal](https://esgf-node.llnl.gov/search/cmip6) as part of the data citation information.

## Acknowledgment

We thank Urs Beyerle for downloading and curating the CMIP5 and CMIP6 data at the Institute for Atmospheric and Climate Science, ETH Zurich, Zurich, Switzerland.

### CMIP5

We acknowledge the World Climate Research Programme's Working Group on Coupled Modelling, which is responsible for CMIP, and we thank the climate modeling groups (listed in [cmip5_models.md](cmip5_models.md)) for producing and making available their model output. For CMIP the U.S. Department of Energy's Program for Climate Model Diagnosis and Intercomparison provides coordinating support and led development of software infrastructure in partnership with the Global Organization for Earth System Science Portals.”

### CMIP6

We acknowledge the World Climate Research Programme, which, through its Working Group on Coupled Modelling, coordinated and promoted CMIP6. We thank the climate modeling groups for producing and making available their model output, the Earth System Grid Federation (ESGF) for archiving the data and providing access, and the multiple funding agencies who support CMIP6 and ESGF.
