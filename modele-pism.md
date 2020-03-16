.. _modele-pism:

Building / Running ModelE-PISM Coupled Model
============================================

These instructions outline how to set up a coupled ModelE-PISM on
*discover* (or any computer), starting from scratch.  For the most
part, command lines that can be cut-n-pasted into *discover* are used;
but many pathes can be changed to suit your needs as well.

Build the Spack Environment
---------------------------

This builds the prerequisite software.  It only needs to be done once
by *someone*.  If you are not `eafisch2`, it has probably already been
done, and this is just for future reference.

Check out Spack
```````````````

It doesn't have to be in `~eafisch2/spack7`, you can put Spack wherever you like.

.. code-block:: bash

   cd ~eafisch2
   git@github.com:citibeth/spack.git -b efischer/giss2 spack7


Use Spack to Build Environement
```````````````````````````````

.. code-block:: bash

   ~eafisch2/spack7/bin/spack -e twoway-discover concretize -f
   ~eafisch2/spack7/bin/spack -e twoway-discover install
   ~eafisch2/spack7/bin/spack -e twoway-discover env loads -r
   pushd /gpfsm/dnb53/eafisch2/spack7/var/spack/environments/twoway-discover-latest
   sort loads | uniq >loads2
   cp loads2 loads


Set up your own Harness on the Spack Environment
------------------------------------------------

The Spack Environemnt above consists of all prerequisite packages,
plus a small number of packages you will build yourself.  A *Spack
Harness* consists of CMake setup scripts that use the Spack
Environment, but allow you to build your packages in your own private
location.  You can create as many harnesses as you like, for as many
checkouts / clones of the software as you like.


Start by creating the harness.  Note that you can put the harness in
any location you like, not just in `~/git`.  And you can name the
harness folder anything you like (the `-o` flag below):

.. code-block:: bash
   
   ~eafisch2/spack7/bin/spack -e twoway-discover env harness -o twoway-discover
   cd twoway-discoer

Load your Spack Environment
```````````````````````````

This needs to be done every time you log in or start a new shell.  You
might want to put it in your `.bashrc` file:

.. code-block:: bash

   source ~/git/twoway-discover/loads-x


Clone Your Software
```````````````````

Now clone the software you need:

.. code-block:: bash

   git clone git@github.com:citibeth/ibmisc.git
   git clone git@github.com:citibeth/icebin.git
   git clone git@github.com:pism/pism.git -b efischer/dev
   git clone <username>@simplex.giss.nasa.gov:/giss/gitrepo/modelE.git -b e3/twoway

.. note::

   Cloning ModelE requires you have an account on *simplex*.  Replace
   `<username>` with your Simplex username.

Build the Software
``````````````````

It should be built in the order: *ibmisc*, *icebin*, *pism*.  The first three are all built the same way; instructions for *ibmisc* are given here:

.. code-block:: bash

   pushd ibmisc
   mkdir build
   cd build
   python3 ../../ibmisc-setup.py
   make install -j20
   popd

To clean a build:

.. code-block:: bash

   # rm -rf ibmisc/build

In the future, if you edit any of these packages, you will need to
rebuild them.  If you edit header files in *ibmisc*, you will also
need to rebuild *icebin*.

