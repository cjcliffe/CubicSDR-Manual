.. _build-windows: 

============================
Building CubicSDR on Windows
============================

Windows 8.1/10, Visual Studio 64-bit: -- improvements welcome.


Install Visual Studio Community 2015
------------------------------------

If you don't already have Visual Studio 2015 you can install the free Microsoft "Visual Studio Community 2015" version available from https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx which was used for this guide.

During installation make sure you select the 'C++' compiler under 'Programming Languages' or you'll be unable to compile the project due to the missing C++ tools.

Build wxWidgets
---------------

Download https://github.com/wxWidgets/wxWidgets/releases/download/v3.1.0/wxWidgets-3.1.0.zip and unzip it to somewhere such as C:\MSVCDev\wxWidgets-3.1.0\

Navigate to C:\MSVCDev\wxWidgets-3.1.0\build\msw\ (or wherever you extracted) and open wx_vc14.sln.

Choose "Release" and "x64" for the build configuration and "Build Solution". All should compile successfully and you can close the project.

Install CMake:
--------------

Download CMake from:

::

    http://www.cmake.org/download/

For this guide I've used:

::

    https://cmake.org/files/v3.4/cmake-3.4.0-rc3-win32-x86.exe

Just install CMake with default or preferred options.

Install SoapySDR
----------------

Download ZIP or clone SoapySDR from https://github.com/pothosware/SoapySDR to C:\MSVCDev\SoapySDR

*    Launch CMake, set source path to C:/MSVCDev/SoapySDR/
*    Set destination to C:/MSVCDev/SoapySDR_win64/
*    Click "Configure" and choose "Visual Studio 14 2015 Win64" and Finish
*    Click "Generate"

Open "Developer Command Prompt for VS2015" by right-clicking and "Run as Administrator".

From the prompt:

::

   C:\> cd C:\MSVCDev\
   C:\MSVCDEV> cmake --build SoapySDR_win64 --config Release --target install
   ... Bunch of building ...
       0 Error(s)

Update your system environment variables (Search "enviornment variables" in windows 8/10 search) and append the following to the Path variable:

::

   ;C:\Program Files\SoapySDR\bin

Build CubicSDR:
---------------

*    Clone or download ZIP from https://github.com/cjcliffe/CubicSDR/ to C:\MSVCDev\CubicSDR
*    Run CMake GUI
*    Choose C:\MSVCDev\CubicSDR for source.
*    Choose C:\MSVCDev\CubicSDR_win64 for build folder.
*    Click Configure.
*    Choose "Visual Studio 14 2015 Win64" and Finish.
*    Set wxWidgets_ROOT_DIR to "C:\MSVCDev\wxWidgets-3.1.0".
*    Configure again, all should be good, then Generate.
*    Navigate to C:\MSVCDev\CubicSDR_win64\ in explorer and open CubicSDR.sln.
*    Once open select "Release" and "x64" build configuration and then "Build Solution" (F6)
*    CubicSDR.exe should now be in the output folder (i.e. C:\MSVCDev\CubicSDR_win64\x64) and ready to run (minus support modules).

Build Support Modules
=====================

SoapyRTLSDR
-----------

*    Clone or download ZIP from https://github.com/pothosware/SoapyRTLSDR to C:\MSVCDev\SoapyRTLSDR
*    Download http://sdr.osmocom.org/trac/attachment/wiki/rtl-sdr/RelWithDebInfo.zip and unpack to C:\MSVCDev\rtl-sdr-release\
*    Copy C:\MSVCDev\rtl-sdr-release\x64\libusb-1.0.dll and C:\MSVCDev\rtl-sdr-release\x64\rtlsdr.dll to C:\Program Files\SoapySDR\bin
*    Launch CMake, set source path to C:/MSVCDev/SoapyRTLSDR/
*    Set destination to C:/MSVCDev/SoapyRTLSDR_win64/
*    Click "Configure" and choose "Visual Studio 14 2015 Win64" and Finish
*    Set RTLSDR_INCLUDE_DIR to C:/MSVCDev/rtl-sdr-release/
*    Set RTLSDR_LIBRARY to C:/MSVCDev/rtl-sdr-release/x64/rtlsdr.lib
*    Click "Configure" again and then click "Generate"

Open "Developer Command Prompt for VS2015" by right-clicking and "Run as Administrator".

From the prompt:

::


   C:\> cd C:\MSVCDev\
   C:\MSVCDEV> cmake --build SoapyRTLSDR_win64 --config Release --target install
   ... Bunch of building ...
       0 Error(s)

SoapySDRPlay
------------

*    Clone or download ZIP from https://github.com/pothosware/SoapySDRPlay to C:\MSVCDev\SoapySDRPlay
*    Download "Windows API & Hardware Driver Installer" from http://sdrplay.com/windows.html and install it with defaults.
*    Copy C:\Program Files\MiricsSDR\API\x64\mir_sdr_api.dll to C:\Program Files\SoapySDR\bin

Open "Developer Command Prompt for VS2015" by right-clicking and "Run as Administrator".

From the prompt:

::


   C:\> cd "C:\Program Files\MiricsSDR\API\x64"
   C:\Program Files\MiricsSDR\API\x64>dumpbin /exports mir_sdr_api.dll > mir_sdr_api.def

Leave prompt open and edit the .def file down so it looks like this; remove some lines and prefixes and add "EXPORTS" at the top.

(reference only, these are the functions at the time of this instruction)

::


   EXPORTS
   mir_sdr_ApiVersion
   mir_sdr_DownConvert
   mir_sdr_Init
   mir_sdr_ReadPacket
   mir_sdr_ResetUpdateFlags
   mir_sdr_SetDcMode
   mir_sdr_SetDcTrackTime
   mir_sdr_SetFs
   mir_sdr_SetGr
   mir_sdr_SetGrParams
   mir_sdr_SetParam
   mir_sdr_SetRf
   mir_sdr_SetSyncUpdatePeriod
   mir_sdr_SetSyncUpdateSampleNum
   mir_sdr_SetTransferMode
   mir_sdr_Uninit

From the prompt:

::


   C:\Program Files\MiricsSDR\API\x64>lib /MACHINE:x64 /def:mir_sdr_api.def /OUT:mir_sdr_api.lib
   Microsoft (R) Library Manager Version 14.00.23026.0
   Copyright (C) Microsoft Corporation.  All rights reserved.
   
      Creating library mir_sdr_api.lib and object mir_sdr_api.exp
   C:\Program Files\MiricsSDR\API\x64>


*    Launch CMake, set source path to C:/MSVCDev/SoapySDRPlay/
*    Set destination to C:/MSVCDev/SoapySDRPlay_win64/
*    Click "Configure" and choose "Visual Studio 14 2015 Win64" and Finish
*    Click "Generate"

From the prompt:

::


   C:\> cd C:\MSVCDev\
   C:\MSVCDEV> cmake --build SoapySDRPlay_win64 --config Release --target install
   ... Bunch of building ...
       0 Error(s)




