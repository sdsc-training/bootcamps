# TSCC Bootcamp: Using Modules
Note: Taken from  SDSC 2018 Webinar Series Topic: Introduction to Running Jobs on TSCC 

## <a name="modules"></a>Modules: Customizing Your User Environment
The Environment Modules package provides for dynamic modification of your shell environment. Module commands set, change, or delete environment variables, typically in support of a particular application. They also let the user choose between different versions of the same software or different combinations of related codes. See:
https://www.sdsc.edu/support/user_guides/tscc.html#env-modules

<b> Common module commands:</b>
   
   Here are some common module commands and their descriptions:

| Command | Description | 
|---|---|
| module list | List the modules that are currently loaded| 
| module avail | List the modules that are available| 
| module display <module_name> | Show the environment variables used by <module name> and how they are affected| 
| module show  <module_name>  | Same as display| 
| module unload <module name> | Remove <module name> from the environment| 
| module load <module name> | Load <module name> into the environment| 
| module swap <module one> <module two> | Replace <module one> with <module two> in the environment| 
   
<b> A few module commands:</b>

* Default environment: `list`, `li`
```
$ module li
Currently Loaded Module files:
  1) intel/2013_sp1.2.144   2) mvapich2_ib/2.1        3) gnutools/2.69
```
* List available modules:  `available`, `avail`, `av`

```
$ module av
------------------------- /opt/modulefiles/mpi/.intel --------------------------
intelmpi/2016.3.210(default) mvapich2_ib/2.1(default)
mvapich2_gdr/2.1(default)    openmpi_ib/1.8.4(default)
mvapich2_gdr/2.2
--------------------- /opt/modulefiles/applications/.intel ---------------------
atlas/3.10.2(default)     lapack/3.6.0(default)     scalapack/2.0.2(default)
boost/1.55.0(default)     mxml/2.9(default)         slepc/3.6.2(default)
. . .
```

* Load modules:
```
$ module load fftw/3.3.4 
$ module li
Currently Loaded Modulefiles:
  1) intel/2013_sp1.2.144   3) gnutools/2.69
  2) mvapich2_ib/2.1        4) fftw/3.3.4
```
* Show what a module does:
```
$ module show fftw/3.3.4
-------------------------------------------------------------------
/opt/modulefiles/applications/.intel/fftw/3.3.4:
module-whatis fftw 
module-whatis Version: 3.3.4 
module-whatis Description: fftw 
module-whatis Compiler: intel 
module-whatis MPI Flavors: mvapich2_ib openmpi_ib 
setenv FFTWHOME /opt/fftw/3.3.4/intel/mvapich2_ib 
prepend-path PATH /opt/fftw/3.3.4/intel/mvapich2_ib/bin 
prepend-path LD_LIBRARY_PATH /opt/fftw/3.3.4/intel/mvapich2_ib/lib 
prepend-path LIBPATH /opt/fftw/3.3.4/intel/mvapich2_ib/lib 
-------------------------------------------------------------------
```

