---
pub-type: "paper"
pub-authors: "Flannery Sutherland, J.T., Moon, B.C., Stubbs, T.L. & Benton, M.J."
title: "Does exceptional preservation distort our view of disparity in the
fossil record?"
start-date: 2019-02-27
institution: "Proceedings of the Royal Society B"
doi: 10.1098/rspb.2019.0091
volume: 286
pages: 20190091
---
What started out as a summer project led by Tom Stubbs started growing into an
exploration of methods for calculating diversity. This was done by Joe Flannery
Sutherland from Summer 2017.

Initially, the plan was to look at the effect of Lagerstätten on disparity
calculations in ichthyosaurs. We use discrete-character disparity calculations
to measure the disparity of ichthyosaurs through time and then remove taxa that
are from sites of exceptional preservation – predominately those with
particularly complete specimens, but also those that preserve lots of material.
The argument is that taxa from these exceptional sites may selectively increase
the disparity, both by presenting more material and better-preserved material.

{% include figure
    image_path="https://royalsocietypublishing.org/cms/asset/cc88911a-548f-448c-8004-000cbfffe3d8/rspb20190091f01.jpg"
    alt_text="Ichthyosaur disparity with and without Lagerstätten"
    caption="Ichthyosaur disparity with (black) and without (red) taxa from
    Lagerstätten included. Lagerstätten do increase the observed disparity,
    however, the patterns for both series are broadly the same. From Flannery
    Sutherland et al. (2019)."
%}

The positive news is that the trends we found in ichthyosaur evolution were
consistent whether all taxa were included or not, even though removing taxa from
Lagerstätten did reduce the disparity in some bins.

One of the things that Joe noticed doing this is that the disparity and
morphospace region occupied decreased as taxa became less complete.

{% include figure
    image_path="https://royalsocietypublishing.org/cms/asset/97ec5370-42cd-4776-9ed2-47aeb727672b/rspb20190091f04.jpg"
    alt_text="Morphospace occupation decreases with decreasing completeness"
    caption="Morphospace occupation collapses towards the centroid with
    decreasing completeness of the input data. From Flannery Sutherland et al.
    (2019)"
%}

This was also explored by [Lehmann et al.
(2019)](https://doi.org/10.1111/pala.12430), who went into the underlying
methods and found similar trends with their analysis. This particularly reflects
the used of Generalised Euclidean Distances that fill missing values with
averages across a range of taxa, and so creates an artificial averaging effect
as taxa are less complete and more values are replaced.

## References

Lehmann, O.E.R., Ezcurra, M.D., Butler, R.J. and Lloyd, G.T. 2019. Biases with
the generalized Euclidean distance measure in disparity analyses with high
levels of missing data. _Palaeontology_ **62** (5): 837–849.
doi:[10.1111/pala.12430](https://doi.org/10.1111/pala.12430)
