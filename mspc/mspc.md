# Overview:
[MSPC](http://planetarycomputer.microsoft.com/) is a free-to-use geocomputing environment created and provided by Microsoft.
It offers:
1. a [Data Catalog](http://planetarycomputer.microsoft.com/docs/overview/about) with a lot of commonly used datasets ready to go (and worked examples to use as a quick-start);
2. a [Hub](http://planetarycomputer.microsoft.com/docs/overview/environment/) for doing all of your computing work;
3. really thorough [Documentation](http://planetarycomputer.microsoft.com/docs/overview/environment/);
4. a friendly and responsive [Discussions](http://github.com/microsoft/PlanetaryComputer/discussions) where you can seek help from the growing user community.

The Hub is built using [JupyterHub](https://jupyter.org/hub), and provides 
environments to working in Python (CPU and GPU) and R notebooks, as well as QGIS.
The Python environment is based on the [PanGeo stack](https://pangeo.io/)
(including the powerful combination of core packages:
[rioxarray](https://corteva.github.io/rioxarray/stable/),
[geopandas](https://corteva.github.io/rioxarray/stable/),
and [dask](https://www.dask.org/)).

The Hub is set up with dask, allowing you to create and use cloud clusters
to easily (well, relatively easily! see details below...) parallelize large or demanding jobs. (**Note**: Parallel computing adds extra levels of debugging: not just your main analysis code,
but also the parallel perfomance of that code and the configuration
and resource-use of your cluster, using new tools such as the [dask dashboard](https://docs.dask.org/en/latest/dashboard.html).
Thus, save yourself the work and don't parallelize until you know you need to!)

The Data Catalog, and the workflows for which MSPC was designed,
use the [SpatioTemporal Asset Catalog (STAC)](https://stacspec.org)
specification. This specification creates [JSON](https://en.wikipedia.org/wiki/JSON#Syntax) files
that index arbitrarily complex, spatiotemporal datasets
(think single-band image files, nested within single-date, multiband image acquisitions,
 nested within long-term imagery archives, nested within multi-archive
catalogs of data...).
The actual data files are stored as BLOBs (binary large objects) in
cloud-optimized formats (namely, [Cloud Optimized Geotiffs (COGs)](https://www.cogeo.org/)
in Azure's `west_europe` region (where the MSPC computing happens).
This whole setup allows analyses to, with minimal code, slice and dice
the data along space-time axes
(e.g., "give me this dataset for these 2 years and within this lat-lon bounding box"),
then stream just the necessary spatiotemporal portions of the data
into the analysis at (and not before) the time a final computed result is requested.

All in all, this cloud-native workflow is staking out the future of geocomputation
in our fields.
It resolves a number of limitations of the old, bare-metal way of using servers,
but brings with it a new set of tools and a new learning curve.
Read on to start learning the ins and outs and start getting things done!


# Quick-start:
1. [Request access](http://planetarycomputer.microsoft.com/).
2. Peruse the [Data Catalog](http://planetarycomputer.microsoft.com/docs/overview/about) and choose a dataset of interest.
3. Open that dataset's Example Notebook ([e.g.](https://planetarycomputer.microsoft.com/dataset/landsat-c2-l2#Example-Notebook)).
4. Launch the [Hub](http://planetarycomputer.microsoft.com/docs/overview/environment/).
5. Start copying, pasting, and playing with code from the Example Notebook!


# Details:
- [the Hub](erthward.github.io/mspc/hub.md)
- [STAC](erthward.github.io/mspc/hub.md)
- [dask](erthward.github.io/mspc/dask.md)
- [using your own data](erthward.github.io/mspc/own_data.md]


