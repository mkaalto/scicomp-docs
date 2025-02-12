================
Software modules
================

.. admonition:: Video

   Watch this in our courses: `2021 January <https://www.youtube.com/watch?v=dOWBGBdqT4g&list=PLZLVmS9rf3nN_tMPgqoUQac9bTjZw8JYc&index=9>`__

There are hundreds of people using every cluster.  They all have different
software needs, including conflicting versions required for different
projects!  How do we handle this without making a mess, or one person
breaking the cluster for everyone?

This is actually a very hard, but solved within certain parameters,
problem.  Software installation and management takes up a huge amount
of our time, but we try to make it easy for our users.  Still, it can
end up taking a lot of your effort as well.

.. admonition:: Abstract

   * We use the standard `Lmod module system
     <https://lmod.readthedocs.io/>`__, which makes more software
     available by adjusting environment variables like ``PATH``
   * See the :doc:`../ref/index` for a ``module`` command cheatsheet.

.. admonition:: Local differences

   Almost every site uses modules, and most use the same Lmod system
   we use here.  But, the exact module names you can load will be
   different.



Introduction to modules
-----------------------

The answer is the standard "module" system `Lmod <https://lmod.readthedocs.io/en/latest/>`__.
It allows us to
have unlimited number of different software packages installed, and the
user can select what they want.  Modules include everything from
compilers (+their required development files), libraries, and programs.
If you need a program installed, we will put it in the module system.

In a system the size of Triton, it just isn't possible to install all
software by default for every user.

A module lets you adjust what software is available,
and makes it easy to switch between different versions.

As an example, let's inspect the ``gcc`` module (abbreviated output
shown) with ``module show gcc``:

.. highlight:: console

.. code-block:: console

    $ module show gcc
    ----------------------------------------------------------------------------------------------
       /share/apps/spack/envs/fgci-centos7-generic/lmod/linux-centos7-x86_64/all/gcc/9.2.0.lua:
    ----------------------------------------------------------------------------------------------
    whatis("Name : gcc")
    whatis("Version : 9.2.0")
    whatis("Short description : The GNU Compiler Collection includes front ends for C, C++, Objective-C, Fortran, Ada, and Go, as well as libraries for these languages.")
    whatis("Configure options : --disable-multilib --enable-languages=c,c++,fortran --with-mpfr=/share/apps/spack/envs/fgci-centos7-generic/software/mpfr/3.1.6/m6xmzws --with-gmp=/share/apps/spack/envs/fgci-centos7-generic/software/gmp/6.1.2/mnsg5g2 --enable-lto --with-quad --with-system-zlib --with-mpc=/share/apps/spack/envs/fgci-centos7-generic/software/mpc/1.1.0/uaijipe --with-isl=/share/apps/spack/envs/fgci-centos7-generic/software/isl/0.19/indu5p6")
    help([[The GNU Compiler Collection includes front ends for C, C++, Objective-C,
    Fortran, Ada, and Go, as well as libraries for these languages.]])
    family("compiler")
    prepend_path("PATH","/share/apps/spack/envs/fgci-centos7-generic/software/gcc/9.2.0/dnrscms/bin")
    prepend_path("MANPATH","/share/apps/spack/envs/fgci-centos7-generic/software/gcc/9.2.0/dnrscms/share/man")
    prepend_path("LIBRARY_PATH","/share/apps/spack/envs/fgci-centos7-generic/software/gcc/9.2.0/dnrscms/lib")
    prepend_path("LD_LIBRARY_PATH","/share/apps/spack/envs/fgci-centos7-generic/software/gcc/9.2.0/dnrscms/lib")
    prepend_path("LIBRARY_PATH","/share/apps/spack/envs/fgci-centos7-generic/software/gcc/9.2.0/dnrscms/lib64")
    prepend_path("LD_LIBRARY_PATH","/share/apps/spack/envs/fgci-centos7-generic/software/gcc/9.2.0/dnrscms/lib64")
    prepend_path("CPATH","/share/apps/spack/envs/fgci-centos7-generic/software/gcc/9.2.0/dnrscms/include")
    prepend_path("CMAKE_PREFIX_PATH","/share/apps/spack/envs/fgci-centos7-generic/software/gcc/9.2.0/dnrscms/")
    setenv("CC","/share/apps/spack/envs/fgci-centos7-generic/software/gcc/9.2.0/dnrscms/bin/gcc")
    setenv("CXX","/share/apps/spack/envs/fgci-centos7-generic/software/gcc/9.2.0/dnrscms/bin/g++")
    ...

