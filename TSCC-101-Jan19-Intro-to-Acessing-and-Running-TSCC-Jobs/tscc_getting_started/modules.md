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


Once you have loaded the modules, you can check the system variables that are available for you to use by running common unix commands:
* To see all variable, run the <b>`env`</b> command. Typically, you will see more than 60 lines containing information such as your login name, shell, your home directory, and other variables
* Look at what is in your path:   <b>`echo $PATH`</b>
* run command <b>`which mpicc`</b> to see location of the compiler command.


[Back to Top](#top)
<hr>

