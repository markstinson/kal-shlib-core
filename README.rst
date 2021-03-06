What is kal-shlib ?
--------------------

This might or might not interest you as it was a personnal solution to manage
my LFS_ scripts. I was using a lot of shell script and a need emerged for a
shell code library. I'm using it since. "shlib" stands for "shell libs".

The shell lib component contains the core libraries that I've written that ease
a lot some common tasks I use when scripting in bash. I was looking at reducing
my programming time AND enhance the quality of my code. There's a lot to be
done.

The shell libs are divided into several sub-packages providing code for various
needs. An exception is ``kal-shlib-core`` which contains the central library
inclusion mecanism. It is thou a dependence of all ``kal-shlib-*`` packages.

.. _LFS: http://www.linuxfromscratch.org


What is kal-shlib-core ?
------------------------

This package only provide the library inclusion mecanism. Please note that it
provides also the ``shlib`` executable that allows to inline bash code from the
library into any shell script using shlib facilities. This allows you to use
the shell script on system without the kal-shlib-core package installed.

You can compare this with statically compiled C code, and dynamically linked
library. ``shlib`` allows you to go back and forth between these 2 states.


Do I need shlibs ?
------------------

If you take time shell-scripting, this might be of interest, some function have
actually saved me lot of time.

If you have found an utility for other component of the kal packages, you can
save place by making all binaries dynamically linked to these libraries. This
is especially usefull if you have installed more than one kal package.

I use ``shlib`` executable for some of my ``autogen.sh`` provided in some
other shell script I wrote.


Where can I find docs ?
-----------------------

There are very few docs at the moment. Best of all is to look at libraries
source code.


How can I install it ?
----------------------

Consider this release as alpha software. Use at your own risk. It may or may
not upgrade to a more user friendly version in future, depending on my spare
time.

From source
===========

You might have to consider running ``./autogen.sh`` if you got
source from ``git``.

If you got the source code thanks to downloading the ``tar.gz``,
running ``./autogen.sh`` shouldn't be necessary.

This package support GNU install quite well so a simple::

  # ./configure && make && make install

Should work (and has been tested and is currently used).

Note: you can specify a '--prefix=/your/location'

From debian package
===================

A debian package repository is available at::

  deb http://deb.kalysto.org no-dist kal-alpha

you should include this repository to your apt system and then::

  # apt-get update && apt-get install kal-shlib-core


What do this package contains ?
-------------------------------

- directory ``$prefix/lib/shlib/`` where library will be loaded
- an executable ``shlib`` installed in $prefix/bin
- a config file ``shlib`` installed in $prefix/etc

The debian package version will install to these location (and ``$prefix``
is set to ``/``)


How can I use the libraries in one of my scripts ?
--------------------------------------------------

These lines of code should be inserted at the beginning of your
shell script, before any other shell code, but after the shebang::

  #!- shlib loader
  #!-

Note that there two ``#!-`` at the beginning of line. You can put any comment
after. These 2 lines will mark the beginning and ending of the ``shlib
call``. Notice that you shouldn't write the code between the two lines yourself
unless you understand the following:

The shlib call is meant to call a ``source $prefix/etc/shlib``. To generate
automagically this call for your config, you can use the ``shlib`` executable::

  # shlib <filename>

..

  will do some checks, and if all checks succeeded, it'll output the actual
  shlib loader code. (that is in between the to lines beginning with ``#!- ``)

::

  # shlib d <filename>

..

  will *erase* and write a *dynamical* version of the shlib caller. This means
  that your shell script will look for the libraries at each call.

::

  # shlib s <filename>

..

  will *erase* and write a *statical* version of the shlib caller. It makes a
  snapshot of your current libraries and feeds it *in* your script.
