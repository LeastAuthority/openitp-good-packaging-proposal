
============================================================
 Good Packaging of LAFS for Windows, Mac, Linux, and Python
============================================================

OpenITP “Last Mile” proposal

• Principal Investigators:

  • Zooko Wilcox-O'Hearn <zooko@LeastAuthority.com>
  • Nathan Wilcox <nathan@LeastAuthority.com>
  • Daira Hopwood <daira@LeastAuthority.com>

• Organization Name:

  *Least Authority Enterprises*

• Technical & Administrative Contact::

     Zooko Wilcox-O'Hearn
     3450 Emerson Ave.
     Boulder, CO 80305-6452
     Phone: 303-543-2301

===================
 Executive Summary
===================

`Tahoe-LAFS`_ (the Least-Authority File System) implements a secure,
decentralized storage network. It is widely regarded as having
excellent quality and security, and it is being used by an increasing
number of people. However before this project, it was not available
as a precompiled package for Windows or Macintosh users.

.. _Tahoe-LAFS: https://Tahoe-LAFS.org

A related issue is that there is currently no way for a user to
*verify* that a binary package they acquire was generated from the
alleged source code. Such *verifiable builds* are important for users
to gain the security benefits from source-code-oriented security
auditing performed by others.

For this project we proposed:

#. to extend the Tahoe-LAFS packaging to reach Windows and Macintosh
   users and Python programmers,
#. to do so in a transparent, and reproducible way, and
#. to document how this approach can move us toward a world of
   verifiable builds.

=====================
 Tahoe-LAFS Overview
=====================

LAFS is widely regarded as having excellent quality and security. A
`report by the Comprehensive National Cybersecurity Initiative`_ said
“We wish to highlight the Least-Authority File System, a
cross-platform open-source software solution which demonstrates both
secure chunking and redundant data decentralization. … It promotes an
explicitly secure, fault- tolerant model: stored files are broken into
pieces, encrypted, and the pieces are redundantly stored across
arbitrarily many servers. … Wider deployment of this type of file
storage system would have an immediate impact on the quality of modern
data protection.”.

.. _report by the Comprehensive National Cybersecurity Initiative: https://tahoe-lafs.org/~zooko/National_Cyber_Leap_Year_Summit_2009_CoChairs_Report.pdf

It has also received `praise from Jacob Appelbaum and
Richard M. Stallman`_, among others. It has been studied and used by
computer scientists, hackers, Free and Open Source software
developers, activists, the U.S. Defense Advanced Research Projects
Agency (DARPA), and the U.S. National Security Agency (NSA).

.. _praise from Jacob Appelbaum and Richard M. Stallman: https://leastauthority.com/blog/least-authority-announces-prism-proof-storage-service.html

The design has been published in a `peer-reviewed science paper`_, and
has been `cited more than one hundred times`_.

.. _peer-reviewed science paper: http://eprint.iacr.org/2012/524

.. _cited more than one hundred times: http://scholar.google.com/scholar?q=%22least-authority+filesystem%22+OR+%22tahoe-lafs%22+OR+%22least-authority+file+system%22&btnG=&hl=en&as_sdt=0%2C6

LAFS provides *end-to-end* security, meaning that the end user's
confidentiality and integrity guarantees are not subject to the
discretion of any third party. The ultimate goal of the LAFS project
is to put control of every individual's data into that person's hands.

LAFS appears to have a steadily growing userbase based on `ubuntu
popularity contest`_, `debian popularity contest`_, and `crate.io python
package info`_ installation statistics.

.. _`ubuntu popularity contest`: http://www.lesbonscomptes.com/upopcon/
.. _`debian popularity contest`: http://qa.debian.org/popcon.php?package=tahoe-lafs
.. _`crate.io python package info`: https://crate.io/packages/allmydata-tahoe/

===========
 Packaging
===========

Tahoe-LAFS is currently distributed in two ways:

#. A source code distribution, which users then have to build
   themselves, following `instructions`_ included in the source
   distribution and posted on the Tahoe-LAFS.org web site.

#. `Binary packages`_ for each of several Linux and NetBSD operating
   systems. These binary packages are built and distributed by the
   operating system maintainers themselves rather than by the
   Tahoe-LAFS developers.

.. _instructions: https://tahoe-lafs.org/trac/tahoe-lafs/browser/trunk/docs/quickstart.rst
.. _Binary packages: https://tahoe-lafs.org/trac/tahoe-lafs/wiki/Installation

We propose to extend this to include a third and fourth kind of
distribution, without interfering with the maintenance and improvement
of the first two kinds:

