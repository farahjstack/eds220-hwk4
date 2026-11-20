# Mapping the 2025 Palisades and Eaton Fires with False Color Satellite Imagery

## About

This repository presents an analysis of the January 2025 Palisades and Eaton fires in Los Angeles County using Landsat 8 satellite imagery and fire perimeter data. The analysis uses raster and vector processing in Python to compare true color and false color composites of the burned areas.

A shortwave infrared (SWIR) / near-infrared (NIR) / red false color composite is combined with both fire perimeters in a single map. False color assigns wavelengths we can't see to display colors, which makes burn scars and healthy vegetation easier to tell apart than in a true color image.

## Repository Structure

```
eds220-hwk4/
├── data/                                 # Local data folder (excluded from Git)
├── hwk4-task2-false-color-STACK.ipynb    # Notebook with code, maps, and written interpretation
├── hwk4-task2-false-color-STACK.pdf      # Rendered PDF of the notebook
├── .gitignore                            # Excludes the data/ folder from Git
└── README.md                             # Project overview, data information, methods, and references
```

## Data

The data are not stored in this repository. Download the course data and place all files directly in a `data/` directory at the top level of the repository, with no subfolders. [Add where the course data came from, such as a course link or server path.]

### Sources

- **Landsat 8 imagery:** `landsat8-2025-02-23-palisades-eaton.nc`, a multi-band surface reflectance scene from February 23, 2025 covering the Palisades and Eaton fire areas. It contains the `red`, `green`, `blue`, `nir08`, and `swir22` bands on a 1418 × 2742 pixel grid. [Add the original provider, for example USGS Landsat Collection 2 Level-2, and how it was accessed.]
- **Fire perimeters:** `Eaton_Perimeter_20250121` and `Palisades_Perimieter_20250121` shapefiles, from the LA County GIS Data Portal layer [Palisades and Eaton Dissolved Fire Perimeters (2025)](https://data.lacounty.gov/maps/ad51845ea5fb4eb483bc2a7c38b2370c/about). The source is CAL FIRE (originally NIFC FIRIS). Each shapefile holds one `MultiPolygon` feature in NAD83 (EPSG:4269).

## Reproducing the Analysis

1. Clone or download this GitHub repository.
2. Download the data and place the files directly in the `data/` directory.
3. Install the required Python packages (see Methods).
4. Open `hwk4-task2-false-color-STACK.ipynb` in Jupyter, VS Code, or another notebook environment.
5. Check that the file paths in the data-loading cells match your local folder structure.
6. Run all cells to generate the plots and map, then export the notebook to PDF.

## Methods

Spatial data were processed and visualized in Python using the following packages:

- os
- numpy
- geopandas
- xarray
- rioxarray
- matplotlib

The fire perimeter data were explored to identify their coordinate reference system (CRS), which is geographic (NAD83, EPSG:4269) and therefore not projected. The Landsat data were imported with `xarray.open_dataset()` and explored to describe their variables, dimensions, and coordinates. Because the geospatial information was stored in the `spatial_ref` variable and not recognized as a CRS, it was restored by reading `spatial_ref.crs_wkt` and applying `rio.write_crs()`.

A true color image was created from the red, green, and blue bands, using `robust=True` to adjust the plotting scale. Bands containing NaN values were identified, and the NaNs were replaced with zero using `.fillna()` so the image plots without warnings. A false color image was then created from the SWIR (`swir22`), NIR (`nir08`), and red bands, in that order. Finally, both fire perimeters were reprojected to the raster CRS and plotted over the false color image with a title and legend.

## Author
Farah Stack

## References
- (2025). *Landsat 8 surface reflectance, Palisades and Eaton fire area, February 23, 2025* [Data set].
- County of Los Angeles. (2025). *Palisades and Eaton dissolved fire perimeters (2025)* [Data set]. LA County GIS Data Portal. https://data.lacounty.gov/maps/ad51845ea5fb4eb483bc2a7c38b2370c/about
- Bennett, M. M., Chen, J. K., Alvarez León, L. F., & Gleason, C. J. (2022). The politics of pixels: A review and agenda for critical remote sensing. *Progress in Human Geography, 46*(3), 729–752. (Check the details against the paper.)
- NASA Earth Observatory. *Why is that forest red and that cloud blue? How to interpret a false-color satellite image.* https://earthobservatory.nasa.gov/features/FalseColor
- UCSB Master of Environmental Data Science. *EDS 220: Working with Environmental Datasets.* https://meds-eds-220.github.io/MEDS-eds-220-course/