The command ``module show gcc`` shows some meta-info (name of the module, its version, etc.)
When you load this module, it adjusts various environment paths (as
you see there),
so that when you type ``gcc`` it runs the program from
``/share/apps/spack/envs/fgci-centos7-generic/software/gcc/9.2.0/dnrscms/bin/gcc``.
This is almost magic: we can have many versions of any software installed,
and everyone can pick what they want, with no conflicts.



Loading modules
---------------

Let's dive right into an example and load a module.

.. admonition:: Local differences

   If you are not at Aalto, but in Finland (but not at CSC),
   then you need to run ``module load
   fgci-common`` first, before any of the other commands will work
   (and you will need to *keep doing this* for every other tutorial in
   this series).  You have to do this *every time you start a new shell*.
   If you are at CSC or not in Finland, the concepts here also apply to
   you, but the actual names of the modules loaded may differ.

Let's assume you've written a Python script that is only compatible with Python version 3.7.0 or higher.
You open a shell to find out where and what version our Python is. The
**which** program looks up the current detected version of a program -
very useful when testing modules.::

  $ which python3
  /usr/bin/python3
  $ python3 -V
  Python 3.6.8

But you need a newer version of Python.  To this end, you can **load** the ``anaconda`` module
using the ``module load anaconda`` command,
that has a more up to date Python with lots of libraries already included::

  $ module load anaconda
  $ which python
  /share/apps/anaconda-ci/fgci-centos7-generic/software/anaconda/2020-03-tf2/521551bc/bin/python
  $ python -V
  Python 3.6.10 :: Anaconda, Inc.

As you see, you now have a newer version of Python, in a different directory.

You can see a list of the all the loaded modules in our working shell using the ``module list`` command::

  $ module list
  Currently Loaded Modules:
    1) anaconda/2020-03-tf2

.. note::
  The ``module load`` and ``module list`` commands can be abbreviated as ``ml``


Let's  use the ``module purge`` command to **unload** all the loaded modules (``anaconda`` in this case)::

  $ module purge

Or explicitly unload the ``anaconda`` module by using the ``module unload anaconda`` command::

  $ module unload anaconda

You can load any number of modules in your open shell,
your scripts, etc.  You could load modules in your
``~/.bash_profile``, but then it will always automatically load it -
perhaps even if you don't expect it.  Watch out for this if you get
un-explainable bugs - it may be best to explicitly load what you
need.



Type-along: Where is Matlab?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Let's say you want to use Matlab.  You log in and try in the shell::

  $ matlab
  -bash: matlab: command not found

So first search for it using the ``module spider`` command::

  $ module spider matlab

  ----------------------------------------------------------------------------
    matlab:
  ----------------------------------------------------------------------------
       Versions:
	  matlab/r2012a
	  matlab/r2014a
	  matlab/r2015b
	  matlab/r2016a
	  matlab/r2016b
	  matlab/r2017b
	  matlab/r2018a
	  matlab/r2018b
	  matlab/r2019a
	  matlab/r2019b


We see there are a lot of versions available.

Load the latest version of Matlab as::

  $ module load matlab

It never hurts to double check the version and in fact is recommended. So let's do just that::

  $ module list
  Currently Loaded Modules:
    1) matlab/r2019b