[Back to Top](#top)
<hr>

Once you have loaded the modules, you can check the system variables that are available for you to use.
* To see all variable, run the <b>`env`</b> command. Typically, you will see more than 60 lines containing information such as your login name, shell, your home directory:
```
[username@comet-ln3 IBRUN]$ env
HOSTNAME=comet-ln3.sdsc.edu
INTEL_LICENSE_FILE=/opt/intel/composer_xe_2013_sp1.2.144/licenses:/opt/intel/licenses:/root/intel/licenses
TERM=xterm-256color
SHELL=/bin/bash
GDBSERVER_MIC=/opt/intel/composer_xe_2013_sp1.2.144/debugger/gdb/target/mic/bin/gdbserver
LOADEDMODULES=intel/2013_sp1.2.144:mvapich2_ib/2.1:gnutools/2.69
HOME=/home/username
MPIHOME=/opt/mvapich2/intel/ib
SDSCHOME=/opt/sdsc
PYTHONPATH=/opt/sdsc/lib:/opt/sdsc/lib/python2.6/site-packages
LOGNAME=username
SSH_CONNECTION=76.176.117.51 58589 198.202.113.252 22
MODULESHOME=/usr/share/Modules
INCLUDE=/opt/intel/composer_xe_2013_sp1.2.144/mkl/include
INTELHOME=/opt/intel/composer_xe_2013_sp1.2.144
```

To see the value for any of these variables, use the `echo` command:
```
[username@comet-ln3 IBRUN]$ echo $PATH
PATH=/opt/gnu/gcc/bin:/opt/gnu/bin:/opt/mvapich2/intel/ib/bin:/opt/intel/composer_xe_2013_sp1.2.144/bin/intel64:/opt/intel/composer_xe_2013_sp1.2.144/mpirt/bin/intel64:/opt/intel/composer_xe_2013_sp1.2.144/debugger/gdb/intel64_mic/bin:/usr/lib64/qt-3.3/bin:/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/sbin:/opt/ibutils/bin:/usr/java/latest/bin:/opt/pdsh/bin:/opt/rocks/bin:/opt/rocks/sbin:/opt/sdsc/bin:/opt/sdsc/sbin:/home/username/bin
```
[Back to Top](#top)
<hr>

## <a name="compilers"></a>Compiling & Linking

TSCC provides the Intel, Portland Group (PGI), and GNU compilers along with multiple MPI implementations (MVAPICH2, MPICH2, OpenMPI). Most applications will achieve the best performance on TSCC using the Intel compilers and MVAPICH2 and the majority of libraries installed on TSCC have been built using this combination. 

Other compilers and versions can be installed by TSCC staff on request. For more information, see the user guide:
http://www.sdsc.edu/support/user_guides/comet.html#compiling

### <a name="compilers-supported"></a>Supported Compiler Types

TSCC compute nodes support several parallel programming models:
* __MPI__: Default: Intel
   * Default Intel Compiler: intel/2013_sp1.2.144; Versions 2015.2.164 and 2016.3.210 available.
   * Other options: openmpi_ib/1.8.4 (and 1.10.2), Intel MPI, mvapich2_ib/2.1
   * mvapich2_gdr: GPU direct enabled version
* __OpenMP__: All compilers (GNU, Intel, PGI) have OpenMP flags.
* __GPU nodes__: support CUDA, OpenACC.
* __Hybrid modes__ are possible.

In this tutorial, we include several hands-on examples that cover many of the cases in the table:

* MPI
* OpenMP
* HYBRID
* GPU
* Local scratch
    
Default/Suggested Compilers to used based on programming model and languages:

| |Serial | MPI | OpenMP | MPI+OpenMP|
|---|---|---|---|---|
| Fortran | ifort | mpif90 | ifort -openmp | mpif90 -openmp |
| C | icc | mpicc | icc -openmp | mpicc -openmp |
| C++ | icpc | mpicxx | icpc -openmp | mpicxx -openmp |

[Back to Top](#top)
<hr>

### <a name="compilers-intel"></a>Using the Intel Compilers:

The Intel compilers and the MVAPICH2 MPI implementation will be loaded by default. If you have modified your environment, you can reload by executing the following commands at the Linux prompt or placing in your startup file (~/.cshrc or ~/.bashrc)
```
module purge
module load gnutools
module load intel mvapich2_ib
```
For AVX2 support, compile with the -xHOST option. Note that -xHOST alone does not enable aggressive optimization, so compilation with -O3 is also suggested. The -fast flag invokes -xHOST, but should be avoided since it also turns on interprocedural optimization (-ipo), which may cause problems in some instances.

Intel MKL libraries are available as part of the "intel" modules on TSCC. Once this module is loaded, the environment variable MKL_ROOT points to the location of the mkl libraries. The MKL link advisor can be used to ascertain the link line (change the MKL_ROOT aspect appropriately).



[Back to Top](#top)
<hr>
