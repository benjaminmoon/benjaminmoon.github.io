---
title: "Palaeogeographic Maps in R"
category: palaeontology
tags:
    - palaeogeography
    - R
    - coding
    - ichthyosaurs
toc: true
---

Just before Christmas I was looking at data from the Palaeobiology Database
(PBDB) and wanted to draw a map of occurrences. I'd made maps of Jurassic
ichthyosaurs before that appeared in Moon & Kirton (2018, Fig. 46). I made these
by hand, tracing maps produced by Blakey (2008, 2014) and then overlaying
palaeo-positions of various deposits. As I remember the tracing itself was not
too time consuming, but wasn't the most interesting thing I've ever done.

With my recent work, I wanted to create maps with many more occurrence points,
showing a series of time slices as the position of the continents change. This
would definitely benefit from a more automated method rather than me getting
annoyed with Affinity Designer while tracing several maps.

Fortunately there are ways to do this in R through some code and packages that
are readily available. Here's a little intro to how I've made a map with data
from the PBDB, including:

* plotting continent and coastal outlines
* adding points to show occurrence locations
* separating different groups into subplots.

Let's dive in.

## Data gathering ##

The few things that we need are:

* Occurrence data from the Palaeobiology Database
* Palaeogeographic outlines of the continents at a desired time.

### Occurrence data ###

