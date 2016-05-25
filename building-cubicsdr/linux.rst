.. _build-linux: 

===========================
Building CubicSDR for Linux
===========================

Basic build support (Debian)
---------------------------

::

   $ sudo apt-get install git build-essential automake cmake

Basic dependencies (Debian)
--------------------------

::

   $ sudo apt-get install libpulse-dev libgtk-3-dev

If you did not install your own OpenGL driver/headers (via Nvidia, AMD binaries or other) this will bring in the appropriate libs and headers: 

::

   $ sudo apt-get install freeglut3 freeglut3-dev

Build and install SoapySDR
--------------------------

::

   $ git clone https://github.com/pothosware/SoapySDR.git
   $ cd SoapySDR
   SoapySDR$ mkdir build
   build$ cd build
   build$ cmake ../ -DCMAKE_BUILD_TYPE=Release
   build$ make -j4
   build$ sudo make install
   build$ sudo ldconfig
   build$ SoapySDRUtil --info #test SoapySDR install

Build and install liquid-dsp
----------------------------

::

   $ git clone https://github.com/jgaeddert/liquid-dsp
   liquid-dsp$ cd liquid-dsp
   liquid-dsp$ ./bootstrap.sh
   liquid-dsp$ ./configure --enable-fftoverride 
   liquid-dsp$ make -j4
   liquid-dsp$ sudo make install
   liquid-dsp$ sudo ldconfig

Build static wxWidgets 
----------------------

Note: replace '~/Develop/wxWidgets-staticlib' with your own path if you prefer, remember it to be used later when building CubicSDR.

:: 

   $ wget https://github.com/wxWidgets/wxWidgets/releases/download/v3.1.0/wxWidgets-3.1.0.tar.bz2
   [ downloading.. ]
   $ tar -xvjf wxWidgets-3.1.0.tar.bz2  
   [ unpacking.. ]
   $ cd wxWidgets-3.1.0/
   wxWidgets-3.1.0$ mkdir -p ~/Develop/wxWidgets-staticlib
   wxWidgets-3.1.0$ ./autogen.sh 
   wxWidgets-3.1.0$ ./configure --with-opengl --disable-shared --enable-monolithic --with-libjpeg --with-libtiff --with-libpng --with-zlib --disable-sdltest --enable-unicode --enable-display --enable-propgrid --disable-webkit --disable-webview --disable-webviewwebkit --prefix=`echo ~/Develop/wxWidgets-staticlib` CXXFLAGS="-std=c++0x" --with-libiconv=/usr
   
   [ configuring.. ]
   
   wxWidgets-3.1.0$ make -j4 && make install
   
   [ building and installed to ~/Develop/wxWidgets-staticlib in this example ]
   
Build CubicSDR
--------------
Note: add -DUSE_HAMLIB=1 to cmake command line to include hamlib support.

::

   $ git clone https://github.com/cjcliffe/CubicSDR.git
   CubicSDR$ cd CubicSDR
   CubicSDR$ mkdir build
   CubicSDR$ cd build
   build$ cmake ../ -DCMAKE_BUILD_TYPE=Release -DwxWidgets_CONFIG_EXECUTABLE=~/Develop/wxWidgets-staticlib/bin/wx-config
   build$ make
   # You can now run the build from the folder, note if you're on 32-bit linux it will be in x86/
   build$ cd x64/
   x64$ ./CubicSDR


Install CubicSDR (and launcher) 

::

   build$ sudo make install


Un-install CubicSDR 

:: 

   build$ sudo make uninstall 


Support Modules
---------------

SoapyRTLSDR

::


   $ sudo apt-get install librtlsdr-dev
   $ git clone https://github.com/pothosware/SoapyRTLSDR.git
   $ cd SoapyRTLSDR
   SoapyRTLSDR$ mkdir build
   SoapyRTLSDR$ cd build
   build$ cmake .. -DCMAKE_BUILD_TYPE=Release
   build$ make
   build$ sudo make install
   build$ sudo ldconfig
   # should now show RTL-SDR device if connected
   build$ SoapySDRUtil --probe     


SoapySDRPlay
------------

Note: requires API from http://sdrplay.com/linux.html to be installed first.
** Also note that the SoapySDRPlay installer will at present time install an earlier SoapySDR binary -- please re-run 'sudo make install' for your SoapySDR build folder to update to the build version after installing.

::

   $ git clone https://github.com/pothosware/SoapySDRPlay.git
   $ cd SoapySDRPlay
   SoapySDRPlay$ mkdir build
   SoapySDRPlay$ cd build
   build$ cmake .. -DCMAKE_BUILD_TYPE=Release
   build$ make
   build$ sudo make install
   build$ sudo ldconfig
   build$ SoapySDRUtil --probe


* Always ensure to update, build and install SoapySDR before building dependent projects.

Ubuntu 15.10 Note:
------------------

If you've installed a graphics driver that includes OpenGL and your libGL.so currently points to an invalid mesa/libGL.so you may get a compiler error:

::

   make[2]: *** No rule to make target '/usr/lib/x86_64-linux-gnu/libGL.so', needed by 'x64/CubicSDR'.  Stop.


Checking the link should reveal that it's pointing at a deleted file: 

::

   $ ls -lah /usr/lib/x86_64-linux-gnu/libGL.so
   lrwxrwxrwx 1 root root 13 Oct  9 01:16 /usr/lib/x86_64-linux-gnu/libGL.so -> mesa/libGL.so

To fix the link first remove the old one:

::

   $ sudo rm /usr/lib/x86_64-linux-gnu/libGL.so

Then check where libGL.so.1 is pointing:

::

   $ ls -lah /usr/lib/x86_64-linux-gnu/libGL.so.1
   lrwxrwxrwx 1 root root 15 Dec 20 19:03 /usr/lib/x86_64-linux-gnu/libGL.so.1 -> libGL.so.358.16


And create a new link to the same location:

::

   $ sudo ln -s /usr/lib/x86_64-linux-gnu/libGL.so.358.16 /usr/lib/x86_64-linux-gnu/libGL.so



