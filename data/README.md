# Earthquake Catalog Data

Official catalog search and CSV export: https://earthquake.usgs.gov/earthquakes/search/

Official ComCat access documentation: https://earthquake.usgs.gov/data/catalog/access/

The notebook loads `/content/drive/MyDrive/query (6).csv`. Its saved output shows 1,873 rows and the following columns:

```text
time, latitude, longitude, depth, mag, magType, nst, gap, dmin, rms,
net, id, updated, place, type, horizontalError, depthError, magError,
magNst, status, locationSource, magSource
```

The data schema is consistent with a USGS catalog CSV; the exact provenance and search filters were not recorded in the supplied notebook. These links provide compatible data access, not an exact reproduction of the original subset.

No raw data is included. Update the loading path after downloading a CSV and retain the query parameters and download date. The commented `Eartquakes-1990-2023.csv` filename is an alternative reference, not the active input or proof of the active dataset's time range.
