---
last_modified_at: 2021-03-15
title: "Export Palaeogeography"
---

### Export from GPlates

The Cao _et al._ (2017) reconstructions aren't available from GWS, but can be exported from GPlates directly using the menu item _Reconstruction > Export…_ Like above I've already done this for an example data set, in this case using the Late Jurassic (Kimmeridgian, 155 Ma) as an example. To make your own, once in the export window:

1. Select the time of export (can  be set from the main window time).
    - Also an option to export a series at regular intervals.
2. The layers to export
    1. select _Add Export…_ under Export Data
    2. choose _Reconstructed Geometries_
    3. output as _OGR-GMT_
    4. choose single or multiple files
        + I prefer multiple: you get one file for each of sea/land/mountain/ice. This is the form that works with the code below.
    5. _OK_
3. Change _Target directory:_ to somewhere you can find.
3. _Begin Export_

{% include figure
    image_path="/assets/images/palaeomap_workshop/gplates_export1.webp"
    alt="Exporting from GPlates"
    caption="Export options from GPlates."
%}

{% include figure
    image_path="/assets/images/palaeomap_workshop/gplates_export2.webp"
    alt="Layer selection in GPlates"
    caption="Layer selection in GPlates export. Select Reconstructed geometries, OGR-GMT, and multiple layers."
%}

The example data are in `data/palaeogeography` as three `.gmt` files that describe the polygon outlines for each of the layers. Note that as there are no ice caps at this time, there are only three files/layers. Look at the Carboniferous of later Palaeogene to get some ice caps.

### Read in the map data

I find it useful to tie together the layers and colours from the beginning of importing – this ensures keeping the layers in the order for plotting. The data files are:

* `lm_402_2_reconstructed_155.00Ma.gmt`: landmass
* `m_402_2_reconstructed_155.00Ma.gmt`: mountain
* `sm_402_2_reconstructed_155.00Ma.gmt`: shallow marine

This short code snippet sets lists the layer names and assigns the colours for each layer on the map – they are similar to those shown in GPlates, but not quite the same. I've also used `#DAD3FF` for ice caps when needed. (Technically this isn't used for the colours, that's in the `palaeogeog_map_niceties` function, this is somewhat a remnant of a previous way that I made this.)

```r
map_layers <-
  c(
    "Land"           = "#FFD23A",
    "Mountain"       = "#FF8D51",
    "Shallow marine" = "#45D8FF"
  )
```

With that ready, the following code reads and tidies the data (as for above), then adds a column (geog_layer) to identify the data layer (shallow marine, land, mountain) and joins all the data together. It's very _tidyverse_ based, so I'd suggest looking at <https://www.tidyverse.org> to see the rationale and how they link using the _pipe_ (`%>%`). You can also see more about individual functions in the R help.

The final step is to create a column (polygon_id) to identify each region on the, which come after mountain map. I had a long issue where different continents would join up, or Australia's land would always appear plotted underneath the sea. As described above, this is because the shapes on the map are plotted as separate polygons, and the IDs are repeated between each of the layers. We'll use the polygon_id column to make sure that each polygon has a unique ID by joining the layer, group and id name from the tidied data.

```r
polygon_data <- 
  list.files("data/palaeogeography/", pattern = ".gmt", full.names = TRUE) %>%
  map(readOGR) %>%
  map(tidy) %>%
  map2(
    names(map_layers),
    ~ add_column(.x, geog_layer = .y)
  ) %>%
  bind_rows() %>%
  mutate(
    polygon_id = str_c(id, group, geog_layer) %>% as_factor(),
    geog_layer = factor(geog_layer, labels = names(map_layers))
  )
```

### Arranging the map layers

At this point it's important to have the map layers in the correct order, so that marine is plotted under land, which is plotted under
mountain.

This piece of code does that for you, ensuring shallow marine is first (it was third in `map_layers` above) then assigning this order to the polygon_id column. It converts the polygon_id column to a factor and re-orders thee levels based on the reordered `map_layers`.

```r
id_level_order <-
  map_layers[c(3, 1, 2)] %>%
  map(
    function(layr) polygon_data$polygon_id %>%
      levels() %>%
      str_which(layr)
  ) %>%
  unlist()

polygon_data <-
  polygon_data %>%
  mutate(
    polygon_id =
      fct_relevel(polygon_id, levels(polygon_id)[id_level_order])
  )
```

### References

Cao, W., Zahirovic, S., Flament, N., Williams, S., Golonka, J. and Müller, R.D. 2017. Improving global paleogeography since the late Paleozoic using paleobiology. <i>Biogeosciences</i> 14 (23): 5425–5439. [doi:10.5194/bg-14-5425-2017](https://doi.org/10.5194/bg-14-5425-2017) 