Type-along: Where is R?
^^^^^^^^^^^^^^^^^^^^^^^
If you don't specify the version - just as the above Matlab example - the default version of the module is usually loaded, which is usually the latest version.
The default version, however,is not always latest version.  To see an example, let's see what versions of R are available::

  $ module spider r

  ----------------------------------------------------------------------------
    r:
  ----------------------------------------------------------------------------
       Versions:
	  r/3.4.3-python-2.7.14
	  r/3.4.3-python2
	  r/3.4.3-python3
	  r/3.5.0-python-2.7.14
	  r/3.5.0-python2
	  r/3.5.3-python-2.7.14
	  r/3.6.1-python3

Let's try loading the default version::

  $ module load r

You can list all the dependencies the R module requires and loads::

  $ module list
  Currently Loaded Modules:
    1) pcre/8.42        12) libpthread-stubs/0.4  23) libxml2/2.9.9        34) jdk/8u181-b13          45) libice/1.0.9
    2) ncurses/6.1      13) xproto/7.0.31         24) font-util/1.3.2      35) fontconfig/2.12.3      46) libx11/1.6.7
    3) zlib/1.2.11      14) libxau/1.0.8          25) libxft/2.3.2         36) pixman/0.34.0          47) libsm/1.2.2
    4) readline/7.0     15) libxcb/1.13           26) tk/8.6.8             37) cairo/1.14.12-python2  48) libxt/1.1.5
    5) sqlite/3.23.1    16) libxext/1.3.3         27) python/2.7.15        38) libjpeg-turbo/1.5.90   49) harfbuzz/1.4.6-python2
    6) openssl/1.0.2k   17) libxscrnsaver/1.2.2   28) tar/1.31             39) libtiff/4.0.9          50) gobject-introspection/1.49.2-python2
    7) tcl/8.6.8        18) libpng/1.6.34         29) gettext/0.19.8.1     40) bzip2/1.0.6            51) pango/1.41.0-python2
    8) kbproto/1.0.7    19) renderproto/0.11.1    30) gdbm/1.18.1          41) freetype/2.7.1         52) openblas/0.3.2
    9) xextproto/7.3.0  20) libxrender/0.9.10     31) perl/5.26.2          42) libssh2/1.8.0          53) r/3.4.3-python2
   10) libbsd/0.9.1     21) libiconv/1.15         32) libffi/3.2.1         43) curl/7.60.0
   11) libxdmcp/1.1.2   22) xz/5.2.4              33) glib/2.56.1-python2  44) icu4c/60.1

The last loaded module clearly shows that the version of the R loaded is ``r/3.4.3-python2``.
To load the latest version of R, use the *fullName* of the module::

  $ module load r/3.6.1-python3



Module versions
---------------

What's the difference between ``module load r`` and ``module load
r/3.6.1-python3``?

The first loading ``r`` loads the version that Lmod assumes to be the
latest one. **This is necessarily not the latest one.** The second loading
``r/3.6.1-python3`` loads that exact version, which is supposed to not
change.  If you're not interested about the specific version, you can load it
without the version (but then *when* stuff randomly stops working, you're
going to have to figure out what happened).  Once you are past that
(possibly from day one!), it's usually a good idea to load specific
version, so that your environment will stay the same until you are
done.

This is most important for compiled software, but applies to
everything.



What's going on under the hood here?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In Linux systems, different environment variables like ``$PATH`` and
``$LD_LIBRARY_PATH`` help figure out how to run programs.  Modules just
cleverly manipulate these so that you can find the software you need,
even if there are multiple versions available.  You can see these variables
with the **echo** command, e.g. ``echo $PATH``.

When you load a module in a shell, the module command changes the current shell's environment variables,
and the environment variables are passed on to all the child processes.

You can explore more with ``module show NAME``.



Making a module collection
--------------------------

There is a basic dependency/conflict system to handle module dependency.
Each time you load a module, it resolves all the dependencies. This
can result in long loading times or be annoying to do each time you
log in to the system. However, there is a solution:
``module save COLLECTION_NAME`` and ``module restore
COLLECTION_NAME``

Let's see how to do this in an example.

Let's say that for compiling / running your program you need:

  - a compiler
  - CMake
  - MPI libraries
  - FFTW libraries
  - BLAS libraries

