# Running Batch Jobs with `kbatch`

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

