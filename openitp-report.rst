
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
number of people, but it is currently not available as a precompiled
package for Windows or Macintosh users.

.. _Tahoe-LAFS: https://Tahoe-LAFS.org

A related issue is that there is currently no way for a user to
*verify* that a binary package they acquire was generated from the
alleged source code. Such *verifiable builds* are important for users
to gain the security benefits from source-code-oriented security
auditing performed by others.

We propose:

1. to extend the Tahoe-LAFS packaging to reach Windows and Macintosh
   users and Python programmers,
2. to do so in a transparent, and reproducible way, and
3. to document how this approach can move us toward a world of
   verifiable builds.

This project is projected to span 8 weeks, starting in April and ending
in June, with a combined cost of $30,000.

============
 Tahoe-LAFS
============

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

.. _report by the Comprehensive National Cybersecurity Initiative: http://www.cyber.st.dhs.gov/docs/National_Cyber_Leap_Year_Summit_2009_Co-Chairs_Report.pdf

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

1. A source code distribution, which users then have to build
   themselves, following `instructions`_ included in the source
   distribution and posted on the Tahoe-LAFS.org web site.

.. _instructions: https://tahoe-lafs.org/trac/tahoe-lafs/browser/trunk/docs/quickstart.rst

2. `Binary packages`_ for each of several Linux and NetBSD operating
   systems. These binary packages are built and distributed by the
   operating system maintainers themselves rather than by the
   Tahoe-LAFS developers.

.. _Binary packages: https://tahoe-lafs.org/trac/tahoe-lafs/wiki/Installation

We propose to extend this to include a third and fourth kind of
distribution, without interfering with the maintenance and improvement
of the first two kinds:

3. Binary packages for Macintosh and Windows, built and distributed by
   the Tahoe-LAFS developers themselves.

   This would make it possible for Macintosh and Windows users to
   install and use LAFS without building it themselves.

4. A Python package that can be automatically and securely installed
   from source code using the standard Python tool "pip". The source
   code will (of course) be maintained by the Tahoe-LAFS developers,
   but mirrors of the source code will be hosted by the standard
   `Python package repository`_.

   This will make it possible for Python programmers to install
   Tahoe-LAFS using their standard tools, and will facilitate
   making Tahoe-LAFS a dependency of their own Python packages.
   It will also be a part of the technical strategy for implementing
   the Windows and Macintosh installers.

   This will also make it easy to install Tahoe-LAFS on platforms that
   are not served by any of the above packages, such as an embedded
   device running an alternative Linux distribution, not covered by
   any of the packages above, or a system running an alternative
   operating system.

.. _`Python package repository`: https://pypi.python.org

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
this project, is to enable end-users to *verify* that the package of
Tahoe-LAFS that they are using was generated from the exact same
source code that a security auditor examined.

In order to explain the verifiable build concept, consider this simple
diagram::

    distributor: source code ➾ binary package → user

Here we use “➾” to mean “build” — the process that produces usable
packages out of source code.

Now consider a security auditor who does a source-code-based
examination (as opposed to binary-based, which is called “reverse
engineering”). This security auditor will start with the source code,
and examine it for vulnerabilities or backdoors.::

    auditor: source code → security audit

How can the user who receives a binary package know whether that
package was built from the source that the auditor examined?

The “verifiable build” approach attempts to answer that question by
having the security auditor perform the “source code ➾ binary package”
on their own trusted system, and then taking a fingerprint (secure
hash) of the resulting binary package::

   auditor: source code ➾ binary package
   auditor: binary package → generate fingerprint

The auditor then publishes that fingerprint along with their report
about their security audit. Users who receive the binary package can
take a fingerprint of that package and compare it to the fingerprint
in the published report.::

   distributor: source code ➾ binary package → user
   user: binary package → check fingerprint

This approach can work only if the ➾ operation performed by the
distributor results in a bytewise-identical binary as the ➾ operation
performed by the security auditor.

Here is a news article from LWN.net about the concept of verifiable
builds (prompted in part by an open letter that we wrote): `“Security
software verifiability”`_. Here is a `post on the tahoe-dev mailing
list`_ and an `issue tracker ticket`_ about our desire to have
verifiable builds for Tahoe-LAFS.

