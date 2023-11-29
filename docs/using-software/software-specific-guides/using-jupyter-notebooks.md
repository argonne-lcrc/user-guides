## Using Jupyter Notebooks in LCRC

Here, we will outline how to use Jupyter Notebooks in LCRC from a local machine running Linux/MacOS. Below are examples that can be done on any LCRC cluster. Jupyter Notebooks **should not** be launched on any of the login nodes, but instead be run from an interactive compute node.

In this full example, commands are shown for the Bebop cluster on beboplogin1 (login node) and bdw-0001 (compute node),but should be changed appropriately based on the cluster and nodes you actually land on.

First, login to Bebop for this example:

```
# ssh <username>@bebop.lcrc.anl.gov
```

Now we will activate a more recent version of conda for python3:

```
# /soft/anaconda3/2022.05/bin/conda init bash
```

You may see a lot of output here, but it will instruct you to reload your shell. For changes to take effect, close and re-open your current shell, preferably by logging out and back in.

Once you log back in, you should verify you have the correct conda environment loaded:

```
# which conda
```

This should output:
`/soft/anaconda3/2022.05/condabin/conda`

If you see the path above, you can move on to the next step.

Start an interactive job. You could choose any of the partitions where nodes are available. We have used the broadwell shared queue (bdws) in this example, since these nodes are always available for short testing/debugging jobs. If you need a longer queue, the KNL shared queue (knls) will allow a job up to 4 hours:

```
# srun --pty -p bdws -N 1 -t 01:00:00 /bin/bash
```

This command will drop you into 1 random shared broadwell node for 1 hour – in this example we will show it as bdw-0001.

On the compute node bdw-0001, type:

```
# conda activate
```

This will give you access to the newer conda environment now.

Now run:

```
# which jupyter
```
You should see:
`/soft/anaconda3/2022.05/bin/jupyter`

confirming you have the right Jupyter executable loaded.

Now, launch Jupyter Notebook on the compute node (again, bdw-0001 in our case):

```
# jupyter notebook --no-browser
```
After several seconds, you should see something like:
```
[I 2022-01-01 10:32:07.703 LabApp] JupyterLab extension loaded from /soft/anaconda3/2022.05/lib/python3.9/site-packages/jupyterlab
[I 2022-01-01 10:32:07.703 LabApp] JupyterLab application directory is /gpfs/fs1/home/software/anaconda3/2022.05/share/jupyter/lab
[I 10:32:07.710 NotebookApp] Serving notebooks from local directory: /gpfs/fs1/home/username
[I 10:32:07.710 NotebookApp] Jupyter Notebook 6.4.8 is running at:
[I 10:32:07.710 NotebookApp] http://localhost:8888/?token=00a3ae070d595e0862585628381cx67e86a8421ea8b030a5
[I 10:32:07.710 NotebookApp]  or http://127.0.0.1:8888/?token=00a3ae070d595e0862585628381cx67e86a8421ea8b030a5
[I 10:32:07.710 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 10:32:07.717 NotebookApp]

    To access the notebook, open this file in a browser:
        file:///gpfs/fs1/home/username/.local/share/jupyter/runtime/nbserver-31221-open.html
    Or copy and paste one of these URLs:
        http://localhost:8888/?token=00a3ae070d595e0862585628381cx67e86a8421ea8b030a5
     or http://127.0.0.1:8888/?token=00a3ae070d595e0862585628381cx67e86a8421ea8b030a5
```

Please make a note of the provided URL at the bottom of the output (make sure to use your output and not the example text above). You will need that later. Also, please note that the port number after localhost may be different. In this example, it is ***8888***. You should change the below commands to match this port where necessary.

From another terminal on your local machine, run:

```
# ssh -L 8888:localhost:8888 <username>@beboplogin1.lcrc.anl.gov
```

This will open a new, port forwarded session on beboplogin1. You may encounter a scenario where the default port of 8888 is already in use and see something like:

```
bind [127.0.0.1]:8888: Address already in use
channel_setup_fwd_listener_tcpip: cannot listen to port: 8888
Could not request local forwarding. 
```

If this happens, you may want to change the port altogether. You can change it by adding the following to your Jupyter Notebook command from earlier. Simply cancel the process and restart the process with:

```
# jupyter notebook --port=<port number> --no-browser
```

We recommend you simply increase the port number by one to the next value from the default. For example, 8889 or 8890 if others are in use.

Now, from this new session on beboplogin1, SSH into the compute node running your Jupyter Notebook command from earlier:

```
# ssh -L 8888:localhost:8888 bdw-0001
```

Finally, cut-paste the URL obtained by launching Jupyter Notebook on the compute node earlier into a browser (Firefox/Chrome for example) running on your local machine.

This should launch the Jupyter Notebook on the browser of your local machine.
