==========
Basic Info
==========

This is mainly for fixes, workarounds, or development/testing of dev-ada/
updates to use system-gcc instead of gnat-gpl (requires `ada-overlay`_ to
get toolchain Ada language support via USE=ada).

.. _ada-overlay: https://github.com/sarnold/ada-overlay

You can (optionally) add this overlay with layman::

  $ layman -f -a dev-ada -o https://raw.github.com/sarnold/dev-ada-overlay/master/layman.xml


Latest (sort of)
================

This overlay contains modified adacore ebuilds (recently added to the portage
tree for gnat-gpl only) for use with the system toolchain when built from
the above ada-overlay with USE=ada.  The toolchain overlay provides the
changes and bootstrap gnat compilers for each major version of gcc starting
with 5.x (the old gnat-gcc ebuilds stopped at 4.9.x).  This has also been
tested with crossdev and should provide gnat tools for all supported arches
(currently x86, amd64, arm, and arm64).

The modified adacore packages provide relaxed dependencies and a system-gcc
USE flag to build with the "standard" system toolchain with SUE=ada.  The
primary dependencies for all of the adacore tools are gprbuild and xmlada.
Since gprbuild needs itslef to build itslef, there is a bootstrap flag and
a rebuild step for gprbuild/xmlada and then all the tools should build from
there.

The adacore versions are a bit of a mystery (along with what matches what
compared to gnu.org), and the important bit is how up-to-date the Ada RTS
(RunTime System) is in the gnu.org source tree compared to the interface
expected by the tools.

The versions in this overlay are almost all 2017 and build with both gcc
6.4.0 and 7.2.0 as of this writing.  Only two packages had a mismatch with
6.4.0 so the 2016 versions are used in these cases.

.. note:: You can install these overlays with layman or just clone them
          somewhere local and add them to PORTDIR_OVERLAYS.  Also note the
          name of this overlay is "dev-ada" and not "dev-ada-overlay".

Setup and build steps
=====================

0. Install the `ada-overlay`_ and rebuild at least one of the above gcc
   versions (either 6.4.0 or 7.2.0).  See the `ada-overlay`_ README file
   for more details.  Add "ada" to your global flags for both gcc and
   a handful of packages with optional Ada bindings or features.

1. Install the `dev-ada-overlay`_ and setup as above.

2. Set your new toolchain with gcc-config.

3. Add system-gcc to your global USE flags (or for each package in
   package.use).  If you're using gcc-6.4.0, then you will need to
   add both >=dev-ada/gnat_util-2017 and >=dev-ada/asis-2017 to
   package.mask so it will install the 2016 versions instead

4. Emerge gprbuild with USE="bootstrap static", then rebuild gprbuild and
   xmlada with at least USE="shared".  Note the later packages will depend
   on which shared/static/static-pic flags were used for gprbuild.

5. Continue emerging other packages

6. Have fun!

TODO
====

These ebuilds are still not working or haven't been updated yet:

* gps-bin
* polyorb

Stay tuned...
