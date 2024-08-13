# ECOsystem Spaceborne Thermal Radiometer Experiment on Space Station (ECOSTRESS) Collection 3 Data Product Algorithms

This organization contains the algorithms for the ECOsystem Spaceborne Thermal Radiometer Experiment on Space Station (ECOSTRESS) mission collection 3 data products. 

This document will provide background information relevant to the ECOSTRESS mission and data products. 

The ECOSTRESS data product algorithms include:
- [Level 1 Radiance](https://github.com/ECOSTRESS-Collection-3/ECOv003-L1-RAD)
- [Level 2 Surface Temperature](https://github.com/ECOSTRESS-Collection-3/ECOv003-L2-LSTE)
- [Level 2 Vegetation Index & Albedo](https://github.com/ECOSTRESS-Collection-3/ECOv003-L2-STARS)
- [Level 3 Evapotranspiration](https://github.com/ECOSTRESS-Collection-3/ECOv003-L3-JET)
  - [Level 4 Evaporative Stress Index](https://github.com/ECOSTRESS-Collection-3/ECOv003-L3-JET)
  - [Level 4 Water Use Efficiency](https://github.com/ECOSTRESS-Collection-3/ECOv003-L3-JET)

## Evapotranspiration Models

The evapotranspiration models for the level 3 and 4 ecosystem products are being developed in the [JPL-Evapotranspiration-Algorithms](https://github.com/JPL-Evapotranspiration-Algorithms) organization. These models include:
- [Surface Temperature Initiated Closure (STIC)](https://github.com/JPL-Evapotranspiration-Algorithms/STIC)
- [Breathing Earth System Simulator (BESS)](https://github.com/JPL-Evapotranspiration-Algorithms/BESS)
- [Priestley Taylor Jet Propulsion Laboratory (PT-JPL)](https://github.com/JPL-Evapotranspiration-Algorithms/PT-JPL)
- [Priestley Taylor Jet Propulsion Laboratory Soil Moisture (PT-JPL-SM)](https://github.com/JPL-Evapotranspiration-Algorithms/PT-JPL-SM)

## STARS Data Fusion System

The STARS data fusion system supporting the auxiliary inputs for the ecosystem products is being developed in the [STARS-Data-Fusion](https://github.com/STARS-Data-Fusion) organization.

The Julia implementation for the STARS data fusion algorithm is in [STARS.jl](https://github.com/STARS-Data-Fusion/STARS.jl).

There are several supporting sub-components in generalized Julia packages, including:

- [SentinelTiles.jl](https://github.com/STARS-Data-Fusion/SentinelTiles.jl) for geo-referencing Sentinel UTM tiles
- [MODLAND.jl](https://github.com/STARS-Data-Fusion/MODLAND.jl) for geo-referencing MODIS/VIIRS sinusoidal tiles
- [CMR.jl](https://github.com/STARS-Data-Fusion/CMR.jl) for searching the Common Metadata Repository (CMR)
- [HLS.jl](https://github.com/STARS-Data-Fusion/HLS.jl) for searching and downloading the Harmonized Landsat Sentinel (HLS) dataset
- [VNP43NRT.jl](https://github.com/STARS-Data-Fusion/VNP43NRT.jl) for Bidirectional Reflectance Distribution Function (BRDF)
