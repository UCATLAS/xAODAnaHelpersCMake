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
