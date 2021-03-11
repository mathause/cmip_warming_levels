# Global warming levels for CMIP5 and CMIP6

Document the year when a certain global warming level was reached in cmip5 and cmip6 data. Check the [cmip temperatures](https://github.com/mathause/cmip_temperatures) repository for a list of temperature anomalies for time periods in cmip5 and cmip6 data.

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.3591807.svg)](https://doi.org/10.5281/zenodo.3591807)

*Mathias Hauser*<sup>1</sup>, *Francois Engelbrecht*<sup>2</sup>, and *Erich Fischer*<sup>1</sup>

<sup>1</sup>Institute for Atmospheric and Climate Science, ETH Zurich, Zurich, Switzerland

<sup>2</sup>Global Change Institute, University of the Witwatersrand, Johannesburg, Gauteng 2193, South Africa


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

## Disclaimer

This data archive is created and maintained in an voluntary effort by its creators. The data are provided without warranty of any kind. Please note that the ownership of all files in the archive remains with the original providers! That means you should still acknowledge the CMIP6 data providers. This work is published under a [CC BY-SA 4.0](http://creativecommons.org/licenses/by-sa/4.0/) license by the authors. If you use the this archive please cite [10.5281/zenodo.3591806](https://doi.org/10.5281/zenodo.3591806).


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
 * Grid-cell area for atmospheric grid variables (`areacella`)

## Method
To calculate the years when a certain `warming_level` (e.g. 1.5°C above pre-industrial) was first reached, the following method is used for every individual model:
 1. Calculate global mean temperature (weighted with the grid cell area (`areacella`) if available, else the `cos` of the latitude is used)
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

## Changelog

The changelog of the repository can be found under [CHANGELOG.md](CHANGELOG.md).


## License

CMIP WARMING LEVELS is licensed under a Creative Commons Attribution-ShareAlike 4.0 International License.

A copy of the license is availabe at [LICENSE](LICENSE). If not, see [CC BY-SA 4.0](http://creativecommons.org/licenses/by-sa/4.0/).


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
