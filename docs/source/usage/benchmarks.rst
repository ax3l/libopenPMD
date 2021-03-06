.. _usage-benchmark:

Benchmarks
==========

Parallel benchmarks 8a & 8b
---------------------------

The following examples show parallel reading and writing of domain-decomposed data with MPI.

The `Message Passing Interface (MPI) <https://www.mpi-forum.org/>`_ is an open communication standard for scientific computing.
MPI is used on clusters, e.g. large-scale supercomputers, to communicate between nodes and provides parallel I/O primitives.

Writing
^^^^^^^

*Source*: ``examples/8a_benchmark_write_parallel.cpp``

This benchmark writes a few meshes and particles, either 1D, 2D or 3D.

The meshes are viewed as grid of mini blocks.
As an example, we assume the mini blocks dimension are [16, 32, 32].


Next we define the grid based on the mini block.
say, [32, 32, 16]. Then our actual mesh size is [16x32, 32x32, 32x16].


Here is a sample input file ("w.input"):

::

   dim=3
   balanced=true
   ratio=1
   steps=10
   minBlock=16 32 32
   grid=32 32 16

With the above input file,  we will create an openPMD file with the above mesh using

* 3D data
* balanced load
* particle to mesh ratio = 1
* 10 iteration steps


*Note: All files generated are group based. i.e. One file per iteration.*

To run:

./8a_benchmark_write_parallel w.input

then the file generated are:  ../samples/8a_parallel_3Db_*


Reading
^^^^^^^

**Source**: ``examples/8b_benchmark_read_parallel.cpp``

This benchmark is to read from the files written by 8a.

The options are: a file prefix, and a read pattern


For example, if the files are in the format of ``/path/8a_parallel_3Db_%07T.bp``
the input will be: ``/path/8a_parallel_3Db <options>``

The Read options intent to measure overall processing time in the following categories:

* Metadata only  (option = m)
* or data retrieval (after metadata loaded)

The data retrieval is furthur divided into:

* slice the "rho" mesh
    (options = ``sx/sy/sz`` depends on which direction. e.g. ``sx`` implies ``x=0``.)
* slice on the 3D magnetic field(e.g. find values for "Bx", "By" and "Bz")
    (options = ``fx/fy/fz`` depends on which direction. e.g. ``fx`` implies ``x=0``.)

So here are the options one can use to read a file:

* m
* sx
* sy
* sz
* fx
* fy
* fz

For example, To read files generated by the above write commmand,  metadata only:

./8b_benchmark_read_parallel  ../samples/8a_parallel_3Db m

Benchmark Utilities
-------------------

Further benchmarks are fund in :ref:`utilities <utilities-benchmark>`.
