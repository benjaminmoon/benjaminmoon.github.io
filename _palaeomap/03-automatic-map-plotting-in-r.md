---
title: Existing Packages
last_modified_at: 2021-03-15
toc: false
---

A couple of packages already exist for using GPlates reconstructions to create palaeogeographical maps in R:

*   NonaR/paleoMap (<https://github.com/NonaR/paleoMap>)
    -   NB Has not been updated in several years.
*   LunaSare/gplatesr (<https://github.com/LunaSare/gplatesr>)
    -   More recently updated, and similar to above.

Both of these download from GWS. I've borrowed some of their functions for this lab group.

If you're unfamiliar with RStudio or RMarkdown (`.Rmd` files), then the code sections are placed in 'chunks' like the one below. You can run individual lines by Cmd+Enter or Ctrl+Enter, or run the whole chunk using the little green arrow to the top right of each chunk. Try with this chunk below if you want.

```r
library(broom)
library(dplyr)
library(forcats)
library(ggplot2)
library(ggthemes)
library(mapproj)
library(purrr)
library(readr)
library(rgdal)
library(stringr)
library(tibble)
# library(tidyverse) # a quick way to load packages, but slow for Binder.

list.files("functions/", pattern = "\\.R", full.names = TRUE) %>%
    walk(source)
```


**NB not all of the chunks should be run on Binder as some of the code will take a long time or fail to execute.**
