# Running Parallel Jobs with Dask
[Dask](https://www.dask.org) is an important part of Pangeo Cloud, and thus of the MSPC Hub.
It is used to parallelize large calculations: i.e., to break them into small pieces,
run them all simultaneously on a 'cluster' of networked comuter nodes, or 'workers',
and then to combined all the sub-results into a final overall result at the end,
allowing for faster real-world, or 'wall-clock', runtimes than would be possible
if the whole job were run by one single, large computer.
Dask accomplishes this by starting from you analysis code and the result
it requests, working out a graph
of sub-tasks that need to be completed and combined in certain ways in order to 
arrive at that result, and then
farming out the pieces of that 'task graph' to the workers,
collecting their answers,
combining them as needed,
and ultimately reporting the final, complete result.

On a more traditional compute cluster, such as a campus supercomputer, 
the cluster 'size' (i.e., the number of nodes) and the specs of those nodes
(e.g., number of cores, total memory) is essentially fixed, because each
node is an actual, physical piece of hardware.
In a cloud-based dask cluster, however, all of this can be specified as needed
(and the cluster size can even be set to autoscale!), because the cluster
itself is virtual: it consists of a number of independent
cloud computing instances that are networked together to exchange data.
According to the Pangeo docs, Pangeo environments (and thus the MSPC Hub's Python notebook server) are "configured to work with [Dask Gateway](https://gateway.dask.org/),
giving you the power to create a distributed cluster using many cloud compute nodes."
Thus, to do parallel computing with dask, you must first create a cluster with Dask Gateway and connect to it with a client that you can use to interact with it:

 ```python
>>> from dask_gateway import GatewayCluster
>>> cluster = GatewayCluster()
>>> cluster.adapt(minimum=2, maximum=10)  # this creates an autoscaling cluster; use cluster.scale(n) to set a fixed size.
>>> client = cluster.get_client()
 ```

That will create a Dask cluster (with any default settings not overridden here, e.g., number of cores and memory per worker).
Once that is done, any computations run using Dask will automatically be done on the cluster.
The `cluster` and `client` objects will contain a link to your [dask dashboard](https://docs.dask.org/en/latest/dashboard.html), which you can click and open
in another tab/window and then visually inspect and debug while you run your code.


By default, large cloud-optimized datsets (e.g., [cloud-optimized geotiffs (COGs)](https://www.cogeo.org/))
will be read in by dask as dask array objectss: i.e., collections of dask array objects that together represent
the overall raster dataset's numerical array, broken into 'chunks' of some size.
These dask arrays are 'lazy', meaning that they initially only contain data on their
size (i.e., x by y by z dimensions), contents (whether values are bits, bytes, 64-bit floating-point decimals, etc.),
spatiotemporal metadata (where on earth they belong and what time point or time period
they represent) and so forth, and then they only read in the actual data (i.e., the array
contents) at the last possible moment, when they are needed in order to provide 
the requested final result.

The general workflow for running a parallel computation with dask consists of:

1. using dask to create a cluster (i.e., a group of worker nodes, overseen by a main node, which are the computers that will run all of the simultaneous, chunked-out parts of the overall analysis);

2. reading the data in as a lazy dask array (including arguments to dictate the size of the chunks that it will be broken into for parallelization);

3. writing the code to put that data through all the analytical steps needed to get the result, saving the intermediate results (themselves also dask arrays, containing metadata about the sizes and data types that will result from each step, without holding any actual results yet);

4. calling a command that requests the final result (e.g., `.compute()`, `.plot()`, etc.);

5. watching the dask dashboard as the computation runs, and using its rich visual information to understand, debug, and optimize the parallel performance of your code.

Parallelization can be the key for running a computation that would otherwise be prohibitively slow. For this reason it is an invaluable tool in the modern scientific computing toolbox. However, it adds extra layers of complexity (writing the parallel code, configuring the cluster), and thus extra levels of (often difficult) debugging! For this reason, I recommend
that you **only consider parallelization if you cannot otherwise optimize and run your code
on a single **!

If that is the situation you find yourself in, then a great place
to start is to familiarize yourself with the [Dask best practices](https://docs.dask.org/en/latest/array-best-practices.html).
A couple highlights to mention:
- "If you use a distributed cluster, use [adapative mode](https://jobqueue.dask.org/en/latest/index.html#adaptivity) rather than a fixed size cluster; this will help share resources more effectively." (However, I have also seen warnings against defaulting to adaptive cluster sizing, to avoid the overhead of communication between the workers and the decision about worker number that will need to be made computationally, etc. In other words: Parallel computing is complicated, there is no one-size-fits-all approach to all parallel compute jobs, and dask and cloud cluster computation is still an actively developing and changing area!)
- "Use the Dask dashboard heavily to monitor the activity of your cluster." (In my limited experience, this is definitely a crucial step, not a convenient option!)

If you plan to do parallel computing, then a good next step might be the
['Scale with Dask'](https://planetarycomputer.microsoft.com/docs/quickstarts/scale-with-dask/)
Quickstart example provided by MSPC. Below, I copy and annotate some of the code they use there, 
to provide a final, full picture of a dask workflow of the MPSC Hub, with reminders. But read on at that link for a full explanation:
```python
import dask_gateway              # for creating and connecting to a cluster
import pystac_client             # for reading and searching a STAC
import planetary_computer        # for signing the MSPC STAC API
import xarray as xr              # for reading spatiotemporal data into lazy, multidim arrays
import matplotlib.pyplot as plt  # for plotting

# create the cluster and client
cluster = dask_gateway.GatewayCluster()
client = cluster.get_client()

# set the cluster size
cluster.scale(4)

# print out the dask dashboard link (NOTE: can be clicked on to view the dash!)
print(cluster.dashboard_link)

# indicate account and container to read data from
account_name = "daymeteuwest"
container_name = "daymet-zarr"

# read the STAC catalog, signing the MSPC API
catalog = pystac_client.Client.open(
    "https://planetarycomputer.microsoft.com/api/stac/v1",
    modifier=planetary_computer.sign_inplace,
)

# get a specific asset
asset = catalog.get_collection("daymet-daily-hi").assets["zarr-abfs"]

# load the asset into a lazy xarray/dask array object
ds = xr.open_zarr(
    asset.href,
    **asset.extra_fields["xarray:open_kwargs"],
    storage_options=asset.extra_fields["xarray:storage_options"]
)

# get means, at each time step, across the x and y (lon and lat) dims 
timeseries = ds["tmin"].mean(dim=["x", "y"]).compute()

# plot the time series
fig, ax = plt.subplots(figsize=(12, 6))
timeseries.plot(ax=ax);
```