The PBDB has a [Web API](https://paleobiodb.org/data1.2/),
which I find the easiest to access and download data from. There is also the
package [ropensci/paleobiodb](https://github.com/ropensci/paleobioDB), but I
haven't used it  myself.

To download occurrences of ichthyosaurs from the Toarcian
(182.7–174.1 Ma), I used the following code in R.

```r
library(tidyverse)

pbdb_url <-
  "https://paleobiodb.org/data1.2/occs/list.csv?base_name=Ichthyosauromorpha&interval=Toarcian&show=paleoloc"

occ_toarcian_ichthyosaurs <-
  read_csv(pbdb_url)
```

### Palaeogeographic continent outlines ###

In looking for palaeogeographic outline data I found a few sources. Initially I
came across the R package [NonaR/paleoMap](https://github.com/NonaR/paleoMap)
that includes palaeogeographic data and functions for organising them within the
package, but was last updated in 2015. After, I found
[LunaSare/gplatesr](https://github.com/LunaSare/gplatesr). gplatesr downloads
data from the [GPlates Web Service](https://gws.gplates.org), and can
reconstruct palaeocoordinates – something I look forward to reading up on and
using more.

Reading this data into R is straightforward: using code from the package
gplatesr as a cue, these following lines download outline data for the
continental configuration at 182 Ma (Toarcian) – reconstructions of both the
continental plates (static polygons) and the coastlines. The package rgdal
creates a structure that R can use and understand.

```r
library(rgdal)

coastline_gws_url <-
  "http://gws.gplates.org/reconstruct/coastlines/?time=182&model=GOLONKA"
polygons_gws_url <-
  "http://gws.gplates.org/reconstruct/static_polygons/?time=182&model=GOLONKA"

toarcian_coastlines <-
  rgdal::readOGR(coastline_gws_url)
toarcian_polygons <-
  rgdal::readOGR(polygons_gws_url)
```

[GPlates](https://www.gplates.org) itself is a standalone application built to
reconstruct palaeogeography using models of continent trajectories through time.
By default GPlates uses reconstructions from Matthews _et al._ (2016), but can
interpolate between known continental configurations, allowing arbitary time
points to be reconstructed – a very useful feature – that can then be exported
to GMT files, for example, and imported into R as above. This makes GPlates more
useful for producing reconstructions of several times, or producing animations,
whereas the web service may be more time-consuming or restrictive for this.


## Plotting ##

### Plotting the map ###

To plot the map data, I've found that using ggplot is the most convenient as
this provides the `geom_map` function exactly for this purpose. The following
code is modified from the gplatesr vignette to plot the base map, onto which we
can place occurrence points.

```r
library(ggplot2)
library(ggthemes)

toarcian_coastlines <- fortify(toarcian_coastlines)
toarcian_polygons <- fortify(toarcian_polygons)

toarcian_map <-
  ggplot() +
    geom_map(
      data = fortify(toarcian_polygons), map = fortify(toarcian_polygons),
      aes(x = long, y = lat, map_id = id),
      size = 0.15, fill = "#d8d8d6"
    ) +
    geom_map(
      data = fortify(toarcian_coastlines), map = fortify(toarcian_coastlines),
      aes(x = long, y = lat, map_id = id),
      size = 0.15, fill = "darkkhaki"
    ) +
    geom_rect(
      data = data.frame(xmin = -180, xmax = 180, ymin = -90, ymax = 90),
      aes(xmin = xmin, xmax = xmax, ymin = ymin, ymax = ymax),
      color = 1, fill = NA, size = 0.3
    ) +
    coord_map("mollweide") +
    theme_map()

toarcian_map +
  labs(
    title = "Palaeogeographic map of continents (khaki) and coastlines (grey)\nin the early Toarcian (182 Ma)"
  )
```

{% include figure
    image_path="/assets/images/toarcian_palaeogeographic_map.svg"
    alt="Palaeogeographical map of the Toarcian"
    caption="Palaeogeographical map of coastlines (grey) and modern continental
    plates (khaki) in the early Toarcian (182 Ma)."
%}

### Plotting occurrence locations ###

Data downloaded from the PBDB includes pre-calculated palaeocoordinates when
using `show=paleoloc` in the URL. Most often this is calculated using GPlates,
and the palaeomodel column shows the origin of this. Thus we can stick the PBDB
data straight onto our palaeogeographical map. Neat.

```r
toarcian_map +
  geom_point(
    data = occ_toarcian_ichthyosaurs,
    aes(x = paleolng, y = paleolat)
  ) +
  labs(
    title = "Palaeogeographic map of ichthyosaur occurrences\nin the Toarcian (182 Ma)"
  )
```

{% include figure
    image_path="/assets/images/toarcian_ichthyosaurocc_map.svg"
    alt="Ichthyosaur occurrences in the Toarcian"
    caption="Palaeogeographical map of ichthyosaur occurrences in the Toarcian
    (182 Ma)."
%}

And separating out different groups is also easy to do using facets, in
this case separating out the different identification ranks for the
occurrences.

```r
toarcian_map +
  geom_point(
    data = occ_toarcian_ichthyosaurs,
    aes(x = paleolng, y = paleolat)
  ) +
  facet_wrap(vars(identified_rank)) +
  theme(legend.position = "none") +
  labs(
    title = "Palaeogeographic map of ichthyosaur occurrences\nin the Toarcian (182 Ma) separated by identified rank"
  )
```



{% include figure
    image_path="/assets/images/toarcian_ichthyosaurocc_id_map.svg"
    alt="Toarcian ichthyosaur occurrences separated by identified rank"
    caption="Palaeogeographical map of ichthyosaur occurrences in the
    Toarcian (182 Ma) separated by rank."
%}


## Conclusion ##

Hopefully you'll find these code snippets useful to make a map and add
occurrence palaeolocations to this. I was pleasantly surprised by how readily
available the data are and how easy it is to implement in R. Next I'm looking to
add in maps for multiple times and occurrences of different taxa for a project
that – perhaps surprisingly – doesn't include ichthyosaurs.

I hope that you have a happy beginning to 2021.

## References ##

Blakey, R. 2008. Gondwana paleogeography from assembly to breakup—a 500 m.y.
odyssey. <i>Special Paper 441: Resolving the Late Paleozoic Ice Age in Time and
Space</i> 441: 1–28. [doi:10.1130/2008.2441(01)](https://doi.org/10.1130/2008.2441(01))

Blakey, R. 2014. Library of paleogeography. <https://jan.ucc.nau.edu/~rcb7/>

Moon, B.C. and Kirton, A.M. 2018. Ichthyosaurs of the British Middle and Upper
Jurassic. Part 2, <i>Brachypterygius, Nannopterygius, Macropterygius,</i> and
<i>Taxa Invalida</i>. <i>Monograph of the Palaeontographical Society</i> 172
(650): 85–176.
[doi:10.1080/02693445.2018.1468139](https://doi.org/10.1080/02693445.2018.1468139)
