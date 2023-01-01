# Building GDAL
coworker tried to build/install/use gdal with ECW file type support.
it was difficult. you need the ECW drivers from Hexagon Spatial:
`https://download.hexagongeospatial.com/en/downloads/ecw/erdas-ecw-jp2-sdk-v5-5-update-2-windows`
2.5 methods that worked:

1. build vanilla GDAL with cmake
1.5 install gdal package with Nuget (likely works on other platforms and package managers)
  - there should be a file path of x64/Debug/gdal with the main binaries in the install location
  - under this GDAL folder create a plugins directory e.g. gdal/plugins
  - from Hexagon get their drivers (latest 5.5 at time of writing), you have to fill in their form
  - copy these DLLs into the plugins directory
  - GDAL should now be able to read ECW
  - if you want to write ECW files, you need to pay Hexagon for a license
  - quickly check on the command line with `gdalinfo --formats` and look for `ECW` in the output

2. build GDAL with ECW support
  - as above, get drivers from Hexagon
  in first cmake step:
  `cmake -DECW_INCLUDE_DIR="/path/to/dll" -DECW_LIBRARY="/path/to/other/DLL"`
  `other cmake steps as normal`
  - check on the command line with `gdalinfo --formats` and look for `ECW` in the output


