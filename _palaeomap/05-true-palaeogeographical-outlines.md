---
last_modified_at: 2021-03-15
title: "True Palaeogeographical Outlines"
---

### Reconstructing Geography with Occurrences

Among the projects included with the GPlates download is the set of palaeogeographical reconstructions by Cao _et al._ (2017) for the last 402 my. These are based, and so should align nicely, with the Golonka (2007) model that is used for data in the PBDB, which we'll get to below. These data are in `GPlatesSampleData/FeatureCollections/Palaeogeography/Global/`.

Cao _et al._ (2017) used a combination of previous palaeogeographical reconstructions, and updated the coastlines, mountains, and extent of land and ice caps using occurrence data from the PBDB. In a few locations, what had been reconstructed as land produced marine fossils, when accounting for the movement of plates, and vice versa.

These reconstructions include four layers that have the separated palaeoenvironmental regions:

* Shallow marine: shelf seas.
* Land: terrestrial habitats outlined by coastlines.
* Mountain: areas of mountain building and uplift.
* Ice cap: polar regions that have substantial evidence for long term presence of ice.

These are given the abbreviations 'sm', 'l', 'm' and 'i'. It's worth noting that the way these are built means that the layers have to be plotted in the order above – shallow marine first, ice cap last. There are various areas that overlap (Australia especially) so the order of overlay becomes important in making the correct boundaries visible.

