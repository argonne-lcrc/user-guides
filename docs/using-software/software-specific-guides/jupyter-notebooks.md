# Using Jupyter Notebooks in LCRC

Here, we will outline how to use Jupyter Notebooks in LCRC from a local machine running Linux/MacOS. Below are examples that can be done on any LCRC cluster. Jupyter Notebooks **should not** be launched on any of the login nodes, but instead be run from an interactive compute node.

In this full example, commands are shown for the Improv cluster on ilogin1 (login node) and i001 (compute node), but should be changed appropriately based on the cluster and nodes you actually land on.

First, login to Improv for this example:

```console
ssh <username>@improv.lcrc.anl.gov
```

Now we will activate a more recent version of conda for python3:

```console
/soft/software/custom-built/anaconda3/2023.09/bin/conda init bash
```

You may see a lot of output here, but it will instruct you to reload your shell. For changes to take effect, close and re-open your current shell, preferably by logging out and back in.

Once you log back in, you should verify you have the correct conda environment loaded:

```console
conda --version
```

This should output:
`conda 23.7.4`

If you see the version above (or the one you expect), you can move on to the next step.

Start an interactive job. We have used the Improv debug queue in this example, since these nodes are always available for short testing/debugging jobs. Remember to replace PROJECT_NAME with a valid project you belong to. 

```console
qsub -I -A PROJECT_NAME -l select=1:ncpus=128:mpiprocs=128,walltime=01:00:00 -q debug
```

This command will drop you into 1 random debug node for 1 hour – in this example we will show it as i001.

On the compute node i001, type:

```console
conda activate
```

This will give you access to the newer conda environment now.

Now run:

```console
which jupyter
```

You should see:
`/soft/software/custom-built/anaconda3/2023.09/bin/jupyter`

confirming you have the right Jupyter executable loaded.

Now, launch Jupyter Notebook on the compute node (again, i001 in our case):

```console
jupyter notebook --no-browser
```

After several seconds, you should see something like:

```console
[I 14:00:16.503 NotebookApp] Jupyter Notebook 6.5.4 is running at:
[I 14:00:16.503 NotebookApp] http://localhost:8888/?token=3ghbn74a98fbcf4235342d7a835a6d3b5e9d19934be3786e
[I 14:00:16.503 NotebookApp]  or http://127.0.0.1:8888/?token=3ghbn74a98fbcf4235342d7a835a6d3b5e9d19934be3786e
[I 14:00:16.503 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 14:00:16.508 NotebookApp]

    To access the notebook, open this file in a browser:
        file:///gpfs/fs1/home/username/.local/share/jupyter/runtime/nbserver-2561216-open.html
    Or copy and paste one of these URLs:
        http://localhost:8888/?token=3ghbn74a98fbcf4235342d7a835a6d3b5e9d19934be3786e
     or http://127.0.0.1:8888/?token=3ghbn74a98fbcf4235342d7a835a6d3b5e9d19934be3786e
```

Please make a note of the provided URL at the bottom of the output (make sure to use your output and not the example text above). You will need that later. Also, please note that the port number after localhost may be different. In this example, it is ***8888***. You should change the below commands to match this port where necessary.

From another terminal on your local machine, run:

```console
ssh -L 8888:localhost:8888 <username>@ilogin1.lcrc.anl.gov
```

This will open a new, port forwarded session on ilogin1. You may encounter a scenario where the default port of 8888 is already in use and see something like:

```console
bind [127.0.0.1]:8888: Address already in use
channel_setup_fwd_listener_tcpip: cannot listen to port: 8888
Could not request local forwarding. 
```

If this happens, you may want to change the port altogether. You can change it by adding the following to your Jupyter Notebook command from earlier. Simply cancel the process and restart the process with:

```console
jupyter notebook --port=<port number> --no-browser
```

We recommend you simply increase the port number by one to the next value from the default. For example, 8889 or 8890 if others are in use.

Now, from this new session on ilogin1, SSH into the compute node running your Jupyter Notebook command from earlier:

```console
ssh -L 8888:localhost:8888 i001
```

Finally, cut-paste the URL obtained by launching Jupyter Notebook on the compute node earlier into a browser (Firefox/Chrome for example) running on your local machine.

This should launch the Jupyter Notebook on the browser of your local machine.
