---
last_modified_at: 2021-04-14
pub-type: "talk"
pub-authors: Moon, B.C., Stubbs, T.L.
end-date: 2021-03-31
institution: "University of Bristol"
title: "Open Research Prize Symposium"
classes: wide
---

I was selected to present at the University of Bristol [*Open Research Prize 2020* Symposium](http://bristol.ac.uk/staff/researchers/open-research/), after submitting a case study for how Tom Stubbs and I approached making our research more open in publishing our [2020 paper on ichthyosaur evolutionary rates and disparity](/cv/2020-02-13-pub-ichthyosaur-macroevo/). You can see and go through the slides below.

{% include slideshow slide_src="open-research-prize-2021" %}

Here, I wanted to present both our research and how we tried to make it as accessible and easily reusable and reproducible as possible; first presenting the questions around ichthyosaur evolution that we wanted to answer. I spent a few slides going through the work flow that we developed:

1. The data sources: traits, time ranges and family tree (slides 4–6).
2. Analyses completed in R and specialist software (slide 7).
3. The outputs:
    1. Time series and box plots of variation is shape from PCA analysis (slide 8).
    2. Time-scaled trees (slide 9).
    3. Evolutionary rates through time and across the tree (slides 10, 11).

I've summarised our results before, which you [can read about here](/cv/2020-02-13-pub-ichthyosaur-macroevo/).

Our work flow was complex because of the different elements going in and out, so to make it amenable for others to reproduce we used Git and GitHub to [develop and publish the code and figures](https://github.com/benjaminmoon/ichthyosaur-macroevolution) behind it, and [Zenodo to create a referable DOI](https://zenodo.org/badge/latestdoi/211271780). The code is commented, with only a few places to add data and run it. And we published in the open access journal [*Communications Biology*](https://doi.org/10.1038/s42003-020-0779-6).

This was also my first publication that I had properly thought about sharing neat and standardised code. We had scripts here, but included extensive commenting to explain what we used and why, and modularised the process with functions that can be easily reused. Yet there are certainly things that I would change and do differently. Using [RMarkdown](https://rmarkdown.rstudio.com) in a proper notebook format (or even [Jupyter](https://jupyter.org)), managing packages with [renv](https://rstudio.github.io/renv/) and using other live coding tools like [Binder](https://mybinder.org). I've started with many of these now in other projects, some teaching that I do—you can see some in my [GitHub repos](https://github.com/benjaminmoon).

Hopefully you will find something useful in this to spark your inspiration. I'm more than happy to answer questions, and will likely write more on this in the future. Feel free to <i class="far fa-fw fa-envelope" aria-hidden="true"></i> [email me](mailto:ben@bcmoon.uk).
