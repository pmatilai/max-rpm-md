<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-philosophy-summary.md)

[Next](s1-rpm-basics-the-engine.md)

-----

<div class="chapter">

# <span id="ch-rpm-basics"></span>Chapter 10. The Basics of Developing With RPM

Now that we've seen the design philosophy of RPM, let's look at the nuts
and bolts of RPM's build process. Building a package is similar to
compiling code — there are inputs, an engine that does the dirty work,
and outputs.

<div class="sect1">

# <span id="s1-rpm-basics-inputs">The Inputs</span>

There are three different kinds of inputs that are used to drive RPM's
build process. Two of the three inputs are required, and the third,
strictly speaking, is optional. But unless you're packaging your own
code, chances are you'll need it.

<div class="sect2">

## <span id="s2-rpm-basics-sources">The Sources</span>

First and foremost, are the sources. After all, without them, there
wouldn't be much to build\! In the case of packaging someone else's
software, the sources should be kept as the author distributed them,
which usually means a compressed **tar** file. RPM can handle other
archive formats, but a bit more up-front effort is required.

In any case, you should not modify the sources used in the package
building process. If you're a third-party package builder, that means
the sources should be just the way you got them from the author's FTP
site. If it's your own software, the choice is up to you, but you should
consider starting with your mainstream sources.

</div>

<div class="sect2">

## <span id="s2-rpm-basics-patches">The Patches</span>

Why all the emphasis on unmodified sources? Because RPM gives you the
ability to automatically apply patches to them. Usually, the nature of
these patches falls into one of the following categories:

  - The patch addresses an issue specific to the target system. This
    could include changing makefiles to install the application into the
    appropriate directories, or resolving cross-platform conflicts, such
    as replacing BSD system calls with their SYSV counterparts.

  - The patch creates files that are normally created during a
    configuration step in the installation process. Many times, it's
    necessary to either edit configuration files or scripts in order to
    set things up for compilation. In other cases, a configuration
    utility needs to be run before the sources are compiled. In either
    instance, the patches create the environment required for proper
    compilation.

<div class="sect3">

### <span id="s3-rpm-basics-creating-patches">Creating the Patches</span>

While it might sound a bit daunting to take into account the types of
patches outlined above, it's really quite simple. Here's how it's done:

1.  Unpack the sources.

2.  Rename the top-level directory. Make it end with ".orig", for
    example.

3.  Unpack the sources again, leaving the top-level directory name
    unchanged.

The source tree that you created the second time will be the one you'll
use to get the software to build.

If the software builds with no changes required, that's great — you
won't need a patch. But if you had to make any changes, you'll have to
create a set of patches. To do so, simply clean the source directory of
any binaries. Then, issue a recursive **diff** command to compare the
source tree you used for the build, against the original, unmodified
source tree. It's as easy as that\!

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-basics-spec-file">The Spec File</span>

The spec file is at the heart of RPM's packaging building process.
Similar in concept to a makefile, it contains information required by
RPM to build the package, as well as instructions telling RPM *how* to
build it. The spec file also dictates exactly what files are a part of
the package, and where they should be installed.

As you might imagine, with this many responsibilities, the spec file
format can be a bit complex. However, it's broken into several sections,
making it easier to handle. All told, there are eight sections:

<div class="sect3">

### <span id="s3-rpm-basics-spec-preamble">The Preamble</span>

The preamble contains information that will be displayed when users
request information about the package. This would include a description
of the package's function, the version number of the software, and so
on. Also contained in the preamble are lines identifying sources,
patches, and even an icon to be used if the package is manipulated by
graphical interface.

</div>

<div class="sect3">

### <span id="s3-rpm-basics-spec-prep">The Prep Section</span>

The prep section is where the actual work of building a package starts.
As the name implies, this section is where the necessary preparations
are made prior to the actual building of the software. In general, if
anything needs to be done to the sources prior to building the software,
it needs to happen in the prep section. Usually, this boils down to
unpacking the sources.

The contents of this section are an ordinary shell script. However, RPM
does provide two macros to make life easier. One macro can unpack a
compressed **tar** file and **cd** into the source directory. The other
macro easily applies patches to the unpacked sources.

</div>

<div class="sect3">

### <span id="s3-rpm-basics-spec-build">The Build Section</span>

Like the prep section, the build section consists of a shell script. As
you might guess, this section is used to perform whatever commands are
required to actually compile the sources. This section could consist of
a single **make** command, or be more complex if the build process
requires it. Since most software is built today using **make**, there
are no macros available in this section.

</div>

<div class="sect3">

### <span id="s3-rpm-basics-spec-install">The Install Section</span>

Also containing a shell script, the install section is used to perform
the commands required to actually install the software. If the
software's author added an install target in the makefile, this section
might only consist of a **make install** command. Otherwise, you'll need
to add the usual assortment of **cp**, **mv**, or **install** commands
to get the job done.

</div>

<div class="sect3">

### <span id="s3-rpm-basics-spec-scripts">Install and Uninstall Scripts</span>

While the previous sections contained either information required by RPM
to build the package, or the actual commands to do the deed, this
section is different. It consists of scripts that will be run, *on the
user's system*, when the package is actually installed or removed. RPM
can execute a script:

  - Prior to the package being installed.

  - After the package has been installed.

  - Prior to the package being erased.

  - After the package has been erased.

One example of when this capability would be required is when a package
contains shared libraries. In this case, **ldconfig** would need to be
run after the package is installed or erased. As another example, if a
package contains a shell, the file `/etc/shells` would need to be
updated appropriately when the package was installed or erased.

</div>

<div class="sect3">

### <span id="s3-rpm-basics-spec-verify">The Verify Script</span>

This is another script that is executed on the user's system. It is
executed when RPM verifies the package's proper installation. While RPM
does most of the work verifying packages, this script can be used to
verify aspects of the package that are beyond RPM's capabilities.

</div>

<div class="sect3">

### <span id="s3-rpm-basics-spec-clean">The Clean Section</span>

Another script that can be present is a script that can clean things up
after the build. This script is rarely used, since RPM normally does a
good job of clean-up in most build environments.

</div>

<div class="sect3">

### <span id="s3-rpm-basics-spec-file-list">The File List</span>

The last section consists of a list of files that will comprise the
package. Additionally, a number of macros can be used to control file
attributes when installed, as well as to denote which files are
documentation, and which contain configuration information. The file
list is very important — if it is missing, no package will be built.

</div>

</div>

</div>

</div>

<div class="NAVFOOTER">

-----

|                                        |                    |                                       |
| :------------------------------------- | :----------------: | ------------------------------------: |
| [Prev](s1-rpm-philosophy-summary.md) | [Home](index.md) | [Next](s1-rpm-basics-the-engine.md) |
| To Summarize…                          |  [Up](p5206.md)  |                       The Engine: RPM |

</div>
