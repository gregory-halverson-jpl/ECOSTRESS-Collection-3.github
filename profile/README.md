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

#### Cloud-Optimized GeoTIFF Orbit/Scene/Tile Format

All ECOSTRESS tiled products are stored in the Geographic Tagged Image File Format (GeoTIFF). GeoTIFF is a general purpose file format and programming library for storing scientific data. The GeoTIFF format was originally created by Dr. Niles Ritter with the Open Geospatial Consortium publishing the OGC GeoTIFF standard, which defines the GeoTIFF by specifying requirements and encoding rules for using the Tagged Image File Format (TIFF) for the exchange of georeferenced or geocoded image data. The following sections provide some key elements of GeoTIFF that will be employed in ECOSTRESS data products. 

The tiled products include the letter T in their level identifiers: L1CT, L2T, L3T, and L4T. The tiling system used for ECOSTRESS is borrowed from the modified Military Grid Reference System (MGRS) tiling scheme used by Sentinel 2. These tiles divide the Universal Transverse Mercator (UTM) zones into square tiles 109800 m across. SBG uses a 60 m cell size with 1830 rows by 1830 columns in each tile, totaling 3.35 million pixels per tile. This allows the end user to assume that each 60 m SBG pixel will remain in the same location at each timestep observed in analysis. The COG format also facilitates end-user analysis as a universally recognized and supported format, compatible with open-source software, including QGIS, ArcGIS, GDAL, the Raster package in R, `rioxarray` in Python,and `Rasters.jl` in Julia.

Each `float32` data layer occupies 4 bytes of storage per pixel, which amounts to an uncompressed size of 13.4 mb for each tiled data layer. The `uint8` quality flag layers occupy a single byte per pixel, which amounts to an uncompressed size of 3.35 mb per tiled data quality layer.

Each `.tif` COG data layer in each L2T/L3T/L4T product additionally contains a rendered browse image in GeoJPEG format with a `.jpeg` extension. This image format is universally recognized and supported, and these files are compatible with Google Earth. Each L2T/L3T/L4T tile granule includes a `.json` file containing the Product Metadata and Standard Metadata in JSON format.

Each ECOSTRESS tiled product in COG format will contain a standard metadata group that specifies the same type of contents for all products, and a product specific metadata group that specifies those metadata elements that are useful for defining attributes of the product data.  

Complete documentation of the GeoTIFF structure and application software can be found at https://www.ogc.org/standard/geotiff/.
