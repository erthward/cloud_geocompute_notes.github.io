[Google Earth Engine (GEE)](https://earthengine.google.com/) has become an extremely popular
option for cloud geocomputation.
It has been around much longer than most other options, and it is the computational infrastructure
that sits beneath a number of groundbreaking studies and datasets
(e.g., the [Hansen global forest change dataset](https://developers.google.com/earth-engine/tutorials/tutorial_forest_02)).
It is a really powerful cloud environment with a really nice, high-level Javascript (and Python) API that abstracts away nearly all considerations of data management and parallelization, allowing a researcher
to write simple, expressive code the jumps directly into the analytical computational steps.
This comes with a few trade-offs, IMHO, including:
- the [client-server](https://developers.google.com/earth-engine/guides/projections) separation can make debugging bumpy and confusing, particularly during the earliest and steepest part of the learning curve
- management of [CRS and projections](https://developers.google.com/earth-engine/guides/projections) is unconventional compared to more traditional, desktop-style GIS worklows
- [scale](https://developers.google.com/earth-engine/guides/scale) management is unconventional as well, and can lead to confusing, misleading, or 'wrong' results if it is not carefully considered throughout the analysis
- because GEE requires use of its own proprietary API (i.e., open-source Javascript and Python packages cannot be integrated with GEE code, as they cannot be converted to server-side machine instruction), it is an excellent tool for a circumscribed set of spatial analysis workflows and tasks, but it is not a general-purpose toolset (like a scripting language) and many custom operations will need to be moved outside GEE, either because GEE's design does not suit them well (e.g., permutation-based/Monte Carlo analysis) or because attempting to execute them at the desired scale will quickly run into [compute-time or memory constraints](https://developers.google.com/earth-engine/guides/usage) 


Beyond that, [GEE's own documentation](https://developers.google.com/earth-engine/guides)
and the many other tutorials, discussion boards, and StackExchange posts that are available on the web are a much better resource than I could ever hope to recreate.
