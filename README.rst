xAH with CMake
==============

Setup
-----

To set up, create a directory to work in::

  mkdir rel21testing && cd $_

then clone me with the submodules fetched as well::

  git clone --recursive <github url>

Next, create a build directory to build from CMake, set up the release, then try making::

  mkdir build && cd $_
  setupATLAS
  asetup AnalysisBase,2.6.1
  cmake ../xAODAnaHelpersCMake
  make install DESTDIR=../install

After installing, my top-level looks like this::

  $ ls
  total 28K
  drwxr-xr-x  5 kratsg  117 Apr 19 17:24 ./
  drwxr--r-- 53 kratsg 4.0K Apr 19 17:17 ../
  -rw-r--r--  1 kratsg  13K Apr 19 10:56 .asetup.save
  drwxr-xr-x  6 kratsg 4.0K Apr 19 16:18 build/
  drwxr-xr-x  3 kratsg   32 Apr 19 16:12 install/
  drwxr-xr-x  4 kratsg  144 Apr 19 17:24 xAODAnaHelpersCMake/

where everything was installed to ``install``. Now, to run code, I updated ``xAH_run.py`` to check if we are on 2.5/2.6 release series (the ones that use CMake) and requires an extra option: ``-L/--library-linking`` which needs a path to the installed shared library file you just made! So creating an example ``config.py`` for testing purposes, I run the code like this::

  ./install/InstallArea/x86_64-slc6-gcc49-opt/bin/xAH_run.py --config config.py \
  --files input.root \
  -L install/InstallArea/x86_64-slc6-gcc49-opt/lib/libxAODAnaHelpers.so \
  direct

Notice that the path to the ``xAH_run.py`` needs to be specified. This isn't automatically in your ``$PATH`` yet, but I'm sure given time, a solution to make this happen in a cleaner way will come up. For now, just know that ``install/InstallArea/x86_64-slc6-gcc49-opt`` contains everything you've made, including ``bin/``, ``lib/``, ``data/``, and ``docs/``. The last thing is that I gave ``xAH_run.py`` the full path to where it can find the shared library, then inside ``xAH_run.py``, it executes the following calls::

    import ROOT
    ROOT.gSystem.Load(args.ext_library)
    xAH_logger.info("loading packages")
    ROOT.gROOT.Macro("$ROOTCOREDIR/scripts/load_packages.C")

where the shared library that was made needs to be loaded in first before you can load in the RootCore macro here.
