Where to build
--------------

You may compile (build) software on the Mahuika login nodes,
`login.mahuika.nesi.org.nz`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}.
Please be aware that these login nodes are limited and shared resources.
Please limit the amount of processes on these nodes. For example, use
`make -j 5`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
instead of
`make -j`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}.
If you require many CPU cores or long run times for your build process,
please request these resources through a batch job submitted to Slurm,
where you can also ask for a larger amount of compute resources to build
your code.

Compilers and Toolchains
------------------------

Compilers produced by three different vendors are provided on Mahuika:
Cray, GNU and Intel.

The GNU and Intel compilers can be accessed by loading one of the
toolchains:

-   `module load gimkl/2020a`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"} -
    the default toolchain, providing GNU compilers (version 9.2.0),
    Intel MPI and Intel MKL
-   `module load intel/2020a`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"} -
    Intel compilers (version 2020.0.166), Intel MPI and Intel MKL

A large number of dependencies are built against these toolchains, so
they are usually a good place to start when building your own software.
However, if for any reason you require a different version of one of
these compilers, you can load a compiler module directly, instead of
loading a toolchain. For example, the installed versions of the GNU
compilers can be listed with the following command:

`module spider GCC`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}

and version 7.4.0 loaded with the following command:

`module load GCC/7.4.0`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}

The Cray compilers behave differently to the GNU and Intel compilers,
since they are installed as a Cray Programming Environment, and have
some special features (see section Cray Programming Environment). The
Cray compilers are loaded with:

`module load PrgEnv-cray`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}

Third party applications
------------------------

Installation instructions vary from application to application, and we
suggest that you carefully read the instructions provided by the
developers of the software you plan to use. Nevertheless, the following
should give you an impression which steps you usually need to consider:

1.  Change into your desired source code directory. We suggest you use
    `/nesi/project/<projectID>`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"},
    or more typically one of its subdirectories. You may instead
    use `/nesi/nobackup/<projectID>`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"} (or
    one of its subdirectories) if you don\'t mind the software not being
    backed up and prone to automatic deletion in certain circumstances.
2.  Download the source code. This could be done via a repository
    checkout
    (`git clone <source-url>`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"})
    or via downloading a tarball
    (`wget <source-url>`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}).
3.  Ensure the tarball is not a tarbomb,
    using `tar tf <source.tar> | sort | less`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
    (`tar tzf ...`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
    if the source code is a gzipped tarball,
    `tar tjf ...`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
    if a bzip2 compressed tarball). If you find that the tarball is in
    fact a tarbomb, you will need to handle it using special techniques.
4.  Unpack the tarball using
    `tar xf <source.tar>`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}.
    Change into the source directory.
5.  ``{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}Load
    the preferred toolchain (or compiler module) and modules for any
    additional required libraries
    (`module load gimkl FFTW`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"})
6.  Run the configure script with appropriate options,
    e.g. `./configure --prefix=<desired install directory> --use-fftw=$EBROOTFFTW  `{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}(options
    can usually be listed using
    `./configure --help`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"})
7.  In some applications you need to adjust
    the `Makefile`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"} (generated
    by
    `configure`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"})
    to reflect your preferred compiler, and library options (see below)
8.  Compile the code
    (`make`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}`)`
9.  install the binaries and libraries into the specified directory
    (`make install`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"})``

 

Compilers
---------

Compilers are provided for Fortran, C, and C++. For MPI-parallelised
code, different compilers typically need to be used. The different
**compilers** are listed:

  ----------------------------------------------------------------------------
  Language   Cray                  Intel                 GNU
  ---------- --------------------- --------------------- ---------------------
  Fortran    ftn                   ifort                 gfortran

  Fortran +  ftn                   mpiifort              mpif90
  MPI                                                    

  C          cc                    icc                   gcc

  C + MPI    cc                    mpiicc                mpicc

  C++        CC                    icpc                  g++

  C++ + MPI  CC                    mpiicpc               mpicxx
  ----------------------------------------------------------------------------

**Note**, Cray uses compiler wrappers which are described [later in more
detail](#cray-programming-environment).

In general you then compile your code using:

`<compiler> <CompilerOptions> <source-file>`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}

e.g.

`ftn -O3 hello.f90`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}

 Compiler options
----------------

Compilers are controlled using different options to control
optimizations, output, source and library handling. There options vary
between the different compiler vendors. That means you will need to
change them if you decide to switch compilers. The following table
provides a list of commonly used compiler **options** for the different
compilers:

  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Group           Cray                                                                                                   Intel                                                                                      GNU                                                                                                      Notes
  --------------- ------------------------------------------------------------------------------------------------------ ------------------------------------------------------------------------------------------ -------------------------------------------------------------------------------------------------------- --------------------------------------------------------------------------
  Debugging       `-g`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"} or                          `-g`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"} or              `-g or -g{0,1,2,3}`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}                Set level of debugging information, some levels may disable certain
                  `-G{0,1,2,fast}`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}                 `-debug [keyword]`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}                                                                                                            compiler optimisations

  Light compiler  `-O2`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}                            `-O2`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}                `-O2`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}                               
  optimisation                                                                                                                                                                                                                                                                                                               

  Aggressive      `-O3 -hfp3`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}                      `-O3 -ipo`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}           `-O3 -ffast-math -funroll-loops`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}   This may affect numerical accuracy
  compiler                                                                                                                                                                                                                                                                                                                   
  optimisation                                                                                                                                                                                                                                                                                                               

  Architecture    Load this module first:                                                                                `-xHost`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}             `-march=native -mtune=native`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}      Build and compute nodes have the same architecture (Broadwell)
  specific        `module load craype-broadwell`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}                                                                                                                                                                                                       
  optimisation                                                                                                                                                                                                                                                                                                               

  Vectorisation   `-hlist=m`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}                       `-qopt-report`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}       `-fopt-info-vec`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"} or                 
  reports                                                                                                                                                                                                           `-fopt-info-missed`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}                

  OpenMP          `-homp`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"} (default)                `-qopenmp`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}           `-fopenmp`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}                         ``{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Additional compiler options are documented in the compiler man pages,
