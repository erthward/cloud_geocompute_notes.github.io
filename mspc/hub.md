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



# interactive jobs


# dask cluster/dashboard


# kbatch



