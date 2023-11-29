## Using Tensorflow in LCRC

To use TensorFlow on the LCRC GPU resources, we recommend using a container. In LCRC, we support [Singularity](https://sylabs.io/guides/3.5/user-guide/index.html) for containers. Below, we’ll highlight how to get a container, load Singularity to build your container and run a simple test. Before proceeding, you should be familiar with how to run an interactive job on Swing.

### Getting the Container
To get a suitable container which has the latest CUDA and TensorFlow installed, head over to the [NVIDIA NGC](https://catalog.ngc.nvidia.com/containers) website and search for [Tensorflow](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow). Click on the TensorFlow link.

You should now see some basic information about TensorFlow. At the top of the page, you’ll notice versions next to ‘Pull Tag‘. Also notice there are containers tagged with tf1 and tf2. Make sure to select the correct version that applies to you. Choose the correct version and click **'Pull Tag'**. This will copy a Docker URL to your clipboard. More on this later. For this example, we’ll use version *21.12-tf2-py3*.

### Building your Container
Next, from a Swing login node for example, start an interactive job. In this example, we’ll request 2 GPUs.
```
gpulogin1:~$ srun --gres=gpu:2 --pty bash

```

Once your job starts, you should be put onto your allocated node. Node *gpu6* in this example. Now we can load the Singularity module:

```
gpu6:~$ module load singularity
```

Using the Tag you copied in the previous section, let’s build a container with Singularity. Because these are actually Docker built containers by default, what we are doing is converting them to a Singularity ready container. The Tag you copied will probably look like this: `docker pull nvcr.io/nvidia/tensorflow:21.12-tf2-py3`.

Let’s use what we need below to convert this now:

```
gpu6:~$ singularity build tensorflow-21.12-tf2-py3.simg docker://nvcr.io/nvidia/tensorflow:21.12-tf2-py3
```

*tensorflow-21.12-tf2-py3.simg* is the name of your new container. You can name this whatever you’d like, but we’ll keep the naming consistent. The above command may take several minutes.

### Testing TensorFlow
Lastly, we’ll test that the TensorFlow container works. We’ll do a simple query of the GPUs we allocated earlier. Remember, we only requested 2 GPUs:

```
gpu6:~$ singularity exec --nv tensorflow-21.12-tf2-py3.simg python -c "import tensorflow as tf; tf.test.gpu_device_name()"

2022-01-05 15:20:08.205124: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1510] Created device /device:GPU:0 with 38188 MB memory:  -> device: 0, name: NVIDIA A100-SXM4-40GB, pci bus id: 0000:07:00.0, compute capability: 8.0

2022-01-05 15:20:08.230207: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1510] Created device /device:GPU:1 with 38188 MB memory:  -> device: 1, name: NVIDIA A100-SXM4-40GB, pci bus id: 0000:4e:00.0, compute capability: 8.0
```

When you are finished testing, remember to relinquish your node allocation. For more information, including tutorials, please review the TensorFlow documentation on the NVIDIA website and well as the Singularity user guide.