e.g.
`man mpicc`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"},
which are available *after* loading the related compiler module.
Additional documentation can be also found at the vendor web pages:

-   [Cray Fortran
    v8.7](https://pubs.cray.com/content/S-3901/8.7/cray-fortran-reference-manual/fortran-compiler-introduction),
    [Cray C and C++
    v8.7](https://pubs.cray.com/content/S-2179/8.7/cray-c-and-c++-reference-manual/invoke-the-c-and-c++-compilers)
-   [Intel Parallel Studio XE Cluster
    Edition](https://software.intel.com/en-us/node/685016) for Linux is
    installed on the Mahuika HPC Cluster, Mahuika Ancillary Nodes
-   [Intel Developer
    Guides](https://software.intel.com/en-us/documentation/view-all?search_api_views_fulltext=&current_page=0&value=78151,83039;20813,80605,79893,20812,20902;20816;20802;20804)
-   [GCC Manuals](https://gcc.gnu.org/onlinedocs/)

**Note**: Cray uses compiler wrappers. To list the compiler options,
please consult the man pages for the actual compiler, not the wrapper.

For example, the following commands would be used to compile with the
gfortran compiler, activate compiler warnings
(`-Wall`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}),
and requiring aggressive compiler optimisation
(`-O3`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}):

`module load gimkl/2018b`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}

`mpif90 -Wall -O3 -o simpleMpi simpleMpi.f90`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}

Linking {#compiler-options}
-------

Your application may depend on one or more external software packages,
normally libraries, and if so it will need to link against them when you
compile the program. In general, to link against an external package,
one must specify:

-   The location of the header files, using the option
    `-I/path/to/headers`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
-   The location of the compiled library or libraries, using
    `-L/path/to/lib/`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
-   The name of each library, typically without prefixes and suffixes.
    For example, if the full library file name is
    `libfoo.so.1.2.3`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
    (with aliases
    `libfoo.so.1`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
    and
    `libfoo.so`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}),
    the expected entry on the link line is
    `-lfoo`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}.

Thus the linker expects to find the include headers in the
*/path/to/headers* and the library at */path/to/lib/lib.so* (we assume
dynamic linking).

Note that you may need to list several libraries to link successfully,
e.g.,
`-lA -lB`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
for linking against libraries \"A\" and \"B\". The order in which you
list libraries matters, as the linker will go through the list in order
of appearance. If library \"A\" depends on library \"B\", specifying
`-lA -lB`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
will work. If library \"B\" depends on \"A\", use
`-lB -lA`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}.
If they depend on each other, use
`-lA -lB -lA`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
(although such cases are quite rare).

### [External Libraries]{.wysiwyg-underline}

There are already many libraries provided on the Mahuika platform. Most
of them are provided in modules. You can search them using

`module spider`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}

and look in the module description using:

`module help <module-name>`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}

Sometimes modules provide multiple libraries, e.g. *cray-libsci*.

