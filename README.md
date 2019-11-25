# Transient warming levels for CMIP5 and CMIP6

year when a certain warming level was reached in cmip5 and cmip6 data

> :warning: This is **not** an official repository and it may change at any time. 

## Data
 * Monthly temperature data (variable: `tas`, Table ID: `Lmon`) for `historical` and
   * `RCP2.6`, `RCP4.5`, `RCP6.0`, and `RCP8.5` for CMIP5
   * `SSP1-2.6`, `SSP4-4.5`, `SSP3-7.0`, and `SSP5-8.5` for CMIP6

## Method
To calculate the years when a certain `warming_level` (e.g. 1.5°C above pre-industrial) was first reached, the following is method is used for every individual model (currently one ensemble member per model):
 1. Calculate global mean temperature (weighted with the `cos` of the latitude)
 2. Calculate annual mean (Note: currently all months are weighted equally)
 3 Concatenate historical data and projection (constrainded to the historical/ future time period)
 4. Subtract the mean over 1850 to 1900 (inclusive)
    * Models that start after 1850 are excluded
 5. Calculate 20-year centered average
 6. Find the first year `T > warming_level`, this is the `central_year`
 7. From this the `start_year` and `end_year` is calculated as:
 ```
start_year = int(central_year - 20 / 2)
end_year = int(central_year + (20 / 2 - 1))
```

## Format

Currently the data is given in [yaml](https://en.wikipedia.org/wiki/YAML) format. Let me know if you want the output in any other format.

## Code
The code will be published in https://github.com/IPCC-WG1 as soon as I have the right to create a repository.
