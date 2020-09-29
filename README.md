Building the RISC OS Software
=============================

A number of the repositories here contain software for RISC OS, which can be built under the [GCCSDK](http://www.riscos.info/index.php/GCCSDK). This document gives an overview of the environment required to use the source code.


Set up the environment
----------------------

To build the software found here, it will be necessary to have suitable Linux system with a working installation of the [GCCSDK](http://www.riscos.info/index.php/GCCSDK). It will also be necessary to install a number of additional tools.

Start by installing the GCCSDK, following the usual process in its documentation. It is assumed that the `GCCSDK_INSTALL_CROSSBIN` and `GCCSDK_INSTALL_ENV` environment variables point to sensible locations within the installation, perhaps set up in a similar way to this (the home folder name should be amended to suit your own system):

	export GCCSDK_INSTALL_CROSSBIN=/home/steve/gccsdk/cross/bin
	export GCCSDK_INSTALL_ENV=/home/steve/gccsdk/env

In addition, three folders will be required for the "SFTools" build environment. These can be anywhere convenient, but it is likely that they will be located somewhere within the same home directory. Again, an example would be:

	export SFTOOLS_BIN=/home/steve/sftools/bin
	export SFTOOLS_BASIC=/home/steve/sftools/basic
	export SFTOOLS_MAKE=/home/steve/sftools/make


Build OSLib
-----------

It will be necessary to build OSLib using the GCCSDK Autobuilder:

	cd ~/gccsdk/build
	../autobuilder/build -v oslib

In addition, it will be necessary to separately build a "hard float" version of OSLib, which can't be done through the Autobuilder. Moving to a suitable working directory for checking out and developing code, use Subversion to check out the OSLib source and then build it.

	svn co https://svn.code.sf.net/p/ro-oslib/code/trunk/\!OSLib OSLib
	cd OSLib
	make ELFOBJECTTYPE=HARDFPU

The resulting library file can be copied into the GCCSDK environment by hand -- note the additional 'H' in the target filename, to identify the "hard float" version of the code:

	cp Build/libOSLib32.a $GCCSDK_INSTALL_ENV/libOSLibH32.a

At this point, the BindHelp utility used by OSLib to make a StrongHelp manual can be copied into the SFTools bin folder for use by the manual generation tools:

	mkdir -p $SFTOOLS_BIN
	cp Bin/bindhelp $SFTOOLS_BIN


Build the SFTools
-----------------

It is now a case of cloning a number of build tools. Returning to your development folder, clone the following two repositories:

* makefiles.git
* mantools.git

In the order given here, descend into each cloned repository and `make install` to install the tools within the approproate SFTools folders:

	cd makefiles
	make install
	cd ../mantools
	make install

Since ManTools is required to fully build the Makefiles, you should now return to the makefiles folder and repeat the process.

	cd ../makefiles
	make install
	cd ../mantools
	make install

With these in place, clone the remaining tools, in each case entering the folder and running `make install` to build and install the component. Again, these depend on each other, so it's essential that they are made in the order shown in the list.

* packtools.git
* tokenize.git
* menugen.git
* wimplib.git
* swiheader.git
* flexlib.git
* sflib.git

If all of these stages complete successfully, you should now have a working build environment for the other source code.