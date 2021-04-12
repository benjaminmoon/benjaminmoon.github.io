---
title: Introduction
last_modified_at: 2021-04-12 
redirect_from:
  - /research/palaeogeographical-maps-in-r/
---

This guide takes you through two different work flows to create palaeogeographical maps and plot occurrences fossil taxa in R. All the code and data are in this GitHub repository: <https://github.com/benjaminmoon/palaeomap_example>

You can download and run this code directly on your machine, or follow along online at Binder where I've created an environment with all of the data you need. Click the link below to open this in your web browser.

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/benjaminmoon/palaeomap_example/HEAD?urlpath=rstudio)

The Binder link will open in your browser in the RStudio interface. Open the file `palaeomap_labgroup.Rmd` from the file browser to get started.

{% include figure
    image_path="/assets/images/palaeomap_workshop/rstudio_window.webp"
    alt="The RStudio interface"
    caption="The RStudio interface similar to that you should see in Binder. Open the file `palaeomap_labgroup.nb.html` to get started."
%}

### Palaeogeographical maps

Maps are essential when discussing occurrences of taxa, both in the present and the past. Chuck in the issues associated with continental drift, and it can be difficult to place the wider context of some fossil occurrences. 'These marine fossils are found at the top of a mountain, but where were the oceans 150 million years ago?' Here are a couple of recent examples discussing the spread of occurrences, in this case Jurassic marine reptiles.

{% include figure
    image_path="https://media.springernature.com/full/springer-static/image/art%3A10.1186%2Fs42501-019-0043-5/MediaObjects/42501_2019_43_Fig4_HTML.png"
    alt="Occurrence of Jurassic plesiosaurs in China"
    caption="As a first example, Gao _et al._ (2019) discuss the occurrences of Jurassic plesiosaurs in China."
%}


{% include figure
    image_path="https://static.cambridge.org/binary/version/id/urn:cambridge.org:id:binary:20201223091456984-0252:S0016756820001351:S0016756820001351_fig1.png"
    alt="Occurrences of Jurassic polar ichthyosaurs"
    caption="Here, Zverkov _et al._ (2020) show occurrences of ichthyosaurs in the Early Jurassic."
%}

Reconstructing global palaeogeography has been an ongoing task for many decades, and both local and global reconstructions and compendia have been made (e.g. Bradshaw _et al._ 1992). But the case of making maps to show many occurrences by hand can be difficult and time-consuming.

A few examples of groups that have made efforts to create palaeogeographical maps at various points through the Phanerozoic include Ron Blakey at *Deep Time Maps* (<https://deeptimemaps.com>) and Christopher Scotese leading the *Paleomap Project* (<http://scotese.com>).

{% include video id="315907106" provider="vimeo" %}

{% include figure
    image_path="http://scotese.com/images/152.jpg"
    alt="Palaeogeography of the Late Jurassic"
    caption="Palaeogeography of the Late Jurassic, as reconstructed at _Palaeomap Project._"
%}

While built from models of plate tectonics and continental drift, these are nonetheless individual time points in a continuous series and open to some interpretation.

### GPlates

GPlates (<https://www.gplates.org>) is free software that reconstructs the movement of tectonic plates through geological history. Using models of plate movement, the configuration can be reconstructed at arbitrary points in the past. Onto this, the palaeoenvironments of the formations at their current positions can be overlain, and so the positions of landmasses, mountains, coastlines, and marine settings can be plotted long back in time.

The download from GPlates provides lots of example data and projects to show the various plates, tectonics, plumes and other information that can be included. New data are being added as research is published. Try, for instance, the *DataBundleForNovices* project (`GplatesSampleData/DataBundleForNovices`) to see a combined model of modern continents and ocean spreading over the last 410 million years.

{% include figure
    image_path="/assets/images/palaeomap_workshop/gplates_window.webp"
    alt="GPlates main window"
    caption="The main GPlates window showing the DataBundleForNovices loaded."
%}

{% include figure
    image_path="/assets/images/palaeomap_workshop/gplates_layers.webp"
    alt="GPlates layers window"
    caption="Layers can be selected in this GPlates window to show and hide different features."
%}

There is also a GPlates Web Service (GWS; <http://gws.gplates.org>) that can provide some of the same data without the software. I'll look briefly at this first.

### Exercises 1

1.  Find an example of a palaeogeographical map for your favourite time period.
2.  Have a look in the folder `GplatesSampleData` to see what other projects are included.

### References

Bradshaw, M.J., Cope, J.C.W., Cripps, D.W., Donovan, D.T., Howarth, M.K., Rawson, P.F., West, I.M. and Wimbledon, W.A. 1992. Jurassic. <i>Geological Society, London, Memoirs</i> 13 (1): 107–129. [doi:10.1144/GSL.MEM.1992.013.01.12](https://doi.org/10.1144/GSL.MEM.1992.013.01.12)

Gao, T., Li, D.-Q., Li, L.-F. and Yang, J.-T. 2019. The first record of freshwater plesiosaurian from the Middle Jurassic of Gansu, NW China, with its implications to the local palaeobiogeography. <i>Journal of Palaeogeography</i>:8, 27. [doi:10.1186/s42501-019-0043-5](https://doi.org/10.1186/s42501-019-0043-5)

Zverkov, N.G., Grigoriev, D.V. and Danilov, I.G. 2020. Early Jurassic palaeopolar marine reptiles of Siberia. <i>Geological Magazine</i>: 1–18. [doi:10.1017/S0016756820001351](https://doi.org/10.1017/S0016756820001351)
 
