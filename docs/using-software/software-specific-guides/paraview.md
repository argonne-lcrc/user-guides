## Using Paraview in LCRC

Here, we will outline how to use Paraview in client-server mode in LCRC from a local machine running Linux/MacOS. Below are examples using the Bebop cluster. Paraview itself **should not** be launched on any of the login nodes, but instead be run in client-server mode.

### Paraview Client-Server to an LCRC Login Node
In this first example, commands are shown for beboplogin3 and should be changed appropriately if you are using another login node.

First, login to Bebop for this example:
```
# ssh <username>@bebop.lcrc.anl.gov
```

Make note of the login node name and number that you end up on. As mentioned for this example, it is beboplogin3, but may be different for you.

Now, load the Paraview module (in this example we are using paraview 5.4.0. Users can check the available versions of paraview by using the command `module spider paraview`. Load any of the modules of paraview available and ensure that you have the same version available on your local machine.):
```
# module load paraview/5.4.0
```

You can verify that Paraview is loaded if you can produce this output:

```
# which pvserver
/soft/bebop/paraview/5.4.0/bin/pvserver
```

Launch pvserver in your case directory:
```
# pvserver
```

You should get output similar to the following:
```
Waiting for client...
Connection URL: cs://beboplogin3:11111
Accepting connection(s): beboplogin3:11111
```

Now, open new terminal on your local machine/desktop (Mac/Linux). Create an SSH tunnel to the login node you started pvserver on:

```
# ssh -L 11111:localhost:11111 <username>@beboplogin3.lcrc.anl.gov
```

Back on your local machine, download and install the Linux/MacOS Paraview version 5.4.0 (or the matching version from the Paraview module you loaded in the first step) from the [Paraview website](https://www.paraview.org/download/).

Once installed, launch Paraview locally and set up the connection to connect to our server instance.

Click on the icon for *Connect* (marked in red circle on the figure below) and then click on the *Add Server* tab:
![Using Paraview 1](https://apsacquia2stg.prod.acquia-sites.com/sites/default/files/2023-11/using-paraview-1.png)

Fill in the details as shown below. You should use the port number given to you from when you started the pvserver instance if different than below. Set the name as one of the login nodes, beboplogin2, beboplogin3, etc. and hit *Configure*:
![Using Paraview 2](https://apsacquia2stg.prod.acquia-sites.com/sites/default/files/2023-11/using-paraview-2.png)

This will take you to the next tab where you can hit *Save* to save the configuration (Startup Type is *Manual* which is the default setting):
![Using Paraview 3](https://apsacquia2stg.prod.acquia-sites.com/sites/default/files/2023-11/using-paraview-3.png)

After adding a server, you should see a panel as shown below. You could add multiple servers using the above procedure. Click on the server name where you have started pvserver (in this example it would be beboplogin3) and hit *Connect*:
![Using Paraview 4](https://apsacquia2stg.prod.acquia-sites.com/sites/default/files/2023-11/using-paraview-4.png)

To verify your connection was successful (aside from no error messages locally), you should now see a connection message on the window running pvserver (note the last line below):

```
Waiting for client...
Connection URL: cs://beboplogin3:11111
Accepting connection(s): beboplogin3:11111
Client connected.
```

Once connected, you should be able to open your solution files in your case directory on Bebop (since pvserver was launched from it). Open a case file clicking on the icon marked in the red circle below:
![Using Paraview 5](https://apsacquia2stg.prod.acquia-sites.com/sites/default/files/2023-11/using-paraview-5.png)

Your solution files should now be running in client-server mode.

### Paraview Client-Server to an LCRC Compute Node (GPU Example)
Users who need to visualize large problems will have to use a compute node with more resources, preferably on a GPU resource. The below is an example of how to do this on the LCRC Swing cluster.

Note: If needed, please reference the first section of this page for any screenshots needed if you have trouble finding an option in the Paraview GUI.

First, login to Swing for this example:

```
# ssh <username>@swing.lcrc.anl.gov
```

Make note of the login node name and number that you end up on.

Request an interactive job on 1 node and 1 GPU (for 30 minutes as an example):

```
# salloc -N 1 --gres=gpu:1 -t 00:30:00 -A <project_name>
```

Note: The partition may not have free nodes so you might not be able to start your interactive job immediately. If a node is available and your job starts, you should see something like:
```
salloc: Granted job allocation <job_number>
```

Once your job starts, check the node number granted to your job:

```
# squeue -u $USER
```

You should see something like:

```
JOBID PARTITION NAME USER ST TIME NODES NODELIST(REASON) 
<job_number> gpu bash <username> R 0:04 1 <node_number>
```

SSH into the node allocated to your job (say gpu1 for example):
```
# ssh <node_number>
```

Change to bash shell if you aren’t already using it:
```
# bash
```

Now, load the Paraview module:
```
# module load paraview/5.11.1
```

You can verify that Paraview is loaded if you can produce this output:

```
# which pvserver
/gpfs/fs1/soft/swing/manual/paraview/5.11.1/bin/pvserver
```

Launch pvserver in your case directory:
```
# pvserver --server-port=11111
```

You should get output similar to the following:

```
Waiting for client...
Connection URL: cs://<node_number>:11111
Accepting connection(s): <node_number>:11111
```

From your local Linux/MacOS machine type the following command to set up the tunnel to the allotted node:
```
# ssh -L 11111:<node_number>.lcrc.anl.gov:11111 <username>@gpulogin<node_number>.lcrc.anl.gov
```

For example, if you have logged onto gpulogin1 to launch your interactive job and your allotted node number is gpu1 then your command will be:
```
# ssh -L 11111:gpu1.lcrc.anl.gov:11111 <username>@gpulogin1.lcrc.anl.gov
```

Launch Paraview from your local machine and connect to the gpulogin node (gpulogin1 in the above example) as explained in the first section of this page.

On the node you should see:

```
Waiting for client...
Connection URL: cs://<node_number>:11111
Accepting connection(s): <node_number>:11111
Client connected.
```

You can now navigate to the case folder and load the case as described above. After viewing your case, you can cancel the interactive job by either exiting completely out of your node session or by using Slurm:
```
# scancel <job_number>
```
