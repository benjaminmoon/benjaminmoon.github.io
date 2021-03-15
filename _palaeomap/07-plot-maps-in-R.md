---
last_modified_at: 2021-03-15
title: "Plotting Maps in R"
---

### Easier, Automated Plotting

Unfortunately, using the same plot layer method as for the GWS data above is a little excessive. You would have to add a new annotation for each layer (just about okay for three, tedious for more), which then multiplies when plotting reconstructions of many times.

```r
# EXAMPLE CODE: DO NOT RUN
ggplot() +
  geom_map() +
  geom_map() +
  geom_map() + …
```

Instead, you can use the function `geom_polygon` and have the different layers plotted automatically with the colours assigned above. The _fill_ is the most important argument as that assigns the main bulk of the
colour. The _colour_ argument is the outline, which you shouldn't use bas that will outline the map layers and change their shape. The `colour` argument will be used when plotting occurrence points (see below). Also having the ordered and correctly grouped _polygon_id_ is important here as that determines how the individual polygons are grouped and closed, preventing shapes from spanning the globe, or incorrectly overlapping.

I'll point out here that there's quite a lot going on in the `palaeogeog_map_niceties` function at the end. This is a collection of theme options and a colour scale to show the map properly. In particular this adds the outline and the lines of the equator and tropics. I guess one thing to note is that the 'tropics', in a biodiversity sense, may not be the same in the past as the present. Increased temperatures can change the latitudinal biodiversity gradient. Also, obliquity and precession change the latitudes of the astronomical tropics through
time.

```r
# dev.new(width = 7, height = 4)
map_plot <-
  ggplot() +
    geom_polygon(
    data = polygon_data,
    aes(
      x      = long,
      y      = lat,
      fill   = geog_layer,
      group  = polygon_id
    ),
    colour = NA
  ) +
  coord_map("mollweide") +
  theme_map() +
  palaeogeog_map_niceties()
map_plot
```

You can try changing the look by modifying the projection (`coord_map`) and play about with some of the colours and markings by editing the function `palaeogeog_map_niceties` (`functions/palaeogeog_map_niceties.R`). You can also add further annotations (<https://ggplot2-book.org/annotations.html#annotations>) or
change labels as you wish.

### Going further 1: locating with modern coastlines

You may also want to add outlines of modern coastlines to show where these are and help locating. Again these are not available from GWS, but can be exported from GPlates. In GPlates, from *File \> Open Feature Collection...* go to `GplatesSampleData/FeatureCollections/Coastlines` and load `Matthews_etal_GPC_2016_Coastlines_Polyline.gpmlz`. This adds a new layer into GPlates with areas of modern coastline in their relative positions for the time. Not all the modern coastlines are there as not all of these have certain locations, or rock to be found at -- these come and go at different time points.

Like above, you can export this layer from GPlates. In this repo, I've already done that to `data/coastlines/`, which you can load with this code.

```r
modern_coastlines <-
  readOGR("data/coastlines/Matthews_etal_GPC_2016_Coastlines_Polyline_reconstructed_155.00Ma.gmt") %>%
  tidy() %>%
  add_column(geog_layer = "Modern coastlines") %>%
  mutate(
    polygon_id = str_c(id, group, geog_layer) %>%
    as_factor()
  )
```

This layer needs the same organising of IDs as the palaeogeography above, but once done can be easily added to `map_plot` above. This is where it's useful to save your ggplot to an object alongside printing it -- this you can just add new layers on top of the base map.

```r
map_plot +
  geom_path(
    data = modern_coastlines,
    aes(
      x = long,
      y = lat,
      group = polygon_id
    ),
    colour = "#888888",
    size = 0.3
  )
```

### Exercises 3

1.  Try plotting this map with different projections (see above and `?mapproject`). Does this work well for all projections? Why?
2.  Look at the code behind `palaeogeog_map_niceties()` (in `functions/palaeogeog_map_niceties.R`). Modify this to add different lines of latitude. Can you add lines of longitude too? What about changing the colour?
    -   *ggplot* objects can have new layers added to them one-by-one by using `+`, but you can also combine them into function containing a list of layers that can be added in a single line.
    -   Hint: to change the colour of the map layers, use `fill`.
3.  Can you try creating your own palaeogeographical map for your favourite time? This should be done on your own computer. Follow the instructions above to export data from GPlates then plot in R, making sure you change all the file and object names.
