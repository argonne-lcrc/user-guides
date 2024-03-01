# Software

## Lmod

LCRC uses Lmod (Lua Environment Modules) for software and environment variable management. Lmod has several advantages over other software environments. For example, it prevents you from loading multiple versions of the same package at the same time. It also prevents you from having multiple compilers and MPI libraries loaded at the same time. See the [Lmod User Guide](https://lmod.readthedocs.io/en/latest/010_user.html) for information on how to use Lmod. 

### Some basic commands.

#### Finding Available Modules

| Task | Lmod |
|------|------|
| List available modules for the current compiler/MPI library | `module avail` |
| List all available modules | `module spider` |
| List modules with a certain keyword in their description | `module spider <keyword>` |
| List currently loaded modules | `module list` |

#### Loading/Unloading Modules

|Task | Lmod |
|------|------|
| Load a module | `module load <module_name>` |
| Unload a module | `module unload <module_name>` |
| Reload all modules | `module update` |
| Unload all modules | `module purge` |

#### Setting Default Modules

| Task | Lmod |
|------|------|
| File containing default modules | `~/.lmod.d/default` |
| Save currently loaded modules | `module save` |
| Save currently loaded modules to a new collection | `module save <filename>` |
| Restore previously saved modules | `module restore` |
| Restore previously saved modules from another collection | `module restore <filename>` |
| Return to default modules | `module reset` |

#### Finding More Information on a Module

| Task | Lmod |
|------|------|
| Print info for a module | `module whatis` |
| Print description of a module | `module help` |
| See contents of a module | `module show <module_name>` |

## Available Software

With over 500 active users in fields as diverse as climate modeling, engine simulation, and ab-initio molecular dynamics, LCRC provides a diverse software stack. 

If you don’t see a software package installed in our environment that you would like to use, please let us know by contacting [support@lcrc.anl.gov](mailto:support@lcrc.anl.gov). We will consider adding software system wide if appropriate for general LCRC use.

We generally use a package manager called Spack for most of our software installs, but often need to install packages manually. Keep in mind that the software you want may have dozens of dependencies that also need to be installed, so it may take some time for us to complete the installation for you. Of course, you’re always welcome to install your own software in your home or project directory (if applicable) as well.

LCRC does not purchase licenses for users that need paid commercial software. If you need a paid application, please consult with LCRC staff before mkaing a purchase.