#. Binary packages for Macintosh and Windows, built and distributed by
   the Tahoe-LAFS developers themselves.

   This would make it possible for Macintosh and Windows users to
   install and use LAFS without building it themselves.

#. A Python package that can be automatically and securely installed
   from source code using the standard Python tool "pip". The source
   code will (of course) be maintained by the Tahoe-LAFS developers,
   but mirrors of the source code will be hosted by the standard
   `Python package repository`_.

   This will make it possible for Python programmers to install
   Tahoe-LAFS using their standard tools, and will facilitate
   making Tahoe-LAFS a dependency of their own Python packages.

   We made progress on this goal within the scope of the OpenITP
   packaging project (see tickets `#2032`_, `#2209`_, `#2291`_),
   but completing it remains future work (`#2207`_, `#2210`_, `#2317`_).

.. _`Python package repository`: https://pypi.python.org
.. _`#2032`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2032
.. _`#2207`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2207
.. _`#2209`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2209
.. _`#2210`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2210
.. _`#2291`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2291
.. _`#2317`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2317

========================================
 Reproducible and Transparent Packaging
========================================

We propose to do this work in a public and reproducible way by using
*test-driven development* at each step, so that third parties can
verify that the changes we make have the desired results. This means
writing test cases that verify that the packaging process produces the
correct outcome. It also means using a *continuous integration*
service, such as Buildbot_ or Travis-CI_ which runs after each commit
and publicly displays every step of the process and the results of
each step.

.. _Buildbot: http://buildbot.net/
.. _Travis-CI: https://travis-ci.org/

One of the benefits of this test-driven approach is that we are less
likely to suffer regressions and bit-rot. Tahoe-LAFS already had
Windows and Macintosh packages built for it, several years ago, but
the scripts that generated those packages bit-rotted during a period
of insufficient maintenance. By writing comprehensive tests of the
packaging system, we can prevent bit-rot.

Another benefit of this reproducible approach is that third parties
can see exactly how our packaging works, and can help us debug it, and
integrate it into other systems such as Linux distributions and the
Python packaging infrastructure. This will become even more important
when we begin to grapple with the “Verifiable Builds” issue (see
below).

A third benefit of this approach is that it makes it easier for third
parties to copy parts of our build process that they want to use for
their own projects. This too will become more important in the context
of “Verifiable Builds”.

Finally, by having a transparent set of packaging tests, the Tahoe-LAFS
project will benefit from a canonical, specific definition of packaging
targets.

===========================
 Towards Verifiable Builds
===========================

A long-term goal, which we will make progress toward in the scope of
this project, is to enable end-users to verify that the package of
Tahoe-LAFS that they are using was generated from the exact same
source code that a security auditor examined. This requires builds
of the package to be *repeatable*.

In order to explain the repeatable build concept and how it can be
used to verify software, consider this simple diagram::

    distributor: source code ➾ binary package[platform] → user

Here we use “➾” to mean “build” — the process that produces usable
packages out of source code.

Now consider a security auditor who does a source-code-based
examination (as opposed to binary-based, which is called “reverse
engineering”). This security auditor will start with the source code,
and examine it for vulnerabilities or backdoors.::

    auditor: source code → security audit

How can the user who receives a binary package know whether that
package was built from the source that the auditor examined?

The “repeatable build” approach attempts to answer that question by
having the security auditor perform the “source code ➾ binary package”
on their own trusted system for each relevant platform, and then
taking a fingerprint (secure hash) of each resulting binary package::

   for each platform:
     auditor: source code ➾ binary package[platform]
     auditor: binary package[platform] → generate fingerprint

The auditor then publishes these fingerprints along with their report
about their security audit. Users who receive the binary package for
a given platform can take a fingerprint of that package and compare it
to the fingerprint in the published report.::

   distributor: source code ➾ binary package[platform] → user
   user: binary package[platform] → check fingerprint

This approach can work only if, for each platform, the ➾ operation
performed by the distributor results in a bytewise-identical binary
to the ➾ operation performed by the security auditor.

The definition of a "platform" must be clearly stated so that
auditors know which fingerprints need to be generated, and users
know which fingerprint to compare.

Here is a news article from LWN.net about the concept of repeatable
builds (prompted in part by an open letter that we wrote): `“Security
software verifiability”`_. Here is a `post on the tahoe-dev mailing
list`_ and an `issue tracker ticket`_ about our desire to have
repeatable builds for Tahoe-LAFS.

.. _“Security software verifiability”: https://lwn.net/Articles/564263/
.. _post on the tahoe-dev mailing list: https://tahoe-lafs.org/pipermail/tahoe-dev/2013-August/008684.html
.. _issue tracker ticket: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2057

