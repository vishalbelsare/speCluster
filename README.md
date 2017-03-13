
<!-- README.md is generated from README.Rmd. Please edit that file -->
speCluster
==========

[![Project Status: WIP - Initial development is in progress, but there has not yet been a stable, usable release suitable for the public.](http://www.repostatus.org/badges/latest/wip.svg)](http://www.repostatus.org/#wip)

This package wraps the code at:

<https://github.com/cont-limno/SpectralClustering4Regions>

Installation
------------

You can install speCluster from github with:

``` r
# install.packages("devtools")
devtools::install_github("jsta/speCluster")
```

Usage
-----

### Load package

``` r
library(speCluster)
#> Loading required package: maps
```

### Read data

``` r
dataTerr     <- read.csv("data-raw/terrData.csv", header = T)
dataFW       <- read.csv("data-raw/freshData.csv", header = T)
i <- which(colnames(dataFW) == "hu12_states")
dataTerrFW   <- merge(dataTerr, dataFW[-i], by.x = "zoneid", by.y = "zoneid")
islands      <- read.csv("data-raw/islandIdx.csv", header = T)
latLong18876 <- read.csv("data-raw/latLong18876.csv", header = T)
NB18876      <- read.csv("data-raw/NB_18876.csv", header = T)
```

### Prep Input

``` r
# generate the input when we restrict the
# data to dataTerrFW's of state Missouri, 
# and no islands inculded.
input <- generateData(type = dataTerrFW, islands = islands, 
                      latLong = latLong18876, NB18876, islandsIn = F, 
                      states = c("MO"), conFactor = 1)
```

### Generate Clusters

``` r
results <- speCluster(data = input$data, conMatrix = input$conMatrix, 
                      cluster.number = 10)
#> Warning in produceU(similarity = S, ncol = cluster.number): Type 2
#> algorithm might need more than 4.0 G Ram
summary(results)
#>          Length Class  Mode   
#> clusters 1748   -none- numeric
#> SS          2   -none- list
results$SS
#> $SSW
#> [1] 47925.58
#> 
#> $SSB
#> [1] 17244.8
head(results$clusters)
#> 1 2 3 4 5 6 
#> 1 1 1 1 1 1
mapping(lat = input$latLong[,1], long = input$latLong[,2],
         clusters = results$clusters)
```

![](images/unnamed-chunk-4-1.png)

Citation
--------

*Creating Multithemed Ecological Regions for Macroscale Ecology* ([Cheruvelil et al. 2017](https://dx.doi.org/10.1002/ece3.2884))
