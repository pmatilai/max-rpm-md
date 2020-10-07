<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-intro-to-rpm.html)

Chapter 1. An Introduction to Package Management

[Next](s1-intro-to-rpm-rpm-design-goals.html)

-----

<div class="sect1">

# <span id="s1-intro-to-rpm-package-management-how">Package Management: How to Do It?</span>

Well, all that sounds great — easy install, upgrade, and deletion of
packages; getting package information presented several different ways;
making sure packages are installed correctly; and even tracking changes
to config files. But how do you do it?

As mentioned above, the obvious answer is to let the computer do it.
Many groups have tried to create package management software. There are
two basic approaches:

1.  Some package management systems concentrate on the specific steps
    required to manipulate a package.

2.  Other package management systems take a different approach, keeping
    track of the files on the system and manipulating packages by
    concentrating on the files involved.

Each approach has its good and bad points. In the first method, it's
easy to install new packages, somewhat difficult to remove old ones, and
almost impossible to obtain any meaningful information about installed
packages.

The second method makes it easy to obtain information about installed
packages, and fairly easy to install and remove packages. The main
problem using this method is that there may not be a well-defined way to
execute any commands required during the installation or removal
process.

In practice, no package management system uses one approach or the other
— all are a mixture of the two. The exact mix and design goals will
dictate how well a particular package management system meets the needs
of the people using it. At the time Red Hat started work on their Linux
distribution, there were a number of package management systems in use,
each with a different approach to making package management easier.

<div class="sect2">

## <span id="s2-intro-to-rpm-rpm-ancestors">Ancestors of RPM</span>

Since this is a book on the Red Hat Package Manager, a good way to see
what RPM is all about is to look at the package management software that
preceded RPM.

<div class="sect3">

### <span id="s3-intro-to-rpm-rpp">RPP</span>

RPP was used in the first Red Hat Linux distributions. Many of RPP's
features would be recognizable to anyone who has worked with RPM. Some
of these innovative features are:

  - Simple, one command installation and uninstallation of packages.

  - Scripts that can run before and after installation and
    uninstallation of packages.

  - Package verification. The files of individual packages can be
    checked to see that they haven't been modified since they were
    installed.

  - Powerful querying. The database of packages can be queried for
    information about installed packages, including file lists,
    descriptions and more.

While RPP possessed several of the features that were important enough
to continue on as parts of RPM today, it had some weaknesses, too:

  - It didn't use "pristine sources". Every program is made up of
    programming language statements stored in files. This source code is
    later translated into a binary language that the computer can
    execute. In the case of RPP, its packages were based on source code
    that had been modified specifically for RPP, hence the sources
    weren't pristine. This is a bad idea for a number of fairly
    technical reasons. Not using pristine sources made it difficult for
    package developers to keep things straight, particularly if they
    were building hundreds of different packages.

  - It couldn't guarantee executables were built from packaged sources.
    The process of building a package for RPP was such that there was no
    way to ensure the executable programs were built from the source
    code contained in an RPP source package. Once again, this was a
    problem for the package builder, especially those who had large
    numbers of packages to build.

  - It had no support for multiple architectures. As people started
    using RPP, it became obvious that the package managers that were
    unable to simplify the process of building packages for more than
    one architecture, or type of computer, were going to be at a
    disadvantage. This was a problem, particularly for Red Hat, as they
    were starting to look at the possibility of creating Linux
    distributions for other architectures, such as the Digital Alpha.

Even with these problems, RPP was one of the things that made the first
Red Hat Linux distributions unique. Its ability to simplify the process
of installing software was a real boon to many of Red Hat's customers,
particularly those with little experience in Linux.

</div>

<div class="sect3">

### <span id="s3-intro-to-rpm-pms">PMS</span>

While Red Hat was busy with RPP, another group of Linux devotees were
hard at work with their package management system. Known as PMS, its
development, lead by Rik Faith, attacked the problem of package
management from a slightly different viewpoint.

Like RPP, PMS was used to package a Linux distribution. This
distribution was known as the BOGUS distribution, and all the software
in it was built from original unmodified sources. Any changes that were
required were patched in during the processing of building the software.
This is the concept of "pristine sources" and is PMS's most important
contribution to RPM. The importance of pristine sources can not be
overstated. It allows the packager to quickly release new version of
software, and to immediately see what changes were made to the software.

The chief disadvantages of PMS were weak querying ability, no package
verification, no multiple architecture support, and poor database
design.

</div>

<div class="sect3">

### <span id="s3-intro-to-rpm-pm">PM</span>

