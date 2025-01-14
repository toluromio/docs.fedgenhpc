**Using Pre-installed Applications**
---------------------------------------

Applications on FEDGEN HPC Cluster are made available through Miniconda
and Easybuild package managers. Software packages have to be activated
or deactivated through ‘Modules’ before program execution.

**Environment Variables**

The program environment on the HPC is controlled by pre-defined
settings, which are stored in environment (or shell) variables. All the
software packages that are installed on the HPC cluster require
different settings. These packages include compilers, interpreters,
scientific software as well as other applications and libraries.

**The module command**

In order to administer the active software and their environment
variables, the module system:

1. Activates or deactivates *software packages* and their dependencies.

2. Allows setting and unsetting of *environment variables*, including
   adding and deleting entries from list-like environment variables.

3. Does this in a *shell-independent* fashion (necessary information is
   stored in the accompanying module file).

4. Takes care of *versioning aspects*: For many libraries, multiple
   versions are installed and maintained. The module system also takes
   care of the versioning of software packages. For instance, it does
   not allow multiple versions to be loaded at same time.

5. Takes care of *dependencies*: Another issue arises when one considers
   library versions and the dependencies they require. Some software
   requires an older version of a particular library to run correctly
   (or at all). Hence a variety of version numbers is available for
   important libraries. Modules typically load the required dependencies
   automatically.

The main options for the module command are:

module avail **\|** av list available software (modules)

module load **\|** add [module] set up the environment to use the
software

module list list currently loaded software

module purge clears the environment

module spider list all possible modules

module show show the commands **in** the module file

module help get help

**Available modules**

A large number of software packages are preinstalled on the HPC cluster.
A list of all currently available software can be obtained by typing:

module available

There is also a shorter ml command that does exactly the same as
the module command. Whenever you see a module command, you can
replace module with ml..

This will give some output such as:

module avail

--- /apps/gent/RHEL8/zen2-ib/modules/all ---

ABAQUS/2021-hotfix-2132

ABAQUS/2022-hotfix-2214

ABAQUS/2022

ABAQUS/2023

ABAQUS/2024-hotfix-2405 (D)

...

Or when you want to check whether some specific software, some compiler
or some application (e.g., MATLAB) is installed on the HPC.

module avail matlab

--- /apps/gent/RHEL8/zen2-ib/modules/all ---

LIBSVM-MATLAB/3.30-GCCcore-11.3.0-MATLAB-2022b-r5

MATLAB/2019b

MATLAB/2021b

MATLAB/2022b-r5 (D)

SPM/12.5_r7771-MATLAB-2021b

This gives a full list of software packages that can be loaded.

*The casing of module names is important*: lowercase and uppercase
letters matter in module names.

**Organisation of modules in toolchains**

The amount of modules on the system can be overwhelming, and it is not
always immediately clear which modules can be loaded safely together if
you need to combine multiple programs in a single job to get your work
done.

Therefore the system has defined so-called **toolchains**. A toolchain
contains a C/C++ and Fortran compiler, a MPI library and some basic math
libraries for (dense matrix) linear algebra and FFT. Two toolchains are
defined; One, the intel toolchain, consists of the Intel compilers, MPI
library and math libraries. The other one, the foss toolchain, consists
of Open Source components: the GNU compilers, OpenMPI, OpenBLAS and the
standard LAPACK and ScaLAPACK libraries for the linear algebra
operations and the FFTW library for FFT.

The toolchains are then used to compile a lot of the software installed
on the FEDGEN HPC cluster. You can recognise those packages easily as
they all contain the name of the toolchain after the version number in
their name (e.g., Python/3.12.3-GCCcore-13.3.0). Only packages compiled
with the same toolchain name and version can work together without
conflicts.

**Loading and unloading modules**

**module load**

To "activate" a software package, you load the corresponding module file
using the module load command:

module load example

This will load the most recent version of *example*.

For some packages, multiple versions are installed; the load command
will automatically choose the default version (if it was set by the
system administrators) or the most recent version otherwise (i.e., the
lexicographical last after the /).

Assuming, module available openmpi returns the following OpenMPI
modules;

