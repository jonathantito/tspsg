TSP Solver and Generator Installation Guide
===========================================

Thank you for trying **TSP Solver and Generator**. This document will
guide you through the steps necessary to compile and run **TSPSG**.


1. Requirements
---------------

To be able compile **TSPSG** you need to have
*[Qt libraries](http://qt-project.org/)*. The minimum supported version
of *Qt* is **4.5.0**. The recommended version is **4.6.x** or higher.

**NOTE:** Please, note that there will be some regressions in
functionality if your version of *Qt* is lower than the recommended.


2. Assumptions
--------------

This guide assumes that you already have *Qt* libraries and all
necessary prerequisites installed.

Also, the following assumptions are made:

  - For **Linux/UNIX/BSD**: `lrelease` and `qmake` are avilable in
    `$PATH`.
  - For **Windows** (*MinGW*) and **Symbian**: you have installed *Qt
    SDK* or prebuilt libraries and have Start Menu items for *Qt* tools.
  - For **Windows** (*Visual Studio*) and **Windows Mobile**: the *Qt*
    libraries reside in `C:\Qt\`.
  - For **Windows Mobile**: *Windows Mobile 5.0 Pocket PC SDK* or later
    is installed.


3. Supported Platforms
----------------------

**TSPSG** is oficially supported and tested on the following platforms:

  - **Linux**: *Gentoo AMD64* and *Kubuntu 9.10 64-bit AMD*.
  - **Windows**: *Windows XP 32-bit* and *Windows 7 64-bit*.
  - **Windows Mobile**: *Windows Mobile 6.5 Professional Edition*.


4. Building and Installation <a name="s4"></a>
----------------------------------------------

### 4.1. Common Information ###

To be able ot build **TSPSG** you need to have the following *Qt*
modules: *QtCore*, *QtGui* and *QtSvg*. The first two are required, the
last one is optional. To get support for additional image formats (i.e.,
**JPEG** and **TIFF**) you'll additionally need corresponding *Qt
imageformats* plugins.

If you want to build **TSPSG** without **SVG** support add `nosvg` to
*qmake* `CONFIG` parameter, so that *qmake* command will typically be:

    qmake CONFIG+=release CONFIG+=nosvg

This way you will not depend on *QtSvg* module but will not be able to
export solution graph in **SVG** format.


**TSPSG** uses *qmake* `PREFIX` parameter to determine installation path
for make install command. If you don't specify it when running qmake, it
will be assigned the default value depending on the platform:

| Platform              | Default `PREFIX` value                  |
|-----------------------|-----------------------------------------|
| **Linux/UNIX/BSD**    | `/usr`                                  |
| **Windows**           | `%PROGRAMFILES%` environment variable\* |
| **Windows CE/Mobile** | `\Program Files`                        |
| **Symbian**           | *\<ignored\>*                           |

\* - usually, it is `C:\Program Files`.

**NOTE:** Please, note that there are no installation and/or packaging
rules for **MacOS** and other platforms not mentioned in this table.


By default, **TSPSG** uses precompiled header when being built. If you
experience problems with it you may add `CONFIG+=nopch` parameter to
*qmake* to disable the generation and use of the precompiled header.


### 4.2. General Procedure <a name="s42"></a> ###

On most platforms the general building and installation procedure is:

  1. Run `lrelease` to generate binary translation files (*.qm*) from
     the source (*.ts*).
  2. Run `qmake` with `CONFIG+=release` parameter to generate makefiles.
  3. Run `make` utility (e.g., *make*, *nmake*, *mingw32-make*) to build
     **TSPSG**.
  4. Run `make` utility with `install` parameter.

**NOTE:** It is important to run `lrelease` before `qmake`, or `qmake`
will not "pick up" the translations when generating installation rules.


### 4.3. Linux/UNIX/BSD ###

Open a shell, navigate to the directory where you have **TSPSG** source
downloaded and type

    tar xvjf tspsg-<VERSION>-src.tar.bz2
    cd tspsg-<VERSION>-src

where *\<VERSION\>* is the version of **TSPSG** you downloaded. Now run

    lrelease tspsg.pro
    qmake tspsg.pro
    make

In some cases you may need to type

    qmake tspsg.pro CONFIG+=release


If make step finished without errors you can install **TSPSG** by
running

    sudo make install

or

    su
    make install

depending on your distribution.

The executable goes to `<PREFIX>/bin`; *COPYING*, *ChangeLog.md*,
*README.md* and *INSTALL.md* go to `<PREFIX>/share/doc/TSPSG-<VERSION>`.


### 4.4. Windows ###

**TSPSG** will be installed to `<PREFIX>\TSPSG` folder.

**NOTE:** Please, read the [Section 7](#s7) after reading this Section.


#### 4.4.1. Using MinGW ####

Unpack the downloaded source code of **TSPSG** with your favourite
compression software. Now launch the **Qt Command Prompt** from the
Start Menu, navigate to the directory where you unpacked the source and
run

    lrelease tspsg.pro
    qmake tspsg.pro CONFIG+=release
    mingw32-make

**NOTE:** Make process may fail with a crash of *windres.exe*. If
you've run into this issue, please, read the [Section 6.1](#s61).

If make step finished without errors you can install **TSPSG** by
running

    mingw32-make install


#### 4.4.2. Using Visual Studio ####

Unpack the downloaded source code of **TSPSG** with your favourite
compression software. Now launch the **Visual Studio Command Prompt**
from the Start Menu, navigate to the directory where you unpacked the
source and run

    C:\Qt\bin\lrelease tspsg.pro
    C:\Qt\bin\qmake tspsg.pro CONFIG+=release
    nmake

If *make* step finished without errors you can install **TSPSG** by
running

    nmake install


### 4.5. Windows CE/Mobile ###

Unpack the downloaded source code of **TSPSG** with your favourite
compression software. Now launch the **Visual Studio Command Prompt**
from the Start Menu and run

    set PATH=C:\Qt\bin;%PATH%
    setcepaths wincewm50pocket-msvc2008

Now navigate to the directory where you unpacked the source and run

    lrelease tspsg.pro
    qmake tspsg.pro CONFIG+=release
    nmake

There is no automated installation process for **Windows Mobile** build.
To install **TSPSG** on your PDA you need to create a folder on your
device and copy the following files to it:

  - *tspsg.exe* from release folder in the source directory.
  - *QtCore4.dll* and *QtGui4.dll* from `C:\Qt\bin` folder.
  - *msvcr90.dll* from
    `C:\Program Files\Microsoft Visual Studio 9.0\VC\ce\dll\armv4i`
    folder.
  - all *.qm* files from `l10n` folder in the source directory to
    `l10n` subfolder.


### 4.6. Symbian ###

Unpack the downloaded source code of **TSPSG** with your favourite
compression software. Now launch the **Qt for Symbian Command Prompt**
from the Start Menu, navigate to the directory where you unpacked the
source and run

    lrelease tspsg.pro
    qmake tspsg.pro CONFIG+=release
    make release-gcce

**WARNING:** You need to unpack source code to the same drive as **Symbian
SDK** and the path must not contain any spaces or **TSPSG** won't build.

If make step finished without errors you can generate sis installation
file by running

    make sis

You'll get *tspsg.sis* file in the source directory. Copy it to your
phone and run or install using **Nokia PC Suite**.

**NOTE:** You need to install *Qt* libraries on your device before
installing **TSPSG**. Usually, it should be enough to install
*qt_installer.sis* from the *Qt* installation directory.

Alternatively, if you have installed **Nokia Smart Installer** you can
run

    make installer_sis

You'll get an *tspsg_installer.sis* that will automatically download and
install the required *Qt* libraries on **TSPSG** installation.

**NOTE:** Please, be aware that you have to sign the sis file to be able
to install it on your device. You can use [Open Signed Online][1] to
quickly sign the sis file for your device. Alternatively, you can try to
enable the installation of self-signed files in the phone settings.
Please, refer to your phone manual on the instructions how to do this.


### 4.7. Other Platforms, Supported by Qt ###

While **TSPSG** is oficially supported only on **Linux**, **Windows**
and **Windows Mobile** it should be possible to compile it on any
platform, supported by *Qt*. To do so, please, refer to the
[Section 4.2](#s42) for the general build and installation procedure.


5. Uninstallation
-----------------

Usually, it is enough to replace `install` parameter with `uninstall`
in the installation command from the [Section 4](#s4). Also, you can
manually delete all installed **TSPSG** files and directories.


6. Troubleshooting
------------------

### 6.1. WINDRES.EXE Crash <a name="s61"></a> ###

When building under **Windows** using **MinGW toolchain** make process
may fail with *windres.exe* crash (access violation). This is a known
bug in *windres.exe* regarding processing resource files with *UTF-8
(cp65001)* encoding ([Bug 10165][2]). To be able to successfully build
**TSPSG** you will need to download and replace *windres.exe* with a
fixed version. To do this:

  1. Open <https://sourceforge.net/projects/mingw/files/> in your
     favourite browser.
  2. Find and download the latest version of GNU Binutils. At the time
     of writing this guide it was
     *binutils-2.20.1-2-mingw32-bin.tar.gz*.
  3. Unpack the file *bin\windres.exe* from the downloaded archive to
     `<Your Qt installation path>\mingw\bin\` replacing the existing
     one.
  4. Now run

         mingw32-make distclean

     in the **TSPSG** directory and repeat the installation process.


7\. Notes <a name="s7"></a>
--------------------------

*qmake* doesn't always enclose installation paths in quotes. This may
cause some files not to be installed or removed when their path
contains spaces. In this case it is safe to delete these files and
**TSPSG** installation directory manually.


[1]: https://www.symbiansigned.com/app/page/public/openSignedOnline.do
[2]: http://sourceware.org/bugzilla/show_bug.cgi?id=10165


<!--
$Id: $Format:%h %ai %an$ $
$URL: http://tspsg.info/ $
-->
