# Using Conda in LCRC

Due to a change in licensing requirements, we will no longer provide modules or support for Anaconda. Instead, we provide Miniforge, which is a repository that holds the minimal installers for Conda and Mamba specific to conda-forge.

Conda modules that we provide include a common set of packages: pip, numpy, scipy, matplotlib, mpi4py, etc

If the Miniforge module does not have python package that you need already installed, you can create an Conda environment inside your home directory and install the needed packages there.

First, load the Miniforge module that you need:

```console
module load miniforge3/<version>
```

Now you can list the already installed packages by running:

```console
conda list
```

If your package is not installed, you can search to see if it exists to install by running:

```console
conda search <package_name>
```

If your package is not installed and appears in the available package list, you can create a new conda environment to install the package in. The following command will create an Conda environment in your home directory:

```console
conda create -n <environment_name>
```

To activate that environment, run:

```conosle
source activate <environment_name>
```

Now you are ready to install the package:

```console
conda install <package_name>
```

The complete instructions on how to use Conda are at:
[https://conda.io/docs/index.html](https://conda.io/docs/index.html)

If the packages are not provided with pip or Conda and the procedure for installation is very complex, feel free to email to [support@lcrc.anl.gov](mailto:support@lcrc.anl.gov).