OpenMPI\ **/**\ 2.1\ **.**\ 1\ **-**\ GCC\ **-**\ 6.4\ **.**\ 0\ **-**\ 2.28

OpenMPI\ **/**\ 2.1\ **.**\ 1\ **-**\ iccifort\ **-**\ 2017.4\ **.**\ 196\ **-**\ GCC\ **-**\ 6.4\ **.**\ 0\ **-**\ 2.28

OpenMPI\ **/**\ 3.1\ **.**\ 1\ **-**\ GCC\ **-**\ 7.3\ **.**\ 0\ **-**\ 2.30

then with the command

module load
OpenMPI\ **/**\ 2.1\ **.**\ 1\ **-**\ GCC\ **-**\ 6.4\ **.**\ 0\ **-**\ 2.28

you will enable OpenMPI version 2.1.1 compiled with GCC version 6.4.0.
The naming convention for the available modules is always of the
form software/version-toolchain (more on the toolchain part below).

After doing this, when you run e.g. mpicc or mpirun without specifying
the full path, you will be running that specific version of OpenMPI
compilers or launch script.

The ml command is a shorthand for module load: ml example/1.2.3 is
equivalent to module load example/1.2.3.

Modules need not be loaded one by one; the two module load commands can
be combined as follows:

module load example/1.2.3 secondexample/4.5.6-intel-2023a

This will load the two modules as well as their dependencies (unless
there are conflicts between both modules).

**module list**

Obviously, you need to be able to keep track of the modules that are
currently loaded. Assuming you have run the module load commands stated
above, you will get the following:

$ module list

Currently Loaded Modules:

1) env/vsc/<cluster> (S) 7) binutils/2.40-GCCcore-12.3.0 13) iimpi/2023a

2) env/slurm/<cluster> (S) 8) intel-compilers/2023.1.0 14)
imkl-FFTW/2023.1.0-iimpi-2023a

3) env/software/<cluster> (S) 9) numactl/2.0.16-GCCcore-12.3.0 15)
intel/2023a

4) cluster/<cluster> (S) 10) UCX/1.14.1-GCCcore-12.3.0 16)
secondexample/4.5.6-intel-2023a

5) GCCcore/12.3.0 11) impi/2021.9.0-intel-compilers-2023.1.0 17)
example/1.2.3

6) zlib/1.2.13-GCCcore-12.3.0 12) imkl/2023.1.0

Where:

S: Module is Sticky, requires --force to unload or purge

You can also just use the ml command without arguments to list loaded
modules.

It is important to note at this point that other modules
(e.g., intel/2023a) are also listed, although the user did not
explicitly load them. This is
because secondexample/4.5.6-intel-2023a depends on it (as indicated in
its name), and the system administrator specified that
the intel/2023a module should be loaded
whenever *this* secondexample module is loaded. There are advantages and
disadvantages to this, so be aware of automatically loaded modules
whenever things go wrong: they may have something to do with it!

**module unload**

To unload a module, one can use the module unload command. It works
consistently with the load command, and reverses the latter's effect.
However, the dependencies of the package are NOT automatically unloaded;
you will have to unload the packages one by one. When the example module
is unloaded, only the following modules remain:

$ module unload example

$ module list

Currently Loaded Modules:

1) env/vsc/<cluster> (S) 7) binutils/2.40-GCCcore-12.3.0 13) iimpi/2023a

2) env/slurm/<cluster> (S) 8) intel-compilers/2023.1.0 14)
imkl-FFTW/2023.1.0-iimpi-2023a

3) env/software/<cluster> (S) 9) numactl/2.0.16-GCCcore-12.3.0 15)
intel/2023a

4) cluster/<cluster> (S) 10) UCX/1.14.1-GCCcore-12.3.0 16)
secondexample/4.5.6-intel-2023a

5) GCCcore/12.3.0 11) impi/2021.9.0-intel-compilers-2023.1.0

6) zlib/1.2.13-GCCcore-12.3.0 12) imkl/2023.1.0

Where:

S: Module is Sticky, requires --force to unload or purge

To unload the example module, you can also use ml -example.

Notice that the version was not specified: there can only be one version
of a module loaded at a time, so unloading modules by name is not
ambiguous. However, checking the list of currently loaded modules is
always a good idea, since unloading a module that is currently not
loaded will *not* result in an error.

**Purging all modules**

In order to unload all modules at once, and hence be sure to start in a
clean state, you can use:

module purge

Using explicit version numbers

