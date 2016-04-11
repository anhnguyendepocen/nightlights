# nightlights
An `R` package to extract NOAA night lights data for regions within shapefiles (a `SpatialPolygonsDataFrame`). The night lights data can be downloaded from [here](http://ngdc.noaa.gov/eog/data/web_data/v4composites/). A download script is also provided (see below).

![Data](/img.png?raw=true "Night Lights Data")

## Installation
        if (!require(devtools)) install.packages("devtools")
        devtools::install_github("walshc/nightlights")

## Usage

        # Get an example shapefile to work with:
        download.file("ftp://ftp2.census.gov/geo/tiger/TIGER2015/COUSUB/tl_2015_25_cousub.zip",
                      destfile = "tl_2015_25_cousub.zip")
        unzip("tl_2015_25_cousub.zip")
        shp <- rgdal::readOGR(".", "tl_2015_25_cousub")

        # Download and extract some night lights data (one year here as an example):
        download.file("http://ngdc.noaa.gov/eog/data/web_data/v4composites/F182013.v4.tar",
                      destfile = "F182013.v4.tar")
        untar("F182013.v4.tar")
        R.utils::gunzip("F182013.v4c_web.stable_lights.avg_vis.tif.gz")

        # Directory where night lights data are stored (current directory here):
        nl.dir <- "."

        # By default, the function gets the sum of night lights within the regions:
        nl.sums <- extractNightLights(nl.dir, shp)

        # You can specificy other statistics to get, e.g. the mean & standard deviation:
        nl.mean.sd <- extractNightLights(nl.dir, shp, stats = c("mean", "sd"))

## Example output
If the night lights directory contains the data for years 1999 and 2000 and `stats = "sum"`:
               GEOID       NAME night.lights.1999.sum night.lights.2000.sum
        0 2502328285    Hanover                3613.0                3587.0
        1 2502338855 Marshfield                5726.5                5253.5
        2 2502350145    Norwell                4494.0                4268.5
        3 2502372985    Wareham                5625.5                5338.0
        4 2500300975     Alford                 324.5                 409.0
        5 2500334970      Lenox                2339.5                2661.0

## Script to download and extract all the data
To save time downloading and extracting all of the NOAA data manually I also provide a script (`download-and-extract-lights-data.sh`) which should work in the terminal on OS X and Linux. The script will ask which directory you want to save the night lights data to. Keep in mind the full data set is very large so you will need about 50GB of space to do this. You can choose to save it to an external drive if you wish. Paste the following commands in the terminal to get it started:

        wget https://raw.githubusercontent.com/walshc/nightlights/master/download-and-extract-lights-data.sh
        chmod +x download-and-extract-lights-data.sh
        ./download-and-extract-lights-data.sh
