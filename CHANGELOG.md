# Changelog

## History

### 11.03.2021

* Moved history to own file.


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