You could run this each time you want to compile/run your code::

  $ module load gcc/9.2.0 cmake/3.15.3 openmpi/3.1.4 fftw/3.3.8-openmpi openblas/0.3.7
  $ module list           # 15 modules

Let's say this environment works for you. Now you can save it with
``module save MY-ENV-NAME``.  Then ``module purge`` to
unload everything.  Now, do ``module restore MY-ENV-NAME``::

  $ module save my-env
  $ module purge
  $ module restore my-env
  $ module list           # same 15 modules

Generally, it is a good idea to save your modules
as a collection to have your desired modules
all set up each time you want to re-compile/re-build.

So the subsequent times that you want to compile/build,
you simply ``module restore my-env`` and this way
you can be sure you have the same previous environment.

.. note::
  You may occasionally need to rebuild your collections in case we
  re-organize things (it will prompt you to rebuild your collection
  and you simply save it again).



Full reference
--------------

.. include:: ../ref/modules.rst



Final notes
-----------

If you have loaded modules when you build/install software, remember
to load the same modules when you run the software (also in Slurm
jobs).  You'll learn about running jobs later, but the ``module load``
should usually be put into the job script.

The modules used to compile and run a program become part of its
environment and must always be loaded.

We use the Lmod system and Lmod works by changing environment variables.
Thus, they must be *sourced* by a shell and are only transferred to child processes.
Anything that clears the environment clears the loaded modules too. Module
loading is done by special functions (not scripts) that are
shell-specific and set environment variables.

Triton modules are also available on Aalto Linux: use ``module load
triton-modules`` to make them available.

Some modules are provided by Aalto Science-IT, some by CSC.  You could
even `make your own user modules
<https://lmod.readthedocs.io/en/latest/020_advanced.html>`__.



Exercises
---------

Before each exercise, run ``module purge`` to clear all modules.

Then, if you are in the FCCI (Finnish universities) but not at Aalto,
run ``module load fgci-common`` before the exercises to make the Aalto
modules available.

.. exercise:: Modules-1: Basics

   ``module avail`` and check what you see.  Find a software that has
   many different versions available.
   Load the oldest version.

.. exercise:: Modules-2: Modules and PATH

   ``PATH`` is an environment variable that shows from where programs
   are run.  See it's current value using ``echo $PATH``.

   ``type`` is a command line tool (a shell builtin, so your shell may
   not support it, but ``bash`` and ``zsh`` do) which tells you the
   full path of what will be run for a given command name - basically
   it looks up the command in ``PATH``

   * Run ``echo $PATH`` and ``type python``.
   * ``module load anaconda``
   * Re-run ``echo $PATH`` and ``type python``.  How does it change?

.. exercise:: Modules-3: Complex module and PATH

   Check the value of ``$PATH``.  Then, load the module ``py-gpaw``.
   List what it loaded.  Check the value of ``PATH`` again.  Why is
   there so much stuff?  Can you find a module command that explains
   it?

.. exercise:: (advanced) Modules-4: Modules and PATH

   Same as number 2, but use ``env | sort >
   filename`` to store environment variables, then swap to
   ``py-gpaw/1.3.0-openmpi-scalapack-python3``.
   Do the same, and compare the two outputs using ``diff``.

.. exercise:: Modules-5: Modules and dependencies

   Load a module with many dependencies, such as
   ``r-ggplot2`` and save it as a collection.  Compare the time
   needed to load the module and the collection.  (Does ``time``
   not work?  Change your shell to bash, see the previous tutorial)

.. exercise:: (advanced) Modules-6: Module contents

   Load ``openfoam-org/7-openmpi-metis``.
   Use ``which`` to find where executable ``blockMesh`` is
   coming from and then use ``ldd`` to find out what libraries it uses.



See also
--------

* Lmod documentation https://lmod.readthedocs.io/en/latest/

  * The "User documentation" https://lmod.readthedocs.io/en/latest/010_user.html



What's next?
------------

The next tutorial covers :doc:`data storage <storage>`.
