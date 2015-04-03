Nucleo-f091rc Demo Application using FreeRTOSv8.2.0
=======================================================

This Eclipse project takes the github repositories
<http://rgrover/stm32f0-discovery>, adds stm standard
periphal library, STM32F0xx_StdPeriph_Lib_V1.5.0 and
the FreeRTOS V 8.2.0 source.  I chose to include these
because I don't know for certain that compatible versions
of these will be available.

Toolchain:
[ARMGCC](<https://launchpad.net/gcc-arm-embedded)

IDE:
Eclipse Luna
ARM Eclipse plugin (including OpenOCD)

Code Organization
-----------------

* All source files for this particular project (including main.c) are contained within the subfolder **Source/**.
  * **Source/system_stm32f0xx.c** is the place where system clocks are initialized. It comes out of an excel sheet developed by STM. The file included in this repository is taken from the STM32F0-Discovery firmware package.

* The **startup/** folder contains device specific files:
   * **startup_stm32f0xx.S** is the startup file taken from the STM32F0-Discovery firmware package.
   * Linker Script (**stm32f0.ld**) is a copy (with slight modifications) from one of the templates within the peripheral library.

* **OpenOCD/** contains a script file used to write the HEX image to the board via [OpenOCD](http://openocd.sourceforge.net/).

Externals
---------

### OpenOCD

OpenOCD needs to be compiled for STLINK support. Sources may be obtained by
cloning [the git repository](http://openocd.git.sourceforge.net/git/gitweb.cgi?p=openocd/openocd;a=summary). You can place this cloned folder anywhere.

Building the Project
--------------------
*The following is copied from the original github repository.*s

Eclipse makes this very simple if the GNU ARM plugin is installed. The
toolchain is discovered automatically (as long as it is available on the
PATH). You should import this as an existing project using
File->Import->General->Existing Project into Workspace.

Build creates a folder called 'Debug' containing makefile and the build
artifacts. Thereafter it is possible to run **make all** from the command line
directly from within the Debug folder. Note: if any configuration options or
build dependencies are changed, the makefile under **Debug** can be
regenerated from the Eclipse IDE.

Building the Debug configuration produces ~~a **stm32f0-discovery.hex**~~
an **Nucleo-f091rc** binary
artifact under **Debug/**. This is the firmware which needs to be loaded on
the target.

Loading the image on the board
------------------------------

### Build OpenOCD

OpenOCD must be installed with stlink enabled. Clone [the git repository](http://openocd.git.sourceforge.net/git/gitweb.cgi?p=openocd/openocd;a=summary) and use these commands to compile/install it:

    ./bootstrap
    ./configure --prefix=/usr --enable-maintainer-mode --enable-stlink
    make

If there is an error finding the .cfg file, please double-check the
OPENOCD_BOARD_DIR constant at the top of the Makefile (in this template
directory, not in OpenOCD).

In my case, OpenOCD's sources (and binaries) resided under **<workspacedir>/openocd-code/**.

### UDEV Rule for the Discovery Board

If you are not able to communicate with the STM32F0-Discovery board without
root privileges you should follow the step from [the stlink repo readme file](https://github.com/texane/stlink#readme) for adding a udev rule for this
hardware.

### Finally

    $ <path_to_openocd-code>/src/openocd -s <path_to_openocd-code>/tcl/ -f interface/stlink-v2.cfg -f <path_to_openocd-code>/tcl/target/stm32f0x_stlink.cfg -f <path_to_project_folder>/OpenOCD/stm32_program.cfg


Acknowledgement
---------------

Sourced partly from https://github.com/szczys/stm32f0-discovery-basic-template/ which
is based on is based on [an example template for the F4 Discovery board](http://jeremyherbert.net/get/stm32f4_getting_started) put together by Jeremy Herbert.
=======
# FreeRTOS-Nucleo-f091rc
An Eclipse FreeRTOS Demo project using ARM Eclipse plugin, ARMGCC toolchain, OpenOCD 
