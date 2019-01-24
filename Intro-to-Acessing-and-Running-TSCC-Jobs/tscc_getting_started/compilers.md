# TSCC Bootcamp: TSCC Compilers 
Note: Taken from  SDSC 2018 Webinar Series Topic: Introduction to Running Jobs on TSCC 

## <a name="top"></a>Compiling & Linking

## <a name="compilers"></a>Compiling & Linking

Comet provides the Intel, Portland Group (PGI), and GNU compilers along with multiple MPI implementations (MVAPICH2, MPICH2, OpenMPI). Most applications will achieve the best performance on Comet using the Intel compilers and MVAPICH2 and the majority of libraries installed on Comet have been built using this combination. 

Other compilers and versions can be installed by Comet staff on request. For more information, see the user guide:
http://www.sdsc.edu/support/user_guides/comet.html#compiling

### <a name="compilers-supported"></a>Supported Compiler Types

Comet compute nodes support several parallel programming models:
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

Intel MKL libraries are available as part of the "intel" modules on Comet. Once this module is loaded, the environment variable MKL_ROOT points to the location of the mkl libraries. The MKL link advisor can be used to ascertain the link line (change the MKL_ROOT aspect appropriately).

In the example below, we are working with the HPC examples that can be found in 
```
[mthomas@comet-14-01:~/comet-examples/PHYS244/MKL] pwd
/home/mthomas/comet-examples/PHYS244/MKL
[mthomas@comet-14-01:~/comet-examples/PHYS244/MKL] ls -al
total 25991
drwxr-xr-x  2 mthomas use300        9 Nov 25 17:20 .
drwxr-xr-x 16 mthomas use300       16 Aug  5 19:02 ..
-rw-r--r--  1 mthomas use300      325 Aug  5 19:02 compile.txt
-rw-r--r--  1 mthomas use300     6380 Aug  5 19:02 pdpttr.c
-rwxr-xr-x  1 mthomas use300 44825440 Nov 25 16:55 pdpttr.exe
-rw-r--r--  1 mthomas use300      188 Nov 25 16:57 scalapack.20294236.comet-07-27.out
-rw-r--r--  1 mthomas use300      376 Aug  5 19:02 scalapack.sb
```

The file `compile.txt` contains the full command to compile the `pdpttr.c` program statically linking 64 bit scalapack libraries on Comet:
```
[mthomas@comet-14-01:~/comet-examples/PHYS244/MKL] cat compile.txt 
mpicc -o pdpttr.exe pdpttr.c  -I$MKL_ROOT/include ${MKL_ROOT}/lib/intel64/libmkl_scalapack_lp64.a -Wl,--start-group ${MKL_ROOT}/lib/intel64/libmkl_intel_lp64.a ${MKL_ROOT}/lib/intel64/libmkl_core.a ${MKL_ROOT}/lib/intel64/libmkl_sequential.a -Wl,--end-group ${MKL_ROOT}/lib/intel64/libmkl_blacs_intelmpi_lp64.a -lpthread -lm```
```

Run the command:
```
[mthomas@comet-14-01:~/comet-examples/PHYS244/MKL] mpicc -o pdpttr.exe pdpttr.c  -I$MKL_ROOT/include ${MKL_ROOT}/lib/intel64/libmkl_scalapack_lp64.a -Wl,--start-group ${MKL_ROOT}/lib/intel64/libmkl_intel_lp64.a ${MKL_ROOT}/lib/intel64/libmkl_core.a ${MKL_ROOT}/lib/intel64/libmkl_sequential.a -Wl,--end-group ${MKL_ROOT}/lib/intel64/libmkl_blacs_intelmpi_lp64.a -lpthread -lm
```
For more information on the Intel compilers run: [ifort | icc | icpc] -help

[Back to Top](#top)
<hr>

### <a name="compilers-pgi"></a>Using the PGI Compilers
The PGI compilers can be loaded by executing the following commands at the Linux prompt or placing in your startup file (~/.cshrc or ~/.bashrc)

```
module purge
module load gnutools
module load pgi mvapich2_ib
```

For AVX support, compile with -fast

For more information on the PGI compilers: man [pgf90 | pgcc | pgCC]

| |Serial | MPI | OpenMP | MPI+OpenMP|
|---|---|---|---|---|
|pgf90 | mpif90 | pgf90 -mp | mpif90 -mp|
|C | pgcc | mpicc | pgcc -mp | mpicc -mp|
|C++ | pgCC | mpicxx | pgCC -mp | mpicxx -mp|

[Back to Top](#top)
<hr>

### <a name="compilers-gnu"></a>Using the GNU Compilers
The GNU compilers can be loaded by executing the following commands at the Linux prompt or placing in your startup files (~/.cshrc or ~/.bashrc)
```
module purge
module load gnutools
module load gnu openmpi_ib
```

For AVX support, compile with -mavx. Note that AVX support is only available in version 4.7 or later, so it is necessary to explicitly load the gnu/4.9.2 module until such time that it becomes the default.

For more information on the GNU compilers: man [gfortran | gcc | g++]

| |Serial | MPI | OpenMP | MPI+OpenMP|
|---|---|---|---|---|
|Fortran | gfortran | mpif90 | gfortran -fopenmp | mpif90 -fopenmp|
|C | gcc | mpicc | gcc -fopenmp | mpicc -fopenmp|
|C++ | g++ | mpicxx | g++ -fopenmp | mpicxx -fopenmp|


[Back to Top](#top)
<hr>

