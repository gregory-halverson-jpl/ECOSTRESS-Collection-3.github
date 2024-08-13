# ECOsystem Spaceborne Thermal Radiometer Experiment on Space Station (ECOSTRESS) Collection 3 Data Product Algorithms

This organization contains the algorithms for the ECOsystem Spaceborne Thermal Radiometer Experiment on Space Station (ECOSTRESS) mission collection 3 data products. 

ECOSTRESS collection 3 is the precursor to the [Surface Biology and Geology Thermal Infrared (SBG-TIR) Orbiting Terrestrial Thermal Emission Radiometer (OTTER) data products](https://github.com/sbg-tir).

This document will provide background information relevant to the ECOSTRESS mission and data products. 

## ECOSTRESS Data Product Algorithms

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

## Cloud-Optimized GeoTIFF Orbit/Scene/Tile Format

All ECOSTRESS tiled products are stored in the Geographic Tagged Image File Format (GeoTIFF). GeoTIFF is a general purpose file format and programming library for storing scientific data. The GeoTIFF format was originally created by Dr. Niles Ritter with the Open Geospatial Consortium publishing the OGC GeoTIFF standard, which defines the GeoTIFF by specifying requirements and encoding rules for using the Tagged Image File Format (TIFF) for the exchange of georeferenced or geocoded image data. The following sections provide some key elements of GeoTIFF that will be employed in ECOSTRESS data products. 

The tiled products include the letter T in their level identifiers: L1CT, L2T, L3T, and L4T. The tiling system used for ECOSTRESS is borrowed from the modified Military Grid Reference System (MGRS) tiling scheme used by Sentinel 2. These tiles divide the Universal Transverse Mercator (UTM) zones into square tiles 109800 m across. SBG uses a 60 m cell size with 1830 rows by 1830 columns in each tile, totaling 3.35 million pixels per tile. This allows the end user to assume that each 60 m SBG pixel will remain in the same location at each timestep observed in analysis. The COG format also facilitates end-user analysis as a universally recognized and supported format, compatible with open-source software, including QGIS, ArcGIS, GDAL, the Raster package in R, `rioxarray` in Python,and `Rasters.jl` in Julia.

Each `float32` data layer occupies 4 bytes of storage per pixel, which amounts to an uncompressed size of 13.4 mb for each tiled data layer. The `uint8` quality flag layers occupy a single byte per pixel, which amounts to an uncompressed size of 3.35 mb per tiled data quality layer.

Each `.tif` COG data layer in each L2T/L3T/L4T product additionally contains a rendered browse image in GeoJPEG format with a `.jpeg` extension. This image format is universally recognized and supported, and these files are compatible with Google Earth. Each L2T/L3T/L4T tile granule includes a `.json` file containing the Product Metadata and Standard Metadata in JSON format.

Each ECOSTRESS tiled product in COG format will contain a standard metadata group that specifies the same type of contents for all products, and a product specific metadata group that specifies those metadata elements that are useful for defining attributes of the product data.  

Complete documentation of the GeoTIFF structure and application software can be found at https://www.ogc.org/standard/geotiff/.

## Quality Flags

Two high-level quality flags are provided in all gridded and tiled products as thematic/binary masks encoded to zero and one in unsigned 8-bit integer layers. The cloud layer represents the final cloud test from L2 CLOUD. The water layer represents the surface water body in the Shuttle Radar Topography Mission (SRTM) Digital Elevation Model. For both layers, zero means absence, and one means presence. Pixels with the value 1 in the cloud layer represent detection of cloud in that pixel. Pixels with the value 1 in the water layer represent open water surface in that pixel. All tiled product data layers written in `float32` contain a standard not-a-number (`NaN`) value at each pixel that could not be retrieved. The cloud and water layers are provided to explain these missing values.

## Standard Metadata 

| **Name** | **Type** | **Size** | **Example** |
| --- | --- | --- | --- |
| AuxiliaryInputPointer | String | variable | Group name of ancillary file list | 
| AutomaticQualityFlag | String | variable | PASS/FAIL (of product data) |
| BuildId | String | variable | |
| CollectionLabel | String | variable | |
| DataFormatType | String | variable | NCSAHDF5 |
| DayNightFlag | String | variable | |
| EastBoundingCoordinate | LongFloat | 8 | |
| HDFVersionId | String | variable | 1.8.16 |
| ImageLines | Int32 | 4 | 5632 |
| ImageLineSpacing | Float32 | 4 | 68.754 | 
| ImagePixels | Int32 | 4 | 5400 |
| ImagePixelSpacing | Float32 | 4 | 65.536 |
| InputPointer | String | variable | |
| InstrumentShortName | String | variable | SBG |
| LocalGranuleID | String | variable | --- |
| LongName | String | variable | SBG |
| InstrumentShortName | String | variable | --- |
| LocalGranuleID | String | variable | --- |
| LongName | String | variable | SBG |
| NorthBoundingCoordinate | LongFloat | 8 | --- |
| PGEName | String | variable | L2_LSTE (L2_CLOUD) |
| PGEVersion | String | variable | |
| PlatformLongName | String | variable | |
| PlatformShortName | String | variable | |
| PlatformType | String | variable | Spacecraft |
| ProcessingLevelID | String | variable | 1 |
| ProcessingLevelDescription | String | variable | Level 2 Land Surface Temperatures and Emissivity (Level 2 Cloud mask) |
| ProducerAgency | String | variable | JPL |
| ProducerInstitution | String | variable | Caltech |
| ProductionDateTime | String | variable | |
| ProductionLocation | String | variable | |
| CampaignShortName | String | variable | Primary |
| RangeBeginningDate | String | variable | |
| CampaignShortName | String | variable | |
| RangeBeginningDate | String | variable | |
| RangeBeginningTime | String | variable | |
| RangeEndingDate | String | variable | |
| RangeEndingTime | String | variable | |
| SceneID | String | variable | |
| ShortName | String | variable | L2_LSTE (L2_CLOUD) |
| SceneID | String | variable | |
| ShortName | String | variable | |
| SISName | String | variable | |
| SISVersion | String | variable | |
| SouthBoundingCoordinate | LongFloat | 8 | |
| StartOrbitNumber | String | variable | |
| StartOrbitNumber | String | variable | |
| WestBoundingCoordinate | LongFloat | 8 | |

*Table 3. Standard metadata included in SBG-TIR product files*

## Appendix of Abbreviations and Acronyms

| **Abbreviatios** | **Description** |
| --- | --- |
| ALEXI	| Atmospheric-Land Exchange Inversion |
| ARS	| Agricultural Research Service |
| ASD	| Algorithm Specifications Document |
| ATBD	| Algorithm Theoretical Basis Document |
| BESS | Breathing Earth System Simulator |
| C | Celsius |
| CCB	| Change Control Board |
| CDR	| Critical Design Review |
| CF	| Climate and Forecast (metadata convention) |
| CM	| Configuration Management |
| CONUS	| Continental United States |
| COTS	| Commercial Off The Shelf |
| DAAC	| Distributed Active Archive Center |
| BOA | Bottom of Atmosphere
| dB	| DeciBel |
| DCN	| Document Change Notice |
| deg	| Degrees |
| deg/sec	| Degrees per Second |
| DEM	| Digital Elevation Model |
| DisALEXI	| ALEXI Disaggregation algorithm |
| DN	| Data Number |
| EASE	| Equal Area Scalable Earth |
| ECI	| Earth Centered Inertial coordinate system |
| ECR	| Earth Centered Rotating coordinate system |
| ECS	| EOSDIS Core System |
| EOS	| Earth Observing System |
| EOSDIS	| EOS Data and Information System |
| ESDIS	| Earth Science Data and Information System |
| ESDT	| Earth Science Data Type |
| FOV	| Field of View |
| FSW	| Flight Software |
| GB	| gigabytes, 109 bytes |
| GDS	| Ground Data System |
| GHA	| Greenwich Hour Angle |
| GHz	| $$\text{Gigahertz, 10}^9$$ hertz |
| GMAO	| Global Modeling and Assimilation Office |
| GMT	| Greenwich Mean Time |
| GPP	| Gross Primary Production |
| GSE	| Ground Support Equipment |
| GSFC	| Goddard Space Flight Center |
| HDF	| Hierarchical Data Format |
| HK	| Housekeeping (telemetry) |
| HRSL	| Hydrology and Remote Sensing Laboratory |
| Hz	| Hertz |
| HSD	| Health and Status Data |
| I&T	| Integration and Test |
| ICD	| Interface Control Document |
| I/O	| Input/Output |
| IOC	| In-Orbit Checkout |
| IPA	| Inter-Project Agreement |
| ITAR	| International Traffic in Arms Regulation |
| JPL	| Jet Propulsion Laboratory |
| K	| Kelvin |
| KHz	| Kilohertz |
| Km	| Kilometer, 1000 meters |
| L0 – L4	| Level 0 through Level 4 |
| LAN	| Local Area Network |
| LEO	| Low Earth Orbit |
| LOE	| Level of Effort |
| LOM	| Life of Mission |
| LP	| Land Processes |
| LSTE	| Land Surface Temperature and Emissivity |
| m	| Meter |
| MB	| Megabytes, 106 bytes |
| Mbps	| Mega bits per second |
| MHz	| Megahertz |
| MMR	| Monthly Management Review |
| MOA	| Memorandum of Agreement |
| MOD16 | MODIS Global evapotranspiration algorithm |
| MODIS	| Moderate Resolution Imaging Spectroradiometer |
| MOS	| Mission Operations System |
| m/s	| Meters per second |
| ms	| Milliseconds |
| MS	| Mission System |
| NASA	| National Aeronautics and Space Administration  |
| NCEP	| National Centers for Environmental Protection |
| NCSA	| National Center for Supercomputing Applications |
| netCDF	| Network Common Data Format |
| NISN	| NASA Integrated Services Network |
| NOAA	| National Oceanic and Atmospheric Administration |
| OA	| Operations Agreement |
| ODL	| Object Description Language |
| OODT	| Object Oriented Data Technology |
| ORR	| Operational Readiness Review |
| ORT	| Operational Readiness Test |
| PDR	| Preliminary Design Review |
| percent	% | Parts per hundred |
| PLRA | Program Level Requirements Appendix |
| PR	| Problem Report |
| PSD	| Product Specifications Document |
| PT-JPL	| Priestly-Taylor-JPL |
| PT-JPL-SM | Priestly-Taylor-JPL-Soil Moisture |
| QA	| Quality Assurance |
| rad	| radians |
| RDD	| Release Description Document |
| RFA	| Request For Action |
| SBG	| ECOsystem Spaceborne Thermal Radiometer on Space Station |
| S/C	| Spacecraft |
| SCP	| Secure Copy |
| SDP	| Software Development Plan |
| SDS	| Science Data System |
| sec, s	| Seconds |
| SITP	| System Integration and Test Plan |
| SMP	| Software Management Plan |
| SOM	| Software Operators Manual |
| STIC | Surface Temperature Initiated Closure model | 
| TAI	| International Atomic Clock |
| Tb	| Brightness Temperature |
| TBD	| To Be Determined |
| TBS	| To Be Specified |
| TOA	| Top of Atmosphere |
| TPS	| Third Party Software |
| USDA	| United State Department of Agriculture |
| USGS	| United States Geological Society |
| UTC	| Coordinated Universal Time |
| V&V	| Verification and Validation |
| XML	| Extensible Markup Language |

*Table 4. Abbreviations and acronyms used in SBG-TIR documentation and products*
