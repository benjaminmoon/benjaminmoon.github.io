---
last_modified_at: 2021-03-15
title: Getting Fossil Occurrence Data
toc: false
---

Now that you've spent lots of time making a pretty map, let's plot some fossils on top of it.

Of course, being me and having used Jurassic data, we're going to plot some ichthyosaurs. In this instance, the occurrence data comes from the PBDB, downloading ichthyosaur occurrences with these filters:

* Callovian–Tithonian (166-145 Ma)
* all levels of taxonomy – no filtering.

The PBDB has its own web API (<https://paleobiodb.org/data1.2>) from which you can get occurrences, collections, taxonomy and other things. In the code chunk below I've crafted the URL to download the data, but this is already in the repo so doesn't need to be run.

**Do not run the following chunk on Binder, the data is already in the repo.**

```r
#### DO NOT RUN THIS CHUNK ####
pbdb_url <-
  "https://paleobiodb.org/data1.2/occs/list.csv?base_name=Ichthyosauromorpha&interval=Callovian,Oxfordian,Kimmeridgian,Tithonian&show=paleoloc"

occ_ichthyosaurs <-
  read_csv(pbdb_url)
```

The part of the URL `show=paleoloc` is the important bit as the PBDB has already computed the palaeocoordinates of most occurrences using their modern location and the ages of the occurrences. Conveniently using the same default GPlates model as the palaeogeography.