Goals for this deliverable
--------------------------

For this OpenITP proposal, our goal was to have documentation of
the ways in which Tahoe-LAFS builds are *not* currently repeatable
(`#2357`_). The scope of this documentation includes:

* Tahoe-LAFS as built via setup.py (using setuptools and/or pip), and
* the Mac OS X (`#182`_) and Windows (`#195`_) packages 

but does not include Tahoe-LAFS as packaged by an operating system
distribution or package management system.

In order for a build to be repeatable, a successful build for a given
platform must always produce the same binary. We therefore need to
catalogue any sources of nondeterminism in the build process that
could result in different binaries being produced.

Nondeterminism that results in obvious build failures is not a problem,
because by assumption the auditor only produces fingerprints for
successfully built packages.

.. _`#2357`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2357

Sources of nondeterminism
-------------------------

We start by considering the "quickstart" build flow, which builds a
copy of Tahoe-LAFS for the current platform as documented in
`quickstart.rst`_. (This build flow is also one of the steps in
creating the Mac OS X and Windows packages.)

.. _quickstart.rst: https://tahoe-lafs.org/trac/tahoe-lafs/browser/docs/quickstart.rst

#. Install Python

   A source of nondeterminism is the version of Python installed (including
   any OS distribution-specific patches). If an existing Python installation
   is used, it may have been customized or modified by installing other
   Python packages. Depending on the platform and installation method,
   there may also be user choices of installation directory, optional
   components to be installed, and whether the installation is per-user or
   shared across users, that could affect the behaviour of the resulting
   Python instance. It is also possible that multiple instances of Python
   may be installed.

   [NONDET: operating system versions, patches, variants, distribution if counted as the same target]

#. Get Tahoe-LAFS

   The user is directed to download the latest stable release. Release
   archives are provided in six formats: three "SUMO" formats that include
   all dependencies, and three "non-SUMO" formats that only include the
   source code for Tahoe-LAFS itself. Each of these is provided as ``.zip``,
   ``.tar.bz2`` and ``.tar.gz`` archive types. The link from `quickstart.rst`_
   is to the non-SUMO ``.zip`` archive. It may be useful to reduce the
   number of formats provided in order to simplify support for repeatable
   builds.

   The auditor must have an authentic copy of the same release, and a
   correct program for extracting the archive. The archive must be
   extracted into a new directory. There are potential sources of
   nondeterminism in how it is extracted:

   * The permissions of the extracted files may vary depending on the
     extraction program and options, and the umask (or similar on
     non-Unix operating systems) of the user.
   * File timestamps may depend on the clock of the build system.
   * The order of files and subdirectories in each directory may vary,
     if the filesystem or extraction program does not automatically
     sort them.
   * Filesystems may vary in case sensitivity; this can matter if an
     archive has entries in the same directory with names differing
     only in case (or Unicode normalization form).

