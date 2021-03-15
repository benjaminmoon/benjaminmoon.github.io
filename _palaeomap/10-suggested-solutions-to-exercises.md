---
last_modified_at: 2021-03-15
title: Suggested Solutions to Exercises
---

Not all of the exercise questions have a single obvious solution, particularly ones where you pick your favourite period. But here are some code suggestions for you to check against.

### Exercises 1

1. Find an example of a palaeogeographical map for your favourite time period.
  - You can have a look at Deep Time Maps or Paleomap for easy examples, or you can find others in papers.
2. Have a look in the folder `GplatesSampleData` to see what other projects are included. 
  - The `GplatesSampleData/FeatureCollections` directory is the one to look at. It includes:
    + Other plate reconstructions.
    + Ocean boundaries.
    + Plate flow lines.
    + Large igneous provinces … among other things.

### Exercises 2

1. Have a look at the [GWS documentation](<https://github.com/GPlates/gplates_web_service_doc/wiki>). What address would you use to get reconstructed coastlines in the early Carboniferous using the @Matthews2016GPC model?
  - `"https://gws.gplates.org/reconstruct/coastlines/?&time=350&model=MATTHEWS2016_mantle_ref"`
  - You could also use `"MATTHEWS2016_pmag_ref"`. These are two different reconstruction methods.
2. Open an example of the OGR-GMT data from the GWS, either from the `data/GWS` folder or by downloading it. Can you see how the polygons are stored? How are different polygons identified?
  - The metadata at the top has lines beginning with `# @`, including describing how the data for each polygon is organised.
  - Each polygon has a line of metadata describing it and `@P` indicating it's a polygon.
  - Sometimes without metadata for each polygon, they are indicated with numbers: `@P1`, `@P2` etc.
3. Now look at the tidied version in R, i.e. `kimmeridgian_coastlines` or `kimmeridgian_polygons`. By default this will only show the first 10 rows, but you can see more with, for example, `head(kimmeridgian_polygons, n = 50L)`. Can you see how the OGR-GMT data is transferred to this tidy format?
  - The data is stored in a tibble with each point in a row and columns for the longitude and latitude of the points.
  - The order that the points should be plot and the piece, group, and id of the polygon that they belong to are also included to separate the shapes.
  - These ID columns are taken from the metadata for each polygon define in the input OGR-GMT file.
4. Play around with the inputs to the plot above. How messy or colourful can you make this map? What options are there in `theme_map()`? (Hint: use `?theme_map` to see.)
  - I'll leave this to your imagination.

### Exercises 3

1. Try plotting this map with different projections (see above and `?mapproject`). Does this work well for all projections? Why?
  - Some of the projections move the line of 0° longitude away from the centre or have polygons that cross from one side of the map to the other. This can sometimes lead to shapes being drawn right across the map. I'm not sure whether the best way to counteract this is in redrawing the polygons or something to do with the map projection.
2. Look at the code behind `palaeogeog_map_niceties()` (in `functions/palaeogeog_map_niceties.R`). Modify this to add different lines of latitude. Can you add lines of longitude too? What about changing the colour?
  - In `palaeogeog_map_niceties` I stored the lines of latitude that I want as a tibble that describe _line segments._ New lines of latitude can be added by creating a new row with the value _y_ set to your latitude. The lines are described as going from -180°–+180° longitude at a specific single latitude.
  - For lines of longitude, I'd suggest using a new `geom_segment` layer. Modify the code so that the _y_ value covers the range of latitudes –90°–+90° at a specific longitude. Here's an example drawing lines of longitude at -50°, 0° and 72°:

```r
geom_segment(
  data = tibble(
    x = c(-50, 0, 72),
    y = rep(-90, 3),
    yend = rep(90, 3)
  ),
  aes(x = x, xend = x, y = y, yend = yend),
  colour = "grey70", size = 0.3
)
```

3. Can you try creating your own palaeogeographical map for your favourite time? This should be done on your own computer. Follow the instructions above to export data from GPlates then plot in R, making sure you change all the file and object names.
  - here you'll have to modify the code above for your own GPlates data. Remember to add in the ice caps to the layer if you pick a time with these, such as the Carboniferous or the Neogene.

### Exercises 4

1. Look at the PBDB web API (<https://paleobiodb.org/data1.2>). What other options and data sets can you access with this? Can you craft a URL that will download occurrence data of Tithonian bivalves.
  - "https://paleobiodb.org/data1.2/occs/list.csv?base_name=Bivalvia&interval=Tithonian&show=paleoloc" 
2. Try adding colour or shapes to your occurrence points using the options in `geom_point`.
  - Use the `colour` and `shape` arguments.
3. Download and plot the palaeogeographical map and occurrences for your favourite group in your favourite stage. Again this should be done on your own computer. Beware it may take a while if you want to plot many occurrences.
  - Use the code above, but change the relevant URLs and data files to your own GPlates and PBDB data. Don't forget to add ice caps if you have them.
  - Hopefully the PBDB API will be working as it wasn't when I was writing this!
4. It's also useful to separate out occurrences on the map, for instance, can you colour occurrences within the outside the tropics differently? say red in the tropics (±23.5°) and blue outside.
  - You'll need to assign a column of identifiers for tropical and non-tropical occurrences. Below is an example using the _dplyr_ function `case_when`.
  - With that done it'll fit right into the plotting code from earlier, but you will need to specify the `colour` argument. If you want to make it blue and red then you can add a scale.

```r
ex_occ_ichthyosaurs <-
  occ_ichthyosaurs %>%
    mutate(
      tropical = case_when(
        paleolat <= 23.5 & paleolat >= -23.5 ~ "tropical",
        paleolat > 23.5 | paleolat < -23.5   ~ "non-tropical"
      ) %>%
        as_factor()
    )

ex_occ_plot <-
  map_plot +
    geom_point(
      data = ex_occ_ichthyosaurs,
      aes(
        x = paleolng,
        y = paleolat,
        colour = tropical
      ),
      na.rm = TRUE
    ) +
    scale_discrete_manual(
      values = 
        c(
          "tropical" = "red",
          "non-tropical" = "blue"
        ),
        aesthetics = c("colour"),
        name = "Latitude"
      )
ex_occ_plot
```
