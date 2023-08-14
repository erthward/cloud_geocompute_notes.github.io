# The MSPC Hub:

The hub is built as a **PanGeo Cloud** environment in 
a **JupyterHub** instance. Let's take that apart, working backward:

According to the [PanGeo Cloud docs](https://us-central1-b.gcp.pangeo.io/hub/login?next=%2Fhub%2F), [**JupyterHub**](https://jupyter.org/hub) 
    "JupyterHub brings the power of notebooks to groups of users. It gives users access to computational environments and resources without burdening the users with installation and maintenance tasks. Users - including students, researchers, and data scientists - can get their work done in their own workspaces on shared resources which can be managed efficiently by system administrators."


[**PanGeo**](https://pangeo.io/index.html) is a community of folks who develop and provide
compute tools and a cloud-based compute workflow and environment for
big-data geocomputing.
This approach to geocomputing depends on a set of [core packages](https://pangeo.io/packages.html#packages)
that are also central to MSPC workflows
(mostly; I haven't found myself needing to use Iris at all, though...),
and thus are very worth taking the time to learn well.
(**Note**: Their documentation is excellent and, unlike most cloud computing documentation, it is built with scientists in mind rather than private industry!
I recommend starting with their 

The [**PanGeo Cloud**](https://pangeo.io/cloud.html) is a cloud-basd data-science environment
in which each user has a private, virtual environment (analogous to a user space on a bare-metal server like a campus supercomputer), called `/home/joyvan`, which is intended only for code, notes, small data, etc. (10 GB limit).
The compute environment is accessed through the browser, so you just log in there,
and you can up/download smaller objects/files directly to the home directory
(though you should defer to using [Git](https://git-scm.com/)/[GitHub](https://github.com/) as much as possible).
Data of any 'meaningful' size should not be stored in [Azure](https://azure.microsoft.com/en-us) cloud storage instead. (**Note**: they *must* be stored in the `westeurope` Azure region, so that are colocated with the MPSC servers.)

(FYI: Cloud storage is effectively a key-value system,
where keys are strings and values are bytes of data.
Data is read and written using HTTP calls.
Importantly, performance of object storage is very different
from file storage; according to the
[PanGeo docs](https://pangeo.io/cloud.html#cloud-object-storage):
    "On one hand, each individual read / write to object storage has a high overhead (10-100 ms), since it has to go over the network. On the other hand, object storage “scales out” nearly infinitely, meaning that we can make hundreds, thousands, or millions of concurrent reads / writes. This makes object storage well suited for distributed data analytics. However, the software architecture of a data analysis system must be adapted to take advantage of these properties. All large datasets (> 1 GB) in Pangeo Cloud should be stored in Cloud Object Storage."



# Interactive Jobs

The following will walk you stepwise through an interactive session in the Hub (with images):

1. Navigate to the [MSPC homepage](https://planetarycomputer.microsoft.com/). ![image](mspc_homepage.png)

2. Click on the 'Hub' tab at top, choose the notebook server environment you wish to work in, then click the blue 'Start' button. (Note: the 'CPU - Python' option will use the Pangeo Notebook environment discussed above) ![image](hub_choose_environ.png)

3. After some amount of time, your server will start up and you will be redirected to the a Jupyterhub instance with a landing-page README that you can use to practice/learn more. ![image](jupyterhub_landing.png)

4. After closing that README tab, you should see a Launcher tab with a number of notebook, console, and other options. ![image](jupyterhub_launcher.png)

5. Before you start working in a notebook by clicking on a notebook logo within the Launcher, you may want to be sure you choose a working directory to work in. Use your mouse (or <Ctrl>-B) to open the bookmark tab at the left. ![image](jupyterhub_bookmark_tab.png)

6. Navigate 'upward' through the directory tree by clicking on the parent directory's folder icon to the left of the current working directory's name (which should be "PlanetaryComputerExamples"). This will put you in your home directory on the Hub. Then click the new directory icon above (folder with a plus sign in it) and create and name a new directory (here, labeled 'my_MSPC_work'). ![image](jupyterhub_new_folder.png)

7. Double-click on that new folder to navigate 'downward' into it, then launch a Python 3 (ipykernel) notebook using the Launcher tab's icon. ![image](jupyterhub_new_nb.png)

8. You can now use the Jupyter notebook interface as you may have before ([here](https://docs.jupyter.org/en/latest/) is a good starting point if this interface is new to you) to rename your notebook, do some work, and save your work as you go. Working in this new subdirecotry/folder that you created for yourself ('my_MSPC_work' in the example above) will allow you to save your work in a way that it is not wiped between sessions, and thus is still available). ![image](jupyterhub_new_project.png)


9. When finished working for now, don't forget to save your work, close your notebook (a Launcher tab will reappear to take its place), and then click File -> Hub Control Panel to get to the controls for your running server. ![image](jupyterhub_finish_work.png)

10. When you arrive at the Hub Control Panel page, double-click 'Stop My Server' to stop it from running' ![image](hub_stop_server.png)


If you want to work with code that is version-controlled on GitHub (highly recommended!), then the following steps will get you set up:

1. In a new browser tab/window, visit the GitHub repository (or 'repo') that you want to clone (i.e., copy) the code of, click the green "<> Code" button, then click the copy icon next to the link that is displayed. ![image](github_copy_repo_url.png)

2. Back in the MSPC browser tab/window, use the Launcher tab to launch a bash terminal. ![image](jupyterhub_launch_terminal.png)

3. Call the bash `cd ~` command (i.e., type `cd ~` into the terminal's command line, then hit <Enter> to execute it) to make sure you have navigated to your home directory. ![image](jupyterhub_cd_home.png)

4. Type `git clone ` into the terminal, then use <Ctrl>-V to paste the GitHub repo's URL into the command line, then hit <Enter>. (Note: If you are attempting to clone a private GitHub repo to which you have access, you will be prompted to authenticate. Enter your username, press <Enter>, then enter your password (type carefully; for security purposes, no characters appear when passwords are typed into the command line), and press <Enter> again.) ![image](jupyterhub_git_clone.png)

5. Once the command has finished running successfully (you will be returned to a new command line that is awaiting further input), you can click the terminal tab's 'x' to close it (a new Launcher tab will reappear in its place again), navigate into the newly cloned directory using the bookmarks tab at the left, and double-click on any Jupyter Noteook files (.ipynb extensions) or other files you wish to edit to begin working with them on the Hub. ![image](jupyterhub_repo_nb.png)


# Dask
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

# Batch Jobs with `kbatch`

On a standard cluster, such as a campus supercomputer, the workflow for developing a parallel job could look something like this:

1. write basic analysis code, on your local computer, and debug for a small data sample
2. parallelize that code, and try out locally on a small data sample (Note: dask can also be used to run parallel computations elsewhere, even on your laptop!)
3. move the code and data to the cluster, then confirm that the code runs correctly there
4. begin scaling up the analysis to larger portions of your data, potentially monitoring its performance and assesing its runtime interactively
5. use a cluster management/job scheduling tool (e.g., slurm) to submit the full-scale analysis as a batch job 

On the MSPC Hub, you can follow a roughly similar workflow, or adjust as meets your needs. 
However, when it comes time to submit your full job, instead of slurm or other common tools,
the Hub is set up to use `kbatch` to handle 'batch jobs' (put in air quotes because
they are really not truly batch jobs in the same way that slurm/etc work; instead,
kbatch is just a way you can submit from outside the Hub (e.g., from your laptop's command
line) the same commands that you may otherwise execute from within the Hub).

`kbatch` is a simple tool, with only a few commands. Instructions for use on MSPC are
[here](https://planetarycomputer.microsoft.com/docs/overview/batch/), and usage
examples taht you can use to help run your own job are available in the `kbatch` docs
[here](https://kbatch.readthedocs.io/en/latest/examples/index.html). In very short, the basic steps are:

1. install `kbatch` wherever you will be using it (should be a one-time job)
2. configure `kbatch` to run jobs on the Hub (`kbatch configure --kbatch-url=https://pccompute.westeurope.cloudapp.azure.com/compute/services/kbatch --token=<YOUR_TOKEN>`, where
`<YOUR_TOKEN>` is an API token that you can generate on the Hub's [token generation page](https://pccompute.westeurope.cloudapp.azure.com/compute/hub/token)
3. use the `kbatch submit` command to submit a job
4. use the `kbatch job list -o table` to output a table reporting the statuses of your jobs
5. use the `kbatch pod logs <POD_NAME>` command to check the job's logged output (where the `<POD_NAME>` string refers the 'Kubernetes pod', or collection of networked Linux virtual machines, that is operating as your cluster)
6. there are a handful of other commands and options you can use to track progress and performance

A few things to note:

- jobs will not have access to your JupyterHub home directory on the MSPC Hub, so you will need to [submit any dependent code files](https://kbatch.readthedocs.io/en/latest/user-guide.html#submitting-code-files) (e.g., modules you wrote) along with your job
- personally, I did not find it very obvious enough how to capture the `<POD_NAME>` (and `<JOB_NAME>`) strings, so here is a bash script I wrote to submit a job and print them out cleanly (for use in commands like `kbatch pod logs <POD_NAME>`):
```bash
jobstr=$(kbatch job submit --file=job_conf.yaml --output=name)
jobname=$(echo $jobstr | rev | cut -d' ' -f1 | rev)
logstr=$(kbatch pod list --job-name=$jobname --output=name)
logname=$(echo $logstr | rev | cut -d' ' -f1 | rev)
echo
echo "JOB NAME:"
echo $jobname
echo
echo "LOG NAME:"
echo $logname
echo
#kbatch pod logs $logname --stream
```
Save that script to a file, with `job_conf.yaml` renamed to point a YAML file that configures your job (e.g., see [here](https://kbatch.readthedocs.io/en/latest/examples/shell-script.html)), then call that script using `bash <SCRIPT_NAME.sh>`. You can now easily copy-paste the `<POD_NAME>` and `<JOB_NAME>` strings into downstream commands for checking on your job.


# Next steps:

There are plenty of quick example workflows provided in the 'Example Notebook' tabs
under each of the datasets in the MSPC Data Catalog (e.g., [GBIF](https://planetarycomputer.microsoft.com/dataset/gbif#Example-Notebook)), and they can be opened interactively, run, and messed with in the Hub by clicking the blue 'Launch in Hub' button in the page.

MSPC also provides a number of examples that are a bit longer and more detailed, and that demonstate some key workflows, in the 'Quickstarts' section of [the documentation](https://planetarycomputer.microsoft.com/docs/overview/about).

If those workflows mirror your workflow close enough to get you started, and if you
want to work exclusively with data that is already in the MSPC Data Catalog,
then you might be ready to jump in an start building an analysis!

However, if you want to work with data that is not already in the MSPC Data Catalog then you'll need to do some manual interaction with Azure storage containers. Take a quick look at [this blob-storage example](https://planetarycomputer.microsoft.com/docs/quickstarts/storage/) to get a quick glimpse of how that could work, but then move on to my notes on [using data not in the Data Catalog](byo_data.md) for much more detail!

