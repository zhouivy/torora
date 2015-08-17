# You will need the following to perform this build: #

  1. [Visual C++ Express 2005](http://go.microsoft.com/fwlink/?LinkId=51410)
  1. [Microsoft Visual C++ Express 2005 Service Pack 1](http://download.microsoft.com/download/7/7/3/7737290f-98e8-45bf-9075-85cc6ae34bf1/VS80sp1-KB926748-X86-INTL.exe)
  1. [If you are building from Vista, install Service Pack 1 Update for Windows Vista.](http://www.microsoft.com/downloads/details.aspx?FamilyID=90e2942d-3ad1-4873-a2ee-4acc0aace5b6&displaylang=en)
  1. [Install the Windows Server 2003 R2 Platform SDK](http://www.microsoft.com/downloads/details.aspx?familyid=0baf2b35-c656-4969-ace8-e4c0c0716adb&displaylang=en)
  1. [Follow steps 2 and 3 of “How to: Use Visual C++ Express Edition with the Microsoft Platform SDK.](http://msdn.microsoft.com/en-us/library/ms235626(VS.80).aspx)
  1. [ActivePerl](http://www.activestate.com/store/download.aspx?prdGUID=81fbce82-6bd5-49bc-a915-08d58c2648ca)
  1. [OpenSSl](http://www.openssl.org/source/)
  1. The following external GNU tools are needed from the GnuWin32 Project (Install to `C:\GnuWin32` rather than `C:\Program Files\GnuWin32` to avoid silly build failures later on):
    * [Bison](http://gnuwin32.sourceforge.net/downlinks/bison.php)
    * [GPerf](http://gnuwin32.sourceforge.net/downlinks/gperf.php)
    * [Flex](http://gnuwin32.sourceforge.net/downlinks/flex.php)
    * [LibIconv](http://gnuwin32.sourceforge.net/downlinks/libiconv.php)
    * [Make](http://gnuwin32.sourceforge.net/downlinks/make.php)
  1. [QT SDK (and QT Creator)](http://www.qtsoftware.com/downloads/sdk-windows-cpp)
  1. [Git for Windows](http://code.google.com/p/msysgit/)
  1. [SVN for Windows](http://subversion.tigris.org/getting.html#windows)
  1. [Download NSIS](http://nsis.sourceforge.net/Download)
  1. [Download the KillProcDLL\_plug-in](http://nsis.sourceforge.net/KillProcDLL_plug-in)

# Details #

## Step 1: Build OpenSSl ##

  1. [Download Visual C++ Express 2005](http://go.microsoft.com/fwlink/?LinkId=51410)
  1. [Download Microsoft Visual C++ Express 2005 Service Pack 1](http://download.microsoft.com/download/7/7/3/7737290f-98e8-45bf-9075-85cc6ae34bf1/VS80sp1-KB926748-X86-INTL.exe)
  1. [If you are building from Vista, install Service Pack 1 Update for Windows Vista.](http://www.microsoft.com/downloads/details.aspx?FamilyID=90e2942d-3ad1-4873-a2ee-4acc0aace5b6&displaylang=en)
  1. [Install the Windows Server 2003 R2 Platform SDK](http://www.microsoft.com/downloads/details.aspx?familyid=0baf2b35-c656-4969-ace8-e4c0c0716adb&displaylang=en)
  1. [Follow steps 2 and 3 of “How to: Use Visual C++ Express Edition with the Microsoft Platform SDK.](http://msdn.microsoft.com/en-us/library/ms235626(VS.80).aspx)
  1. [Download ActivePerl](http://www.activestate.com/store/download.aspx?prdGUID=81fbce82-6bd5-49bc-a915-08d58c2648ca)
  1. [Download OpenSSl](http://www.openssl.org/source/)
  1. Start Visual C++ Express 2005
  1. Click Tools->Visual C++ Command Prompt
  1. cd c:\location\of\extracted\openssl
  1. I had to manually set the LIB, PATH, and INCLUDE variables in the Visual C++ 2005 command prompt so that `C:\Program Files\Microsoft Platform SDK for Windows Server 2003 R2\XXX` was at the start of each environment variable again. Step 5 above should have taken care of this but Visual C++ 2005 must be stupid:
```
set PATH=C:\Program Files\Microsoft Platform SDK for Windows Server 2003 R2\Bin;%PATH%
set LIB=C:\Program Files\Microsoft Platform SDK for Windows Server 2003 R2\Lib;%LIB%
set INCLUDE=C:\Program Files\Microsoft Platform SDK for Windows Server 2003 R2\Include;%INCLUDE%
```
  1. Build from the Visual C++ command prompt:
```
    openssl_source_directory> perl Configure VC-WIN32
    openssl_source_directory> ms\do_ms
```
  * Note: You can also try ms\do\_masm. Only ms\do\_ms worked for me.
```
    openssl_source_directory> nmake -f ms\ntdll.mak
```
  1. If you're lucky, OpenSSL will build without a hitch. If not, you may get a rash of exotic errors. Some errors will require a `nmake clean` to recover from. This didn't work for me, so I ended up having to delete the source folder and start from scratch once or twice.

## Step 2: Fetch Torora Sources, Fetch Webkit ##

  1. [Download Git for Windows](http://code.google.com/p/msysgit/)
  1. [Download SVN for Windows](http://subversion.tigris.org/getting.html#windows)
  1. [Download patch.exe for Windows](http://gnuwin32.sourceforge.net/downlinks/patch.php)
  1. Launch Git from the start menu.
  1. Make sure the GnuWin32 packages are in your PATH: `set PATH=C:\program files\gnuwin32\bin;%PATH%`:
```
    $ cd $HOMEDIR
    $ git clone git://github.com/mwenge/torora.git
    $ svn checkout http://svn.webkit.org/repository/webkit/trunk %HOME%\WebKit
    $ cd %HOME%/WebKit
```

## Step 3: Build Webkit ##

Check https://trac.webkit.org/wiki/BuildingQtOnWindows for up to date instructions.

  1. The following external GNU tools are needed from the GnuWin32 Project:
    * [Download Bison](http://gnuwin32.sourceforge.net/downlinks/bison.php)
    * [Download GPerf](http://gnuwin32.sourceforge.net/downlinks/gperf.php)
    * [Download Flex](http://gnuwin32.sourceforge.net/downlinks/flex.php)
    * [LibIconv](http://gnuwin32.sourceforge.net/downlinks/libiconv.php)
    * [Make](http://gnuwin32.sourceforge.net/downlinks/make.php)
> > It is safest to install the gnuwin32 tools to `C:\GnuWin32` rather than `C:\Program Files\GnuWin32`. This is because, depending on your luck with the webkit build, the space in `Program Files` can prevent the windows shell from locating the binaries correctly. The error message below is what you will see if you get this problem.
```
make[2]: Entering directory `C:/Users/robert/Development/webkit/WebKitBuild/Release/WebCore'
bison -d -p jscyy ..\..\..\JavaScriptCore\parser\Grammar.y -o Grammar.tab.c && move Grammar.tab.c tmp\Grammar.cpp && move Grammar.tab.h tmp/Grammar.h
m4: cannot open `Files\GnuWin32/share/bison': No such file or directory
m4: cannot open `C:\Program': No such file or directory
m4: cannot open `Files\GnuWin32/share/bison/m4sugar/m4sugar.m4': No such file or directory
bison: I/O error
```
  1. [Download ActivePerl](http://www.activestate.com/store/download.aspx?prdGUID=81fbce82-6bd5-49bc-a915-08d58c2648ca)
  1. [Download QT SDK (and QT Creator)](http://www.qtsoftware.com/downloads/sdk-windows-cpp)
  1. Open for example a Qt Command Prompt from the Start Menu. `Click Start->Program Files->Qt->QT Command Prompt`
  1. Make sure the GnuWin32 packages are in your PATH as well as Perl (`set PATH=C:\GnuWin32\bin;C:\program files\gnuwin32\bin;C:\Perl\site\bin;%PATH%`.
  1. You also need to have %QTDIR% set and have %QTDIR%\bin in your PATH.
  1. Trim your PATH down as much as possible. Remove Git and Mingw from your PATH if you have them installed, their presence can cause odd build failures. For example, the PATH that works for me is:
```
set Path=`C:\GnuWin32\bin;C:\Program Files\GnuWin32\bin;c:\Qt\2009.01\qt\bin;c:\Qt\2009.01\bin;C:\Perl\bin;C:\Windows\system32;c:\Qt\2009.01\mingw\bin;C:\Perl\site\bin;C:\Windows;C:\Program Files\SlikSvn\bin\`.
```
> > Note: You may need to change `c:\Qt\2009.01\` to match the actual location of your Qt SDK installation.
  1. Change into the WebKit source tree: `cd c:\location\of\webkit`
  1. For some reason, the build command was unable to create the `WebKitBuild\Release` folders by itself on my PC. I had to do `mkdir WebKitBuild` and `mkdir WebKitBuild\Release` before building.
  1. You may need to do a `mkdir c:\tmp`. The WebKit build relies on the existence of `c:\tmp` when building in Windows.
  1. Build the patched webkit (release mode):
> > `$perl WebKitTools\Scripts\build-webkit --qt --release`
  1. The webkit build takes forever, so if you have more than one CPU on your machine you should use `--makeargs=j4` (for 4 CPUs). This will speed up the build no end. Other recommendations are `--no-svg`. There is even a `--minimal` flag available in recent webkit versions:
```
    $perl Tools\Scripts\build-webkit --qt --release --makeargs=j4
    $perl Tools\Scripts\build-webkit --qt --release --no-svg
    $perl Tools\Scripts\build-webkit --qt --release --minimal
```
  1. The safest way to recover fully from build failures is to delete the WebKitBuild folder and build again.

## Step 4: Build Torora in Qt Creator ##

  1. Open up Qt Creator
  1. Find the torora.pro file in your clone of the torora git repository and open it.
  1. The git repo contains a torora.pro.user which QT will use to set build environment variables.
  1. Navigate to Projects->Build Settings->Release->Build Environment in QT Creator.
    * In the list below `C:\users\robert\WebKit-SVN-source\webkit\WebKitBuild\Release\lib` is the location of the patched Webkit build. It needs to come at the start of the PATH variable (in Projects->Build Settings->Release->Build Environment) so that the patched webkit is picked up rather than the one that ships with your QT SDK. Also note the presence of the location of the OpenSSL libraries you built in Step 1: `C:\Users\robert\Documents\Development\openssl-0.9.8j\out32dll`
```
PATH=C:\users\robert\WebKit-SVN-source\webkit\WebKitBuild\Release\lib;c:\Qt\2009.01\mingw\bin;c:\Qt\2009.01\qt\bin;C:\Users\robert\Documents\Development\openssl-0.9.8j\out32dll
```
    * Again, the location of your patched webkit build needs to precede the location of the QT SDK in the QTDIR variable.
```
QTDIR=C:\Users\robert\WebKit-SVN-source\webkit;C:/Qt/2009.01/qt
```
    * This is needed for building against the patched webkit build. 'C:\Users\robert\WebKit-SVN-source\webkit' is the location of the webkit svn repo you downloaded.
```
  QT_WEBKIT=webkit_trunk
  WEBKITDIR=C:\Users\robert\WebKit-SVN-source\webkit
```
  1. Navigate to Projects->Build Settings->Release->Build Steps->Qmake in QT Creator. Change the 'additional arguments' for qmake to the existing list:
```
    DEFINES+=TORORA_WEBKIT_BUILD DEFINES+=TORORA CONFIG-=DEBUG CONFIG+=RELEASE
```
  1. Click Build->Run Qmake
  1. Click Build->Build All
  1. Click Ctrl->r to run and test the torora executable.

## Step 5: Creating the NSIS Installer ##

  1. [Download NSIS](http://nsis.sourceforge.net/Download)
  1. [Download the KillProcDLL\_plug-in and move the appropriate DLL into the NSIS plugins directory.](http://nsis.sourceforge.net/KillProcDLL_plug-in)
  1. Edit the hard-coded directory paths in arora\_source\_directory\windowsinstaller.nsi to reflect the appropriate location of the libraries installed above.
  1. In the following ones just change 'C:\Qt\2009.01\' to point to the location of the QT SDK you downloaded.
```
  File "C:\Qt\2009.01\mingw\bin\mingwm10.dll"
  File "C:\Qt\2009.01\qt\bin\QtCore4.dll"
  File "C:\Qt\2009.01\qt\bin\QtGui4.dll"
  File "C:\Qt\2009.01\qt\bin\QtNetwork4.dll"
  File "C:\Qt\2009.01\bin\phonon4.dll"
```
  1. Change '"C:\Users\robert\WebKit-SVN-source\webkit\WebKitBuild\Release\lib\' to reflect the location of the webkit svn repo you downloaded and built.
```
  File "C:\Users\robert\WebKit-SVN-source\webkit\WebKitBuild\Release\lib\QtWebKit4.dll"
```
  1. Change 'C:\Users\robert\Documents\Development\openssl-0.9.8j\out32dll' to reflect the location of the openssl source you downloaded and built.
```
  File "C:\Users\robert\Documents\Development\openssl-0.9.8j\out32dll\ssleay32.dll"
  File "C:\Users\robert\Documents\Development\openssl-0.9.8j\out32dll\libeay32.dll"
```
  1. Compile torora\_source\_directory\windowsinstaller.nsi with the NSIS compiler by right clicking on the file and choosing "Compile NSIS Script" or by using the NSIS compiler interface directly.
  1. Run the output installer file titled "Torora Snapshot (Date) Installer.exe".