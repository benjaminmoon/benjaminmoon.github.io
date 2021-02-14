---
title: "Building Palaeogeographic Maps in R"
category: palaeontology
tags:
    - palaeogeography
    - R
    - coding
    - ichthyosaurs
toc: true
layout: long_post
---

At the end of last year, I was looking at data from the [Palaeobiology
Database](https://paleobiodb.org/#/) (PBDB) and wanted to draw a map of where
fossils were found. I made such maps for Jurassic ichthyosaurs before, which
appeared in Moon & Kirton (2018, Fig. 46). These I made by hand, tracing maps
from Blakey (2008, 2014) then overlaying palaeo-positions of various
ichthyosaur finds. As I remember, the tracing itself was not too time consuming,
but wasn't the most interesting thing I've ever done.

Now I wanted to create maps with many more points and show a series of
time slices as the position of the continents change. This would definitely
benefit from a more automated method rather than me getting annoyed doing it all
manually.

Fortunately there are ways to do this in R with code and packages that
are readily available. Here's a little intro to how I've made a map with data
from the PBDB, including:

* plotting continent and coastal outlines
* adding points to show occurrence locations
* separating different groups into subplots.

In this case I'll plot data of Toarcian (182.7–174.1 million years ago, Ma)
ichthyosaurs. Let's dive in.

### Data gathering ###

The few things that we need are:

* Occurrence data from the Palaeobiology Database
* Palaeogeographic outlines of the continents at a desired time.

Fossil occurrences
:   Finds of species at a single time and place. In the palaeobiology database
these are linked to _collections,_ which are groups of fossil finds from a
single effort, like a single fossil dig.

Palaeogeography
:   The ancient configuration of continents, coastlines, mountains and seas
changing with continental drift and sea level change.

Palaeocoordinate
:   The ancient position of modern fossil find locations. Modern locations have
a latitude and longitude value, while palaeocoordinates have a palaeolatitude
and palaeolongitude. These can be reconstructed using the relative movement of
continental plates from the present back through the past.

The PBDB has a [Web API](https://paleobiodb.org/data1.2/), which I find the
easiest way to download data. There is also the package
[ropensci/paleobiodb](https://github.com/ropensci/paleobioDB), but I haven't
used it myself. Or you can download a CSV file direct from the portal.

To download occurrences of ichthyosaurs from the Toarcian I used the following
code in R.

```r
library(tidyverse)

pbdb_url <-
  "https://paleobiodb.org/data1.2/occs/list.csv?base_name=Ichthyosauromorpha&interval=Toarcian&show=paleoloc"

occ_toarcian_ichthyosaurs <-
  readr::read_csv(pbdb_url)
```

In looking for outlines of ancient continents, I found a few sources. First, I
came across the R package [NonaR/paleoMap](https://github.com/NonaR/paleoMap),
which includes map data and functions for organising them within the package.
But this was last updated in 2015, so may be a little out of date now or may use
functions not in the more recent versions of R. (I admittedly did not take too
long to check.)

After, I found [LunaSare/gplatesr](https://github.com/LunaSare/gplatesr).
gplatesr downloads data from the [GPlates Web Service](https://gws.gplates.org),
and can reconstruct palaeocoordinates – something I look forward to reading up
on and using more.

Getting this data into R is straightforward: using code from the package
gplatesr as a cue, these following lines download outline data for the
continents at 182 Ma, in the early Toarcian. In this case, I wanted both the
positions of the continental plates (static polygons) and the coastlines. The
package _rgdal_ reads in the data and organises it into a structure that R can use
and understand.

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

[GPlates](https://www.gplates.org) is software built to reconstruct
palaeogeography using models of continent trajectories through time. By default
GPlates uses reconstructions from Matthews _et al._ (2016), but it can
interpolate between known continental configurations. This means that you can
get the positions of the continents at any time in the past and export this into
a format for R. This makes the GPlates software useful for producing
reconstructions of several times or producing animations. The web service may be
more time-consuming, having to download lots of data, or restrict you if you try
to grab too much too quickly.

Incidentally, 'GOLONKA' in the URLs above refers to the Golonka (2007) model  of
continental movement (Verard 2019).


### Plotting the map ###

To plot the map data, I've found that using ggplot is the most convenient as
this provides the `geom_map` function exactly for this purpose. I modified the
following code from the gplatesr vignette to plot the base map, ready to add
points later.

```r
library(broom)
library(ggplot2)
library(ggthemes)

toarcian_coastlines <-
  broom::tidy(toarcian_coastlines)
toarcian_polygons <-
  broom::tidy(toarcian_polygons)

toarcian_map <-
  ggplot() +
    geom_map(
      data = toarcian_polygons, map = toarcian_polygons,
      aes(x = long, y = lat, map_id = id),
      size = 0.15, fill = "#d8d8d6"
    ) +
    geom_map(
      data = toarcian_coastlines, map = toarcian_coastlines,
      aes(x = long, y = lat, map_id = id),
      size = 0.15, fill = NA, colour = "grey30"
    ) +
    geom_rect(
      data = data.frame(xmin = -180, xmax = 180, ymin = -90, ymax = 90),
      aes(xmin = xmin, xmax = xmax, ymin = ymin, ymax = ymax),
      color = 1, fill = NA, size = 0.3
    ) +
    coord_map("mollweide") +
    ggthemes::theme_map()

toarcian_map +
  labs(
    title = "Palaeogeographic map of continental plate (grey) arrangement\nin the
    Toarcian (182 Ma) with modern coastlines outlined in khaki."
  )
```

{% include figure
    image_path="/assets/images/toarcian_palaeogeographic_map.svg"
    alt="Palaeogeographical map of the Toarcian"
    caption="Palaeogeographic map of continental plate (grey) arrangement in the
    Toarcian (182 Ma) with modern coastlines outlined in khaki."
%}

We've managed to get a good map. Aside from a few colour changes, from personal
preference, this should cover most of what we want from a base layer. The early
Toarcian is at the earlier end of the separation of Pangaea.

### Plotting occurrence locations ###

The data downloaded from the PBDB include pre-calculated palaeocoordinates when
using `show=paleoloc` in the URL. Conveniently, this is often using GPlates, and
the _palaeomodel_ column shows the origin of this. We can just stick the PBDB data
straight onto our palaeogeographical map. Neat.

```r
toarcian_map +
  geom_point(
    data = occ_toarcian_ichthyosaurs,
    aes(x = paleolng, y = paleolat)
  ) +
  labs(
    title = "Palaeogeographic map of ichthyosaur occurrences\nin the Toarcian (182 Ma)."
  )
```

{% include figure
    image_path="/assets/images/toarcian_ichthyosaurocc_map.svg"
    alt="Ichthyosaur occurrences in the Toarcian"
    caption="Palaeogeographical map of ichthyosaur occurrences in the Toarcian
    (182 Ma)."
%}

Ah … well I was hoping for a little more spread! But it seems that just about
all Toarcian ichthyosaurs come from western Europe. This is not surprising –
most specimens are either from the Posidonia Shale Formation of Germany or the
Yorkshire Lias. And this region was a shallow sea at the time – well suited to
many different marine reptile groups. It's good to know there are more out there
though, covering France up to Norway apparently.

Separating out different groups is also easy to do using _facets._ In
this case I separated out the different identification ranks for the
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
    title = "Palaeogeographic map of ichthyosaur occurrences\nin the Toarcian (182 Ma), separated by identified rank."
  )
```

{% include figure
    image_path="/assets/images/toarcian_ichthyosaurocc_id_map.svg"
    alt="Toarcian ichthyosaur occurrences separated by identified rank"
    caption="Palaeogeographical map of ichthyosaur occurrences in the Toarcian (182 Ma) separated by identified rank."
%}


### Conclusion ###

Hopefully you'll find these code snippets useful to make a map and add
occurrence palaeolocations to this. I was pleasantly surprised by how readily
available the data are and how easy it is to plot in R. Next I'm looking to
add in maps for multiple times and occurrences of different taxa for a project
that, perhaps surprisingly, doesn't include ichthyosaurs.


### References ###

Blakey, R. 2008. Gondwana paleogeography from assembly to breakup—a 500 m.y.
odyssey. <i>Special Paper 441: Resolving the Late Paleozoic Ice Age in Time and
Space</i> 441: 1–28. [doi:10.1130/2008.2441(01)](https://doi.org/10.1130/2008.2441(01))

Blakey, R. 2014. Library of paleogeography. <https://jan.ucc.nau.edu/~rcb7/>

Golonka, J. 2007. Phanerozoic paleoenvironment and paleolithofacies maps:
Mesozoic. _Geologia_ 33: 211–264.

Moon, B.C. and Kirton, A.M. 2018. Ichthyosaurs of the British Middle and Upper
Jurassic. Part 2, <i>Brachypterygius, Nannopterygius, Macropterygius,</i> and
<i>Taxa Invalida</i>. <i>Monograph of the Palaeontographical Society</i> 172
(650): 85–176.
[doi:10.1080/02693445.2018.1468139](https://doi.org/10.1080/02693445.2018.1468139)

Vérard, C. 2019. Plate tectonic modelling: review and perspectives. _Geological
Magazine_ 156: 208–241. [doi:10.1017/S0016756817001030](https://doi.org/10.1017/S0016756817001030)


