# metScanR 
## Mitigating the "80/20 Data Science Dilemma" in atmospheric / environmental sciences
[![](http://cranlogs.r-pkg.org/badges/metScanR)](https://cran.rstudio.com/web/packages/metScanR/index.html) 
[![CRAN_Status_Badge](http://www.r-pkg.org/badges/version/metScanR)](http://cran.r-project.org/package=metScanR)


## Summary 

Every day thousands of meteorological and environmental data are collected throughout the world. The stations that collect these data are part of many large-, medium-, and small-scale networks throughout the globe.  Some stations are part of multiple networks, have well documented metadata, and their data can be accessed through a handfull of public databases.  Other stations however, have poorly documented metadata and their data are harder to locate and access. The latter makes it difficult and time consuming to answer specific scientific questions (Fig 1). 

![](AMS2018/findingMetadata.png "Figure 1: Breakdown of common metadata availability ")
<sup>**FIGURE 1** Some metadata are easy to find in many repositories (green box), while other types of metadata are hard to track down (orange box).  This can make searching for environmental monitoring stations of interest painstakingly slow especially if searching for specific criteria.</sup>


Inconsistent metadata, documentation, data formats, station names, and station identifiers pose major roadblocks to finding, cleaning, and organizing (collectively known as 'wrangling') meteorological and environmental data among different networks.  This concept is not new.  In fact, many studies have estimated that data 'wrangling' accounts for roughly 80% of data science, allowing data scientists only 20% of their time to actually analyze the data ([Forbes (2016)](https://www.forbes.com/sites/gilpress/2016/03/23/data-preparation-most-time-consuming-least-enjoyable-data-science-task-survey-says/); [IBM (2017)](https://www.ibm.com/blogs/bluemix/2017/08/ibm-data-catalog-data-scientists-productivity/)).  

### Mitigating the 80/20 Dilemma:

We are pleased to introduce *metScanR*, an R package that enables users to quickly locate freely available meteorological and environmental data across multiple networks, worldwide. The *metScanR* package utilizes a continuously growing database (see the [*metScanR database (DB)*](#refDatabase) section below), that currently contains metadata for **157,676** stations from **219** countries/territories, worldwide (Fig 2). 

![metScanR](AMS2018/allSites20190130.png "Figure 2: All Stations within the metScanR database.")
<sup>**FIGURE 2**: Plot of all stations within the metScanR database.  Each station is represented by a dot. Station locations are the only items plotted here - no geographical or political boundaries are mapped.  This reveals interesting patterns about environmental station placement, e.g., northern Canada, India, central Australia, etc.</sup>

*metScanR* allows a user to search for stations and metadata in a variety of ways via the R functions within the package.  Below is a list of *metScanR* functions and their use.

**Filtering Functions**

These functions allow the end-user to filter environmental stations by:

* Specific country - `getCountry()` 
* Active date(s) - `getDates()`  
* Station Elevation - `getElevation()`
* Identifier type - `getId()`
* Nearby a Point of Interest (POI) - `getNearby()` 
* Network - `getNetwork()`
* Specific Station - `getStation()`  
* Meteorological / Environmental variables measurered - `getVars()`
* Specific US state or territory - `getTerritory()` 
* Hybrid search (all of the above) - `siteFinder()`

Use of any of the above functions will return an R `list()` object detailing of all meteorological/environmental stations that meet the search criteria.  Metadata of each station are structured in a standardized format (below) and are returned to the end-user when using the above search functions:

* *$namez* [chr] - Name of the station 
* *$identifiers* [data.frame] - Station identifiers, including idType (i.e., the governing body that supplies the station ID such as the World Meteorological Organization) and the associated *id* 
* *platform* [chr] - The primary network or platform that the station belongs to
* *elements* [data.frame] - The meteorological/environmental variables measured at the station, includes the start and end dates of active sampling for each variable (if available)
* *location* [data.frame] - Geolocation information, inlcuding lat/lon, elevation, country, etc., for the site 

![](AMS2018/listOutput.png "Figure 3: Example list output from metScanR search.")

<sup>**FIGURE 3**: Exemplar output of metScanR's `getStation()` search function for the NRCS SCAN site 2172.  Output from all of metScanR's search functions are structured in this manner.</sup>


**Mapping Function**

The *metScanR* package also comes with a function (`mapSiteFinder()`) that displays stations from a user-defined search (Figure 4).  When run in R the map is interactive and users can click on stations for more information and/or toggle different areas by zooming in/out etc. This is an aesthetic, ancillary feature to compliment the many search functions available within the package.

![](AMS2018/fig4_2019_02_01.png "Figure 4: metScanR's mapSiteFinder() function.")

<sup>**FIGURE 4:** Screenshot example of metScanR's `mapSiteFinder()` and `getNearby()` functions.  A total of 803 stations are within 50 km of NEON's NIWO station.  Total run time = 9.2 seconds.</sup> 


### The metScanR Database: <a id="refDatabase"></a>

The current version of the *metScanR DB* is *v2.4.0* and currently contains metadata from **157,676 stations**, worldwide. The DB is updated frequently and hosted externally of the *metScanR* package.  Upon loading the *metScanR* package via `library(metScanR)`, the DB is accessed via internet connection and installed locally to the user's computer via metScanR's `updateDatabase()` function.  The provenance of the DB is detailed below:

* **v1.0.0**  *2017-01-18* Initial release.  Database was in dataframe format and hosted with the R package.  Database comprised ~13,000 sites from the US and parts of Canada.
* **v2.0.0** *2017-05-18* Major release.  Database converted to list format with content (below). Database contains 106,933 stations from around the world and is hosted externally and independent from the *metScanR* package.  The new list format now includes:

- `$namez` - station name [character]
- `$identifiers` - station id(s) and idType(s) [data.frame]
- `$platform` - Primary platform (network) that the station belongs to [character] 
- `$elements`- element type (e.g., precipitation) and active sampling dates for element [data.frame]
- `$location` - geolocation metadata, e.g., lat/lon, elevation, etc. [data.frame]

* **v2.1.0** *2017-07-03* Minor release. NADP and Ameriflux networks added to DB. Database contains 107,624 stations, worldwide.

* **v2.2.0** *2017-11-05* Minor release. Identified 498 stations as duplicate entries, removed from DB.  DB now contains 107,126 worldwide stations.  Attributes (above comment) added to DB.  Will use these as checks to ensure user has most up-to-date version installed

* **v2.3.0** *2018-08-27* Minor release.  Over 50,000 mesonet stations added to DB.  Database now contains 157,655 stations, worldwide.  
* **v2.4.0** *2019-01-29* Minor release.  Mapped mesonet variables to metScanR's terms and traceability data frames.  Added units to many variables.  Added remaining NEON sites, data products and sub terms to database.

### Novelty of *metScanR*:
Because meteorological/environmental networks are managed by different governing bodies, an abundance of discrepancies exist within station metadata.  A single station may be part of many networks, can have many associated identifiers, and may have similar data product-types (i.e., variables monitored) stored among many repositories.  Additionally, the same data product may be available at different temporal resolutions among the repositories.  

A user may find themselves searching for a station of interest and depending on the station identifier that they use, may be routed to a repository that contains only a fraction of the available station inforation (see Figure 5). This results in a  "discrepancy gap" of data avaiability among the thousands of meteorological/environmental stations, worldwide.  The *metScanR* package bridges this  'discrepancy gap.' by organizing all metadata into a standardized database, i.e., the *metScanR database*.  


![](AMS2018/idTraceability.png "Figure 5: station ID traceability.")
<sup>**FIGURE 5:** An example of metadata discrepancies for a single station: 'Downtown Charleston' in Charleston, SC, USA.  This station has many associated identifiers which route to repositories managed by different governing bodies with varying metadata standards.</sup> 

This gap extends to the manner in which measured variables (e.g., air temperature, soil moisture, snow depth, etc.) are abbreviated and reported among networks. Some networks use full term names to document their data products, while others use abbreviations or discrete alpha-numeric identifiers.  For instance, "air temperature" is denoted as, "air temperature", "TEMP", "TAVG, "temp_avg", "NEON.DP1.00002." etc. among networks; *many terms are used to denote the same atmospheric phenomenon*.  To alleviate this discrepancy and make elemets traceable to one another, elements within the *metScanR database* are structured using a hierarchical n-gram framework that links common terms to network-specific element codes.  This accounts for nomenclature discrepancies and allows users to search for a variety of like-elements in one search, a novel approach for structuing and storing meteorological/environmental metadata (Figure 5).  

![](AMS2018/n-grams.png "Figure 6: Element traceability via n-grams.")
<sup>**FIGURE 6:** Linking the many network-specific variable abbreviations to common terms.</sup>  

The standardized databases, n-gram traceability, search features, mapping feature, and ease of use make metScanR a novel tool.  Collectively, these features allow users to greatly reduce the time-sink of *finding* atmospheric and environmental data.  

**A quick Example**

A user wishes to search for environmental / meteorological stations nearby a known point of interest (POI).  Using metScanR's `getNearby()` function this can be done in a matter of seconds.  As an example, let's use the search from Figure 4; it took 9.2 seconds to search for and map the 803 stations nearby NEON's NIWO station.  That's a lot of stations among many networks.  But let's say that the researcher wants to find data from nearby sites that have similar elevations to NEON's NIWO site (3513 m ASL) and measure snow depth. No problem.  Using the `siteFinder()` function, the user can filter the dataset more specifically to their needs (Figure 7).

![](AMS2018/fig7_2019_02_01.png "Figure 7: Filtered user search.")
<sup>**FIGURE 7:** Screenshot example of metScanR's `mapSiteFinder()` and `siteFinder()` functions.  Around a dozen stations are i) within 50 km of NEON's NIWO station, ii) at an elevation between 3200 & 3800 m ASL, that iii) measure snow depth.</sup>   

From start to finish this entire filtering process took less than 2-minutes.  The user can then utilize other network-specific R packages, such as [RNRCS](https://github.com/rhlee12/RNRCS), to quickly download the data.    


### Getting Started:

Install official releases from CRAN with 

```
#install metScanR:
install.packages("metScanR")
#load the package:
library(metScanR)
#download and save the most up-to-date database:
updateDatabase()

```
If you encounter a bug, please provide a reproducible example on this package's [github issues](https://github.com/jaroberti/metScanR/issues) page. 

### Tutorial:

A tutorial is provided at: https://jaroberti.github.io/metScanR/tutorials/intro.html. 

**Update (2018-09-21)** : Please note, the tutorial is a bit out of date.  We're hoping to update the tutorial in the near future. 

### CRAN PDF: 
https://cran.r-project.org/web/packages/metScanR/metScanR.pdf


### Future Directions:

We're hoping to:

1. Enable metScanR with functionality for directly downloading meteorological and environmental data via existing APIs.
2. Build a web-based platform for users not familiar with the R programming language.

### Citation:
```
citation("metScanR")
```

  Josh Roberti, Cody Flagg, Lee Stanish and Robert Lee (2018). metScanR: Find, Map, and Gather Environmental Data
  and Metadata. R package version 1.2.0. https://CRAN.R-project.org/package=metScanR

### Meet the team:
[Josh Roberti](https://www.linkedin.com/in/josh-roberti-09297946/)

[Cody Flagg](https://www.linkedin.com/in/cody-flagg-12354765/)

[Lee Stanish](https://www.linkedin.com/in/lee-stanish-3698971a/)

[Robert Lee](https://www.linkedin.com/in/robert-lee-a51163bb/)


### Presentations:
[Click here](https://ams.confex.com/ams/98Annual/videogateway.cgi/id/44605?recordingid=44605&uniqueid=Paper336096&entry_password=164273) to check out our presentation from the American Meteorological Society's (AMS) Annual Meeting (2018-01-10) in Austin, TX.


### Contact:
If you have any questions, comments, concerns, or if you'd like to see a specific network added to the metScanR database, please email Josh (jaroberti87@gmail.com).