#. Build Tahoe-LAFS

   The user is directed to run "``python setup.py build``". Sources of
   nondeterminism could include:

   * The version of Python that is run. This should be the one chosen
     in step 1 above, but may not be if other versions have been
     installed -- see for example ticket `#1302`_ ("installing Python 3
     breaks ``bin\tahoe`` on Windows"). It is also potentially possible
     for Python subprocesses to use a different instance of Python,
     although the build attempts to avoid this.
   * The shell that runs Python, and the environment variables set in
     that shell. This includes variables specific to Python, of which
     there are many (``PYTHONPATH``, ``PYTHONDONTWRITEBYTECODE``,
     ``PYTHONDEBUG``, ``PYTHONINSPECT``, ``PYTHONOPTIMIZE``,
     ``PYTHONNOUSERSITE``, ``PYTHONUNBUFFERED``, ``PYTHONVERBOSE``,
     ``PYTHONWARNINGS``, ``PYTHONSTARTUP``, ``PYTHONHOME``,
     ``PYTHONCASEOK``, ``PYTHONIOENCODING``, ``PYTHONHASHSEED``), and
     those defined by the operating system (for example on Unix,
     ``LD_LIBRARY_PATH`` and locale-related variables).
   * Redirection and terminal settings.
   * The versions of libraries imported directly by ``setup.py``,
     such as ``setuptools`` and ``pkg_resources``.
   * Whether the build is performed in a ``virtualenv`` environment.
   * Which other Python packages are installed in the system and in
     any ``virtualenv`` environment. (Potentially, *any* installed
     library could have side effects on the build even if it is not
     a dependency of Tahoe-LAFS.)

   .. _`#1302`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/1302

The build process uses a library called ``setuptools`` to satisfy
any needed dependencies. By default, missing dependencies are
downloaded from the Internet. Since Internet access is essentially
impossible to make deterministic, this behaviour would need to be
disabled in order to achieve repeatable builds. For the purpose
of this analysis, we will assume that all dependencies are available
locally (for example by using a SUMO build), and that downloads from
the Internet are prevented. (It may be desirable to block Internet
access by the build process rather than relying only on documented
``setuptools`` behaviour.) Tahoe-LAFS ticket `#2055`_ ("Building
tahoe safely is non-trivial") documents our attempts to fix these
problems.

The following ``setuptools`` bugs may complicate reasoning about
which dependencies are used:

* `#1258`_ ("having Tahoe or any dependency installed in
  ``site-packages`` (or any other shared directory) can cause us
  to run or test the wrong code")
* `#1450`_ ("setuptools downloads and builds a correct version
  of a dependency in the install-to-egg step, but then adds a
  different version not satisfying the requirement to
  ``easy_install.pth``")
* `#2306`_ ("why does zetuptoolz build the same dependency more
  than once?")
* `#717`_ ("unnecessary rebuild of dependencies when ``tahoe-deps/``
  is present")

Improvements to Tahoe-LAFS' build process that could mitigate the
effects of these issues and improve testability include:

* `#1346`_ ("desert-island test can pass incorrectly because
  packages are installed")
* `#1504`_ ("allow build ignoring system-installed packages")
* `#1464`_ ("stronger isolation between the Python libraries
  imported by build steps and those used by buildbot")
* `#709`_ ("hard to run against alternate dependencies, e.g.
  trunk version of Foolscap")
* `#2129`_ ("``bin/tahoe debug trial`` runs installed code
  somewhere other than modified source files in ``src/``")
* `#1220`_ ("build/install should be able to refrain from getting
  dependencies")
* `#1248`_ ("move logic for build steps from buildmaster config
  to misc/build_helpers")
* `#2362`_ ("get buildmaster config synced up with the corresponding
  git repo")

.. _`#709`:  https://tahoe-lafs.org/trac/tahoe-lafs/ticket/709
.. _`#717`:  https://tahoe-lafs.org/trac/tahoe-lafs/ticket/717
.. _`#1220`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/1220
.. _`#1248`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/1248
.. _`#1258`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/1258
.. _`#1346`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/1346
.. _`#1450`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/1450
.. _`#1464`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/1464
.. _`#1504`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/1504
.. _`#2055`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2055
.. _`#2129`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2129
.. _`#2306`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2306
.. _`#2362`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2362

Our current plan to switch to a build process using ``pip`` is
documented in `#2077`_ ("pip packaging plan").

.. _`#2077`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2077

Without underestimating the difficulty in doing so, suppose for the
sake of argument that predictable versions of Tahoe-LAFS and all of
its direct and indirect dependencies are used in the build. That is,
the auditor and end-user are using the same versions of all dependent
libraries.

Note that some dependencies are pure Python while others depend on
C/C++ extension modules.

The following additional sources of nondeterminism may be present:

* The order in which dependencies are built.
* Whether C/C++ extensions are "embedded" or dynamically linked
  against an installed system library (this is relevant for
  Crypto++ and OpenSSL).
* The buildchain for C/C++ code (which includes many non-obvious
  dependencies).
* The build process for C/C++ code may be nondeterministic, for
  example depending on timestamps, permissions, and similar.
* distutils properties that affect compilation
  [need reference]
* Environment variables that affect compilation of Python code.
* Execution of Python code for building Tahoe-LAFS or a dependency
  (for example the order of ``dict`` elements etc.)
* Possible reliance on entropy sources (e.g. ``os.urandom``).
* Side effects of operations such as running tests, which may
  write to files under the build directory.

Sources of nondeterminism specific to the Mac OS X and Windows packages
are discussed in those sections below.


====================
 Mac OS X packaging
====================

This OS X packaging phase has four steps:

#. Solicit a volunteer to provide an OS X Buildbot slave:

   * `#2303`_: ("Acquire and configure a dedicated OSX build slave.")

   A volunteer has been found and this is planned to be set up within the next
   few weeks.

   .. _`#2303`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2303

#. Implement packaging tests for known OS X-specific issues:

   * `#1006`_: ("Incorrect pycryptopp architecture selected on osx 10.6.")

     This ticket has been closed as it is difficult to reproduce. Also there
     are probably not many installations of OS X 10.6 these days. On newer OS X
     versions, this has not been observed.

   * `#2001`_: ("build binary eggs for macosx-10.9-intel (mavericks)")

   .. _`#1006`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/1006
   .. _`#2001`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2001

   * `#182`_: ("build a .pkg installer for Mac OS X 10.9 Mavericks (intel-x86-64)")
   * `#2304`_: ("Create an OSX automated packaging test which exercises building,
     installing ``pkg`` file.")
   * `#2360`_: ("write and deploy a buildbot step that builds and tests the
     Mac OS X package")

   A make target for building the OS X package has been added. Package tests
   are also added to see if the resultant Python package modules are installed
   in the right directories.

   A draft video of the OS X package configuration and usage has been
   made, and will be posted on the blog once editing has been completed.

   The OS X installer package will be made available for the subsequent
   official releases of Tahoe-LAFS. Currently it builds the version from
   the master branch.

   .. _`#182`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/182
   .. _`#2304`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2304
   .. _`#2360`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2360

Sources of nondeterminism
-------------------------

The OS X package uses the installed version of Python. By default, OS X 10.9
(Mavericks) provides Python 2.7.6. It also provides Twisted 13.2.0,
zope.interface, pyOpenSSL 0.13.1, and (with current security updates)
OpenSSL 0.9.8za, which are dependencies of Tahoe-LAFS; therefore these are
not included in the installer. The installed Python packages may vary as
described earlier.

The system C compiler is used to compile extension modules for pycryptopp
and pycrypto.

The pkgbuild and productbuild tools are used to create the .pkg file.

Therefore, the versions of the system C compiler, pkgbuild, and productbuild
are sources of nondeterminism. The version of setuptools used to build the
package is fixed since it is bundled with the Tahoe-LAFS source.

The C compiler, setuptools, pkgbuild, productbuild, and any tools that they
depend on, may also operate nondeterministically.

Other security issues
---------------------

At the time of writing, the version of OpenSSL provided by OS X Mavericks is
OpenSSL 0.9.8za. This is subject to a `number of security flaws`_ including the
`POODLE`_ flaw. We intend to update the installer (and Tahoe-LAFS built as
described in `quickstart.rst`_) to use a version of pyOpenSSL that links against
a more recent embedded copy of the OpenSSL library.

.. _`number of security flaws`: https://www.openssl.org/news/openssl-0.9.8-notes.html
.. _`POODLE`: https://www.openssl.org/~bodo/ssl-poodle.pdf


===================
 Windows packaging
===================

This Windows packaging phase has four steps:

#. Solicit a volunteer to provide a Windows Buildbot slave.
#. Implement packaging tests for known Windows-specific issues:

   * `#1093`_ ("win32 build hell")
   * `#1371`_ ("Windows registry keys for Python file associations may
     have broken permissions, preventing build or installation")

   .. _`#1093`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/1093
   .. _`#1371`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/1371

#. Fix those tickets and verify that source-based or pip-based
   installations work on Windows on the relevant Build-slaves.

   * `#1981`_ ("build binary eggs for Windows x86 (64-bit)")

   .. _`#1981`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/1981

   At this stage announce improved Windows support on the mailing list,
   as for OS X.

#. Create a new build target for a Windows-based package, and develop
   an automated package test in concert with this development.

   * `#195`_: ("build an .exe installer for 64-bit Windows 7 (and preferably 8)")
   * `#2363`_: ("Create a Windows automated packaging test which exercises
     building, installing ``exe`` file.")
   * `#2361`_: ("write and deploy a buildbot step that builds and tests
     the Windows package")

   .. _`#195`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/195
   .. _`#2363`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2363
   .. _`#2361`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2361

Sources of nondeterminism
-------------------------

The Windows installer checks the existing version of Python and installs
a known version if there is no existing version, or if that version
is older than 2.7. In the case where the existing version of Python
is used, the installed Python packages may vary as described earlier.

The Windows package is compiled using Microsoft Visual Studio Express 2008 SP1.
(This somewhat old version is required in order to compile Python extension
modules that link correctly against the Windows build of Python 2.7.)
A number of workarounds are required in order to get Visual Studio to
compile for 64-bit Windows; these are potential sources of nondeterminism
since it is difficult to guarantee that an auditor's machine will be set
up identically to the original build conditions for the Windows package.

The InfoZIP ``zip`` utility is used to generate the self-extracting zip file.

Command-line tools provided with Inkscape are used to convert icon images
to the required format.


.. To render this reStructuredText file into a PDF file, run:
.. rst2pdf openitp-proposal_good-packaging-for-LAFS.rst

.. To render this reStructuredText file into an HTML file, run:
.. F=openitp-proposal_good-packaging-for-LAFS ; rst2html $F.rst > $F.html