Once a module has been installed on the cluster, the executables or
libraries it comprises are never modified. This policy ensures that the
user's programs will run consistently, at least if the user specifies a
specific version. **Failing to specify a version may result in
unexpected behaviour.**

Consider the following example: the user decides to use
the example module and at that point in time, just a single version
1.2.3 is installed on the cluster. The user loads the module using:

module load example

rather than

module load example/1.2.3

Everything works fine, up to the point where a new version of example is
installed, 4.5.6. From then on, the user's load command will load the
latter version, rather than the intended one, which may lead to
unexpected problems.

Consider the following example modules:

$ module avail example/

example/1.2.3

example/4.5.6

Let's now generate a version conflict with the example module, and see
what happens.

$ module load example/1.2.3 example/4.5.6

Lmod has detected the following error: A different version of the
'example' module is already loaded (see output of 'ml').

$ module swap example/4.5.6

Note: A module swap command combines the appropriate module
unload and module load commands.

**Search for modules**

With the module spider command, you can search for modules:

$ module spider example

--------------------------------------------------------------------------------

example:

--------------------------------------------------------------------------------

Description:

This is just an example

Versions:

example/1.2.3

example/4.5.6

--------------------------------------------------------------------------------

For detailed information about a specific "example" module (including
how to load the modules) use the module's full name.

For example:

module spider example/1.2.3

--------------------------------------------------------------------------------

**Save and load collections of modules**

If you have a set of modules that you need to load often, you can save
these in a *collection*. This will enable you to load all the modules
you need with a single command.

In each module command shown below, you can replace module with ml.

First, load all modules you want to include in the collections:

module load example/1.2.3 secondexample/4.5.6-intel-2023a

Now store it in a collection using module save. In this example, the
collection is named my-collection.

module save my-collection

Later, for example in a jobscript or a new session, you can load all
these modules with module restore:

module restore my-collection

You can get a list of all your saved collections with the module
savelist command:

$ module savelist

Named collection list (For LMOD_SYSTEM_NAME =
"<OS>-<CPU-ARCHITECTURE>"):

1) my-collection

To get a list of all modules a collection will load, you can use
the module describe command:

$ module describe my-collection

Currently Loaded Modules:

1) env/vsc/<cluster> (S) 7) binutils/2.40-GCCcore-12.3.0 13) iimpi/2023a

2) env/slurm/<cluster> (S) 8) intel-compilers/2023.1.0 14)
imkl-FFTW/2023.1.0-iimpi-2023a

3) env/software/<cluster> (S) 9) numactl/2.0.16-GCCcore-12.3.0 15)
intel/2023a

4) cluster/<cluster> (S) 10) UCX/1.14.1-GCCcore-12.3.0 16)
secondexample/4.5.6-intel-2023a

5) GCCcore/12.3.0 11) impi/2021.9.0-intel-compilers-2023.1.0 17)
example/1.2.3

6) zlib/1.2.13-GCCcore-12.3.0 12) imkl/2023.1.0

To remove a collection, remove the corresponding file in $HOME/.lmod.d/:

rm $HOME/.lmod.d/my-collection

**Getting module details**

To see how a module would change the environment, you can use the module
show command:

$ module show Python-bundle-PyPI/2024.06-GCCcore-13.3.0

help([[

Description

===========

Bundle of Python packages from PyPI

...

Included extensions

===================

alabaster-0.7.16, appdirs-1.4.4, asn1crypto-1.5.1, atomicwrites-1.4.1,

...

wcwidth-0.2.13, webencodings-0.5.1, xlrd-2.0.1, zipfile36-0.1.3,
zipp-3.19.2

]])

...

load("GCCcore/13.3.0")

load("Python/3.12.3-GCCcore-13.3.0")

load("cryptography/42.0.8-GCCcore-13.3.0")

load("virtualenv/20.26.2-GCCcore-13.3.0")

...

Here you can see that
the Python-bundle-PyPI/2024.06-GCCcore-13.3.0 comes with a lot of
extensions: alabaster, appdirs, ... These are Python packages which can
be used in your Python scripts.

You can also see the modules
the Python-bundle-PyPI/2024.06-GCCcore-13.3.0 module
loads: GCCcore/13.3.0, Python/3.12.3-GCCcore-13.3.0, ...

If you're not sure what all of this means: don't worry, you don't have
to know; just load the module and try to use the software.
