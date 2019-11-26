# Transient warming levels for CMIP5 and CMIP6

Document the year when a certain warming level was reached in cmip5 and cmip6 data

> :warning: This is **not** an official repository and it may change at any time. 

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

## History

### 26.11.2019

 * ANOMALY_YR_START: 1851 -> 1850
 * Added SSP1-1.9
 * Add grid info for cmip6 models
 * Only provide one file for all warming levels.
 * Forgot to call the 20-year running mean.

### 25.11.2019

 * Initial publication.
