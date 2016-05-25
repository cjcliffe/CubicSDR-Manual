.. _build-osx:

========================
Building CubicSDR on OSX
========================


Prerequisites

You need to have XCode and macports / homebrew installed.

Install liquid-dsp
------------------

You can use MacPorts or Brew for this part.

MacPorts:
+++++++++

::

   ccliffe$ sudo port install liquid-dsp cmake
   .. installing ..

Homebrew:
+++++++++

::


   ccliffe$ brew install automake cmake
   .. installing ..
   ccliffe$ git clone https://github.com/jgaeddert/liquid-dsp.git
   ccliffe$ cd liquid-dsp
   liquid-dsp$ ./bootstrap.sh
   liquid-dsp$ ./configure --enable-fftoverride 
   .. configuring ..
   liquid-dsp$ make -j4
   .. building ..
   liquid-dsp$ sudo make install
   .. installing ..

Build wxWidgets
---------------

Substitute your own user paths where appropriate.

::


   ccliffe$ mkdir ~/Dev
   ccliffe$ cd ~/Dev
   Dev$ mkdir wxWidgets-build
   Dev$ wget -O wxWidgets-3.1.0.tar.bz2 wget https://github.com/wxWidgets/wxWidgets/releases/download/v3.1.0/wxWidgets-3.1.0.tar.bz2
   Dev$ tar -xvjpf wxWidgets-3.1.0.tar.bz2
   ... unpacking ...
   Dev$ cd wxWidgets-3.1.0
   wxWidgets-3.1.0$ ./configure --with-opengl --disable-shared --enable-monolithic --with-libjpeg --with-libtiff --with-libpng --with-zlib --with-mac --disable-sdltest --enable-unicode --enable-display --enable-propgrid --disable-webkit --disable-webview --disable-webviewwebkit --with-macosx-version-min=10.9 --prefix=/Users/(YOUR_USERNAME)/Dev/wxWidgets-build CXXFLAGS="-std=c++0x" --with-libiconv=/usr
   ... configuring ...
   wxWidgets-3.1.0$ make -j4 && make install
   ... building, installing ...
   
Build SoapySDR
--------------

::


   ccliffe$ cd ~/Dev
   Dev$ git clone https://github.com/pothosware/SoapySDR.git
   Dev$ cd SoapySDR
   SoapySDR$ mkdir build
   SoapySDR$ cd build
   build$ cmake .. -DCMAKE_BUILD_TYPE=Release
   build$ make
   build$ sudo make install
   build$ SoapySDRUtil --info

Build CubicSDR
--------------

Note: add -DUSE_HAMLIB=1 to cmake command line to include hamlib support.

::


   ccliffe$ git clone https://github.com/cjcliffe/CubicSDR.git
   .. cloning ..
   ccliffe$ cd CubicSDR
   ccliffe$ mkdir build
   ccliffe$ cd build
   build$ cmake ../ -DwxWidgets_CONFIG_EXECUTABLE=/Users/(YOUR_USERNAME)/wxWidgets/wxWidgets-staticlib/bin/wx-config -DCMAKE_BUILD_TYPE=Release -DBUNDLE_APP=1 -DCPACK_BINARY_DRAGNDROP=1
   ... generating ...
   -- Build files have been written to: .../CubicSDR/build
   build$ cpack
   .. compiling / bundling ..

CubicSDR.app should now be built as x64/CubicSDR.app as well as a .DMG bundle (and possibly some other default bundles)


Example Support Modules:
------------------------

SoapyRTLSDR
+++++++++++

::


   ccliffe$ cd ~/Dev
   Dev$ sudo port install rtl-sdr
   Dev$ git clone https://github.com/pothosware/SoapyRTLSDR.git
   Dev$ cd SoapyRTLSDR
   SoapyRTLSDR$ mkdir build
   SoapyRTLSDR$ cd build
   build$ cmake .. -DCMAKE_BUILD_TYPE=Release
   build$ make
   build$ sudo make install
   build$ SoapySDRUtil --probe

SoapySDRPlay 
++++++++++++

*(though usually latest SoapySDRPlay package will be available from www.sdrplay.com/mac.html)*

::


   ccliffe$ cd ~/Dev
   Dev$ git clone https://github.com/pothosware/SoapySDRPlay.git
   Dev$ cd SoapySDRPlay
   SoapySDRPlay$ mkdir build
   SoapySDRPlay$ cd build
   build$ cmake .. -DCMAKE_BUILD_TYPE=Release
   build$ make
   build$ sudo make install
   build$ SoapySDRUtil --probe

*Always ensure to update, build and install SoapySDR before building dependent projects.*