Later, Rik Faith and Doug Hoffman, working under contract for Red Hat,
produced PM. The design combined all the important features of RPP and
PM, including one command installation and uninstallation, scripts run
before and after installation and uninstallation, package verification,
advanced querying, and pristine sources. However it retained RPP's and
PM's chief disadvantages: weak database design and no support for
multiple architectures.

PM was very close to a viable package management system, but it wasn't
quite ready for prime time. It was never used in a commercially
available product.

</div>

<div class="sect3">

### <span id="s3-intro-to-rpm-rpmv1">RPM Version 1</span>

With two major forays into package management behind them, Marc Ewing
and Erik Troan went to work on a third attempt. This one would be called
the Red Hat Package Manager, or RPM.

Although it built on the experiences of PM, PMS, and RPP, RPM was quite
different under the hood. Written in the Perl programming language for
fast development, the creation of RPM version 1 focused on addressing
the flaws of its ancestors. In some cases, the flaws were eliminated,
while in others, the problems remained.

Some of the successes of RPM version 1 were:

  - Automatic handling of configuration files. The contents of config
    files are often changed from what they were in the original package,
    making it hard for a package manager to know how a particular config
    file should be handled during installs, upgrades, and erasures. PM
    made an attempt at config file handling, but in RPM it was improved
    further. In many respects, this feature is the key to RPM's power
    and flexibility.

  - Ease of rebuilding large numbers of packages. By making it easy for
    people who were trying to create a Linux distribution consisting of
    several hundred packages, RPM was a step in the right direction.

  - It was easy to use. Many of the concepts used in RPP had withstood
    the test of time and were used in RPM. For instance, the ability to
    verify the installation of a package was one of the features that
    set RPP apart. It was adapted and expanded in RPM version 1.

But RPM version 1 wasn't perfect. There were a number of flaws, some of
them major:

  - It was slow. While the use of Perl made RPM's development proceed
    more quickly, it also meant that RPM wouldn't run as quickly as it
    would have, had it been written in C.

  - Its database design was fragile. Unfortunately, under RPM version 1
    it was not unusual for there to be problems with the database. While
    the approach of dedicating a database to package management was a
    good idea, the implementation used in RPM version 1 left a lot to be
    desired.

  - It was big. This is another artifact of using Perl. Normally, RPM's
    size requirements were not an issue, except for one area. When
    performing an initial system install, RPM was run from a small,
    floppy-based system environment. The need to have Perl available
    meant space on the boot floppies was always a problem.

  - It didn't support multiple architectures (types of computers) well.
    The need to have a package manager support more than one *type* of
    computer hadn't been acknowledged before. With RPM version 1, an
    initial stab was taken at the problem, but the implementation was
    incomplete. Nonetheless, RPM had been ported to a number of other
    computer systems. It was becoming obvious that the issue of
    multi-architecture support was not going away and had to be
    addressed.

  - The package file format wasn't extensible. This made it very
    difficult to add functionality, since any change to the file format
    would cause older versions of RPM to break.

Even though their Linux distribution was a success, and RPM was much of
the reason for it, Marc and Erik knew that some changes were going to be
necessary to carry RPM to the next level.

</div>

<div class="sect3">

### <span id="s3-intro-to-rpm-rpmv2">The RPM of Today: Version 2</span>

Looking back on their experiences with RPM version 1, Marc and Erik made
a major change to RPM's design: They rewrote it entirely in C. This did
wonderful things to RPM's speed and size. Querying the database was
quicker now, and there was no need to have Perl around just to do
package management.

In addition, the database format was redesigned to improve both
performance and reliability. Displaying package information can take as
little as a tenth of the time spent in RPM version 1, for example.

Realizing RPM's potential in the non-Linux arena, they also created
rpmlib, a library of RPM routines that allow the use of RPM
functionality in other programs. RPM's ability to function on more than
one architecture was also enhanced. Finally, the package file format was
made more extensible, clearing the way for future enhancements to RPM.

So is RPM perfect? No program can ever reach perfection, and RPM is no
exception. But as a package manager that can run on several different
types of systems, RPM has a lot to offer, and it will only get better.
Let's take a look at the design criteria that drove the development of
RPM.

</div>

</div>

</div>

<div class="NAVFOOTER">

-----

|                                       |                            |                                               |
| :------------------------------------ | :------------------------: | --------------------------------------------: |
| [Prev](ch-intro-to-rpm.html)          |     [Home](index.html)     | [Next](s1-intro-to-rpm-rpm-design-goals.html) |
| An Introduction to Package Management | [Up](ch-intro-to-rpm.html) |                              RPM Design Goals |

</div>