Most libraries are provided using the EasyBuild software management
system, that NeSI/NIWA use to provide modules. Easybuild automatically
defines environment variables
`$EBROOT<library name in upper case>`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
when a module is loaded, which help pointing the compiler and linker to
include files and libraries as in the example above. Thus, you can keep
your Makefile independent of library versions, by defining e.g.
`-L$EBROOT<library name in upper case>/lib`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}.
Therewith you can use another version by only swapping modules. If you
are unsure which
`$EBROOT<...>`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
variables are available, use

`module show <module-name>`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}

to find out.

Note that specifying search paths with
`-I`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
and
`-L`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
is not strictly necessary in case of the GNU and Intel compilers, which
will use the contents of
`CPATH`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"},
`LIRARY_PATH`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"},
and
`LD_LIBRARY_PATH`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
provided by the NeSI/NIWA module. This will not work with the Cray
compiler.

**Important note:** Make sure that you load the correct variant of a
library, depending on your choice of compiler. Switching compiler
environment will *not* switch NeSI/NIWA modules automatically.
Furthermore, loading a NeSI/NIWA module may switch programming
environment if it was built with a different compiler. In general, the
used library should be build with the same compiler.

**Note:** the mentioned MPI compilers are practically compiler wrappers
adding the location to the MPI library. This can be observed calling
e.g.
`mpif90 -showme`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}

### [Common Linker Problems ]{.wysiwyg-underline}

Linking can easily go wrong. Most often, you will see linker errors
about \"missing symbols\" when the linker could not find a function used
in your program or in one of the libraries that you linked against. To
resolve this problem, have a closer look at the function names that the
linker reported:

-   Are you missing some object code files (these are compiled source
    files and have suffix
    `.o`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"})
    that should appear on the linker line? This can happen if the build
    system was not configured correctly or has a bug. Try running the
    linking step manually with all source files and debug the build
    system (which can be a lengthy and cumbersome process,
    unfortunately).
-   Do the missing functions have names that contain \"mp\" or \"omp\"?
    This could mean that some of your source files or external libraries
    were built with OpenMP support, which requires you to set an OpenMP
    flag
    (`-fopenmp`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
    for GNU compilers,
    `-qopenmp`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
    for Intel) in your linker command. For the Cray compilers, OpenMP is
    enabled by default and can be controlled using
    `-h[no]omp`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}.
-   Do you see a very long list of complex-looking function names, and
    does your source code or external library dependency include C++
    code? You may need to explicitly link against the C++ standard
    library
    (`-lstdc++`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
    for GNU and Cray compilers,
    `-cxxlib`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
    for Intel compilers); this is a particularly common problem for
    statically linked code.
-   Do the function names end with an underscore (\"\_\")? You might be
    missing some Fortran code, either from your own sources or from a
    library that was written in Fortran, or parts of your Fortran code
    were built with flags such as
    `-assume nounderscore`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
    (Intel) or
    `-fno-underscoring`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
    (GNU), while others were using different flags (note that the Cray
    compiler always uses underscores).
-   Do the function names end with double underscores (\"\_\_\")?
    Fortran compilers offer an option to add double underscores to
    Fortran subroutine names for compatibility reasons
    (`-h [no]second_underscore`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"},
    `-assume [no]2underscores`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"},
    `-f[no-]second-underscore`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"})
    which you may have to add or remove.
-   Compiler not necessarily enable preprocessing, which could result in
    `#ifndef VAR; Warning: Illegal preprocessor directive`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}.
    For example, using preprocessor directives in
    `.f`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
    files with gfortran requires the
    `-cpp`{style="font-family: Menlo, Consolas, 'DejaVu Sans Mono', monospace;"}
    option.

Note that the linker requires that function names match exactly, so any
variation in function name in your code will lead to a \"missing
symbols\" error (with the exception of character case in Fortran source
code).

Cray Programming Environment {#compiler-options}
----------------------------

The Cray Programming Environment includes the Cray compiler, various
libraries and tools. These work nicely together and provide certain
user-friendly features by using compiler wrappers. This works very
similar as the Cray XC environment, provided on Māui, and is described
in detail on the page [Building Code on
Māui](https://support.nesi.org.nz/hc/en-gb/articles/360000336076).

**Note:** on Māui the three compilers (Cray, Gnu and Intel) are provided
in this special environment, which provides support for both dynamic and
static linking. In contrast to that, on Mahuika only the Cray compiler
is provided in this environment. Furthermore, it only provides support
for dynamic linking.