.. _“Security software verifiability”: https://lwn.net/Articles/564263/
.. _post on the tahoe-dev mailing list: https://tahoe-lafs.org/pipermail/tahoe-dev/2013-August/008684.html
.. _issue tracker ticket: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2057

An example of this approach is the recent ad hoc `reproduction of the
TrueCrypt Windows binaries`_.

.. _reproduction of the TrueCrypt Windows binaries: https://madiba.encs.concordia.ca/~x_decarn/truecrypt-binaries-analysis/

There are several reasons why we will not be able to completely solve
this problem in the scope of this project:

1. Users sometimes prefer to use software as packaged by their
   operating system distribution, rather than as packaged by the
   upstream maintainers. In this case, the *verifiable build property*
   can be achieved only by the operating system maintainers choosing
   to opt-in to the verifiable build process.

   The Debian project has already `begun working on this`_, following
   the lead of the `Tor`_ and `Bitcoin`_ projects, which have
   pioneered reproducible builds.

   .. _begun working on this: https://wiki.debian.org/ReproducibleBuilds
   .. _Tor: https://blog.torproject.org/category/tags/deterministic-builds
   .. _Bitcoin: https://en.bitcoin.it/wiki/Release_process

   One deliverable from this project, therefore, will be to document best
   practices for upstream software developers (us) to cooperate with
   distribution maintainers (starting with Debian), to facilitate
   verifiable builds.

2. Verifiable builds would be a lot more valuable to users if many
   software packages, many security auditors, many distributors
   (e.g. Linux distributions), and many users all use a comparable
   process. If there are only a few software projects generating
   verifiable builds, and if software projects tend to generate
   verifiable builds in bespoke and non-reusable ways, then security
   auditors are less likely to use this approach, and users are less
   likely to verify packages.

   Therefore one desirable outcome of this project would be to document
   and evangelize these practices to other developers of open-source
   software.

====================
 Mac OS X packaging
====================

This OS X packaging phase has four steps:

#. Solicit a volunteer to provide an OS X Buildbot slave.
#. Implement packaging tests for known OS X-specific issues:

   * `Ticket 1006`_: *Incorrect pycryptopp architecture selected on osx 10.6.*

     This ticket has been closed as it is difficult to reproduce. Also there
     are probably not many installations of OS X 10.6 these days. On newer OS X
     versions, this has not been observed..

   * `Ticket 2001`_: *build binary eggs for macosx-10.8-intel (osx mountain lion)*

   .. _`Ticket 1006`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/1006
   .. _`Ticket 2001`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/2001

   * `Ticket 182`_: *user-friendly installer for Mac -- for my Mom!*

     A make target for building OS X package has been added. Package tests are
     also added to see if the resultant python package modules are installed
     in the right directories.

     A video of the OS X package building, configuration and usage has been
     made and will be posted on the blog.

     The OS X installer package  will be made available for the subsequent
     official releases of Tahoe-LAFS. Currently it builds the version from
     the master branch.

   .. _`Ticket 182`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/182

.. (comment) See this - http://stackoverflow.com/questions/116657/how-do-you-create-an-osx-application-dmg-from-a-python-package

===================
 Windows packaging
===================

This Windows packaging phase has four steps, similar to `Phase 3: Mac OS X packaging`_

#. Solicit a volunteer to provide a Windows Buildbot slave.
#. Implement packaging tests for known Windows-specific issues:

   * `Ticket 1093`_: *win32 build hell*
   * `Ticket 1371`_: *Windows registry keys for Python file associations may have broken permissions, preventing build or installation*

   .. _`Ticket 1093`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/1093
   .. _`Ticket 1371`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/1371

#. Fix those tickets and verify that source-based or pip-based
   installations work on Windows on the relevant Build-slaves.

   At this stage announce improved Windows support on the mailing list,
   as for OS X.

#. Create a new build target for a Windows-based package, and develop
   an automated package test in concert with this development.

   * `Ticket 195`_: *user-friendly installer for Windows -- for my Dad!*

   .. _`Ticket 195`: https://tahoe-lafs.org/trac/tahoe-lafs/ticket/195

.. To render this reStructuredText file into a PDF file, run:
.. rst2pdf openitp-proposal_good-packaging-for-LAFS.rst

.. To render this reStructuredText file into an HTML file, run:
.. F=openitp-proposal_good-packaging-for-LAFS ; rst2html $F.rst > $F.html
