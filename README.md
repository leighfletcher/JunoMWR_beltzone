# Juno MWR Belt/Zone Study
Supplemental material for Fletcher et al. (2021) Juno MWR belt/zone analysis:

*"Jupiter's Temperate Belt/Zone Contrasts Revealed at Depth by Juno Microwave Observations"*

L.N. Fletcher, F.A. Oyafuso, M. Allison, A. Ingersoll, L. Li, Y. Kaspi, E.Galanti, M.H. Wong, G.S. Orton, K. Duer, Z. Zhang, C. Li, T. Guillot, S.M. Levin, S. Bolton

[![DOI](https://zenodo.org/badge/337518717.svg)](https://zenodo.org/badge/latestdoi/337518717)


Note:  We use H5 files to store data in a Hierarchical Data Format (HDF5). Each file contains multidimensional arrays of scientific data. [HDFView](https://www.neonscience.org/resources/learning-hub/tutorials/explore-data-hdfview) can be used to explore the data, or this can be read in Python by:

`import h5py`
`hf = h5py.File('nadirTB_submit.h5', 'r')`
`hf.keys()`

[IDL](https://www.l3harrisgeospatial.com/docs/HDF5_Overview.html) also provides access to HDF5 files via a set of procedures and functions contained in a dynamically loadable module (DLM).

## MWR Limb-Darkened Brightness temperatures
File: `TBmu_submit.h5`

Fig. 1 and 2 of Fletcher+2021, `MWRbeltzone.png` and `limbdarken_mean.png`

MWR brightness temperatures calculated for six channels, 255 latitudes, and 90 emission angles, based on the quadratic functional form in Fletcher+2021 and shown in `limbdarken_mean.png`.  The HDF file contains `TBmu` for 6 channels, 255 latitudes (`latitudes`), and 91 angles (`emissionangle`).  We also include the three weighted average coefficients (c0, c1, c2) for the limb darkening for 6 channels and 255 latitudes.  Only the nadir coefficients were then used to generated Fig. 1.

## MWR Nadir Brightness Temperatures
File: `nadirTB_submit.h5`

Fig. 3 of Fletcher+2021 and `nadirTB.png`

Contains the MWR brightness temperatures for all six channels and nine perijoves as an array called `nadirTB` with an associated `latitudes` and `nadirchisq` array, the latter parameter is based on the deconvolution process of Oyafuso et al. (2020).  A weighted average of the perijoves for each channel is provided as `meanTB`, with an associated `meanlat` latitude grid and standard deviation `stddevTB`.

## MWR Nadir Pseudoshear
File: `nadirpseudoshear_submit.h5`

Fig. 4 of Fletcher+2021 and `nadir_dudz.png`.

Contains the pseudoshear (i.e., the microwave brightness temperature gradient adjusted for gravitational acceleration and Coriolis parameter) for each of the six MWR channels as `dudz` array, with an associated `meanlat` array.

## Cloud-top Winds
File:  `cassiniwinds_submit.h5`

Fig. 5 of Fletcher+2021

Contains the Cassini/ISS cloud-top zonal winds from Porco et al. (2003), as the `winds` array with corresponding `windlat` array giving planetocentric latitudes.  Latitudes of prominent eastward (`eastjets`) and westward (`westjets`) jets are also provided.  These cloud-top winds were correlated with the nadir pseudoshear in Fig. 5 of Fletcher+2021.  Alternative wind correlations in the Supplementary Material use `hubblewinds2017_tollefson.txt` for Feb 2017 (Tollefson et al., 2017) and `hubblewinds2019_wong.txt` for June 2019 (Wong et al., 2020).

## Contribution Functions
File: `contfn_truenh3_submit.h5` and `contfn_default_submit.h5` **Warning - 300MB files**

Fig. 7 of Fletcher+2021, and `contfn_truenh3.png` and `contfn_default.png`

JAMRT code used to estimate the contribution function variation for all six channels on 17 latitudes (`latitudes`) and a high-resolution pressure grid (`pressure`) for 160 emission angles (`emissionangle`).  We used two different approaches for the ammonia abundance:  "truenh3" uses the retrieved ammonia abundances from Guillot et al. (2020), based on the inversion technique of Li et al. (2017); "default" uses a latitudinally-uniform deep abundance, decreasing at higher altitudes via the condensation of cloud decks.  Within each HDF file, we use two opacity models (Hanley et al., 2009 and Belotti et al., 2016) to produce `cf_hanley` and `cf_bellotti`.  The peak of the contribution function is measured in three regions (north 20N-40N, equator 5N-5S, and south 20S-40S), included as `cfpeak` arrays.  Finally, the north and south arrays are averaged to produce `meanpeaks` for each of the six MWR channels, and this is used in Fletcher+2021 to reproject brightness temperatures to a pressure grid.

## Brightness Temperature Cross-Section
File:  `TBxsection_submit.h5`

Fig. 8-9 of Fletcher+2021, `TBxsection.png`

Reprojection of the MWR brightness temperatures using the contribution functions to assign unique pressures to each emission angle.  Results in a cross-section of brightness temperatures (`TBpress`) on a latitude (`latitudes`) and pressure (`pressure`) grid.  Note that these are not kinetic temperatures.

## Gravity and Height Grids
File: `gravheight_submit.h5`

Fig:  `gravity_calc.png`

Integration of the thermal wind equation, along with the original calculation of the pseudoshears, requires the calculation of altitude grids and gravitational acceleration as a function of latitude and pressure, as described in our supplemental materials.  HDF file contains `pressure`, `latitudes`, `grav` and `height` grids for use in subsequent calculations.

## Pseudoshear Cross-Section
File:  `dudzxsection_submit.h5`

Fig. 10 of Fletcher+2021 and `MWRpseudoshear_jets.png`

Measurement of the pseudoshear from the reprojected brightness-temperature grid, using the appropriate gravitational acceleration.  HDF file contains the pseudoshear `dudz` as a function of `latitudes` and `pressure`.  Note that these pseudoshears are over-estimates of the true windshear.

## Dry Thermal Winds
File: `drywinds_xsection_submit.h5`

Fig. 11 of Fletcher+2021 and `MWRpseudowinds_jets.png`

Integrated thermal winds based on the pseudoshears, assuming the brightness temperatures represent kinetic temperatures.  HDF file contains the zonal wind cross-section `utherm` on a grid of `pressure` and `latitudes`.  Integration used the altitude grid above.

## Moist Thermal Winds
Files: `moistwinds_nh3_submit.h5` and `moistwinds_h2o_submit.h5`

Fig. 12 of Fletcher+2021, `nh3windshear.png` and `h2owindshear.png`

Calculating windshears based on molecular abundance gradients rather than kinetic temperature gradients.  Ammonia cross-sections are from Guillot et al. (2020), based on inversions by Li et al. (2017), and are included as `nh3` on a `pressure` and `latitudes` grid, alongside the calculated `nh3shear` as described in the paper.  We provide the same calculations of `h2o` and `h2oshear`, assuming that that water follows the same latitudinal dependence as `nh3.`
