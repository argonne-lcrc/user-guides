## Using Conda in LCRC 

Due to the increasing amount of python packages, we switched to Anaconda python distribution for python package management.

Anaconda modules that we provide include a common set of packages: pip, numpy, scipy, matplotlib, mpi4py, etc

If the Anaconda module does not have python package that you need already installed, you can create an Anaconda environment inside your home directory and install the needed packages there.

First, load the Anaconda module that you need:
```
$ module load anaconda/<version>
```

Now you can list the already installed packages by running:
```
$ conda list
```

If your package is not installed, you can search to see if it exists to install by running:
```
$ conda search <package_name>
```

If your package is not installed and appears in the available package list, you can create a new conda environment to install the package in. The following command will create an Anaconda environment in your home directory:
```
$ conda create -n <environment_name>
```

To activate that environment, run:
```
$ source activate <environment_name>
```

Now you are ready to install the package:
```
$ conda install <package_name>
```

The complete instructions on how to use Anaconda are at:
[https://conda.io/docs/index.html](https://conda.io/docs/index.html)

If the packages are not provided with pip or Anaconda and the procedure for installation is very complex, feel free to email to [support@lcrc.anl.gov](mailto:support@lcrc.anl.gov).
