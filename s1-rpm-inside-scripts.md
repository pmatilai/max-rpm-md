<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-inside-tags.md)

Chapter 13. Inside the Spec File

[Next](s1-rpm-inside-macros.md)

-----

<div class="sect1">

# <span id="s1-rpm-inside-scripts">Scripts: RPM's Workhorse</span>

The scripts that RPM uses to control the build process are among the
most varied and interesting parts of the spec file. Many spec files also
contain scripts that perform a variety of tasks whenever the package is
installed or erased.

The start of each script is denoted by a keyword. For example, the
**%build** keyword marks the start of the script RPM will execute when
building the software to be packaged. It should be noted that, in the
strictest sense of the word, these parts of the spec file are not
scripts. For example, they do not start with the traditional invocation
of a shell. However, the contents of each script section are copied into
a file and executed by RPM as a full-fledged script. This is part of the
power of RPM: Anything that can be done in a script can be done by RPM.

Let's start by looking at the scripts used during the build process.

<div class="sect2">

## <span id="s2-rpm-inside-build-time-scripts">Build-time Scripts</span>

The scripts that RPM uses during the building of a package follow the
steps known to every software developer:

  - Unpacking the sources.

  - Building the software.

  - Installing the software.

  - Cleaning up.

Although each of the scripts perform a specific function in the build
process, they share a common environment. Using RPM's **--test** option
[<span class="footnote">\[1\]</span>](#FTN.AEN7910) , we can see the
common portion of each script. In the following example, we've taken the
`cdplayer` package, issued an **rpmbuild** **-ba --test
cdplayer-1.0-1.spec**, and viewed the script files left in RPM's
temporary directory. This section (with the appropriate package-specific
values) is present in every script RPM executes during a build:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#!/bin/sh -e
# Script generated by rpm

RPM_SOURCE_DIR=&quot;/usr/src/redhat/SOURCES&quot;
RPM_BUILD_DIR=&quot;/usr/src/redhat/BUILD&quot;
RPM_DOC_DIR=&quot;/usr/doc&quot;
RPM_OPT_FLAGS=&quot;-O2 -m486 -fno-strength-reduce&quot;
RPM_ARCH=&quot;i386&quot;
RPM_OS=&quot;Linux&quot;
RPM_ROOT_DIR=&quot;/tmp/cdplayer&quot;
RPM_BUILD_ROOT=&quot;/tmp/cdplayer&quot;
RPM_PACKAGE_NAME=&quot;cdplayer&quot;
RPM_PACKAGE_VERSION=&quot;1.0&quot;
RPM_PACKAGE_RELEASE=&quot;1&quot;
set -x

umask 022

          </code></pre></td>
</tr>
</tbody>
</table>

As we can see, the script starts with the usual invocation of a shell
(in this case, the Bourne shell). There are no arguments passed to the
script. Next, a number of environment variables are set. Here's a brief
description of each one:

  - `RPM_SOURCE_DIR` — This environment variable gets its value from the
    rpmrc file entry **sourcedir**, which in turn can get part of its
    value from the **topdir** entry. It is the path RPM will prepend to
    the file, specified in the **source** tag line.

  - `RPM_BUILD_DIR` — This variable is based on the **builddir** rpmrc
    file entry, which in turn can get part of its value from the
    **topdir** entry. This environment variable translates to the path
    of RPM's build directory, where most software will be unpacked and
    built.

  - `RPM_DOC_DIR` — The value of this environment variable is based on
    the **defaultdocdir** `rpmrc` file entry. Files marked with the
    **%doc** directive can be installed in a subdirectory of
    **defaultdocdir**. For more information on the **%doc** directive,
    refer to [the Section called *The **%doc**
    Directive*](s1-rpm-inside-files-list-directives.md#s3-rpm-inside-flist-doc-directive).

  - `RPM_OPT_FLAGS` — This environment variable gets its value from the
    **optflags** `rpmrc` file entry. It contains options that can be
    passed on to the build procedures of the software being packaged.
    Normally this means either a configuration script or the **make**
    command itself.

  - `RPM_ARCH` — As one might infer from the example above, this
    environment variable contains a string describing the build system's
    architecture.

  - `RPM_OS` — This one contains the name of the build system's
    operating system.

  - `RPM_BUILD_ROOT` — This environment variable is used to hold the
    "build root", into which the newly built software will be installed.
    If no explicit build root has been specified (either by command line
    option, spec file tag line, or rpmrc file entry), this variable will
    be null.

  - `RPM_PACKAGE_NAME` — This environment variable gets its value from
    the **name** tag line in the package's spec file. It contains the
    name of the software being packaged.

  - `RPM_PACKAGE_VERSION` — The **version** tag line is the source of
    this variable's translation. Predictably, this environment variable
    contains the software's version number.

  - `RPM_PACKAGE_RELEASE` — This environment variable contains the
    package's release number. Its value is obtained from the **release**
    tag line in the spec file.

All of these environment variables are set for your use, to make it
easier to write scripts that will do the right thing even if the build
environment changes.

The script also sets an option that causes the shell to print out each
command, complete with expanded arguments. Finally, the default
permissions are set. Past this point, the scripts differ. Let's look at
the scripts in the order they are executed.

<div class="sect3">

### <span id="s3-rpm-inside-prep-script">The **%prep** Script</span>

The **%prep** script is the first script RPM executes during a build.
Prior to the **%prep** script, RPM has performed preliminary consistency
checks, such as whether the spec file's **source** tag points to files
that actually exist. Just prior to passing control over to the **%prep**
script's contents, RPM changes directory into RPM's build area, which,
by default, is `/usr/src/redhat/BUILD`.

At that point, it is the responsibility of the **%prep** script to:

  - Create the top-level build directory.

  - Unpack the original sources into the build directory.

  - Apply patches to the sources, if necessary.

  - Perform any other actions required to get the sources in a
    ready-to-build state.

The first three items on this list are common to the vast majority of
all software being packaged. Because of this, RPM has two macros that
greatly simplify these routine functions. More information on RPM's
**%setup** and **%patch** macros can be found in [the Section called
*Macros: Helpful Shorthand for Package
Builders*](s1-rpm-inside-macros.md).

The last item on the list can include creating directories or anything
else required to get the sources in a ready-to-build state. As a result,
a **%prep** script can range from one line invoking a single **%setup**
macro, to many lines of tricky shell programming.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-build-script">The **%build** Script</span>

The **%build** script picks up where the **%prep** script left off. Once
the **%prep** script has gotten everything ready for the build, the
**%build** script is usually somewhat anti-climactic — normally invoking
**make**, maybe a configuration script, and little else.

Like **%prep** before it, the **%build** script has the same assortment
of environment variables to draw on. Also, like **%prep**, **%build**
changes directory into the software's top-level build directory (located
in `RPM_BUILD_DIR`, or usually called **`<name>`-`<version>`**).

Unlike **%prep**, there are no macros available for use in the
**%build** script. The reason is simple: Either the commands required to
build the software are simple (such as a single **make** command), or
they are so unique that a macro wouldn't make it easier to write the
script.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-install-script">The **%install** Script</span>

The environment in which the **%install** script executes is identical
to the other scripts. Like the other scripts, the **%install** script's
working directory is set to the software's top-level directory.

As the name implies, it is this script's responsibility to do whatever
is necessary to actually install the newly built software. In most
cases, this means a single **make install** command, or a few commands
to copy files and create directories.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-check-script">The **%check** Script</span>

The environment in which the **%check** script executes is identical to
the other scripts. Like the other scripts, the **%check** script's
working directory is set to the software's top-level directory.

This script's primary function is to run the test suite of the built
software to ensure that the binaries work correctly. Some typical
commands to run in this script are **make test** or **make check**.

The **%check** script is available in RPM version 4.2 and newer.
[<span class="footnote">\[2\]</span>](#FTN.AEN8053)

</div>

<div class="sect3">

### <span id="s3-rpm-inside-clean-script">The **%clean** Script</span>

The **%clean** script, as the name implies, is used to clean up the
software's build directory tree. RPM normally does this for you, but in
certain cases (most notably in those packages that use a build root)
you'll need to include a **%clean** script.

As usual, the **%clean** script has the same set of environment
variables as the other scripts we've covered here. Since a **%clean**
script is normally used when the package is built in a build root, the
`RPM_BUILD_ROOT` environment variable is particularly useful. In many
cases, a simple

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>rm -rf $RPM_BUILD_ROOT

            </code></pre></td>
</tr>
</tbody>
</table>

will suffice. [<span class="footnote">\[3\]</span>](#FTN.AEN8074)

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-inside-erase-time-scripts">Install/Erase-time Scripts</span>

The other type of scripts that are present in the spec file are those
that are only used when the package is either installed or erased. There
are four scripts, each one meant to be executed at different times
during the life of a package:

  - Before installation.

  - After installation.

  - Before erasure.

  - After erasure.

Unlike the build-time scripts, there is little in the way of environment
variables for these scripts. The only environment variable available is
`RPM_INSTALL_PREFIX`, and that is only set if the package uses an
installation prefix.

Unlike the build-time scripts, there *is* an argument defined. The sole
argument to these scripts, is a number representing the number of
instances of the package currently installed on the system, *after* the
current package has been installed or erased. Sound tricky? It really
isn't. Here's an example:

Assume that a package, called `blather-1.0`, is being installed. No
previous versions of `blather` have been installed. Since the software
is being installed, only the **%pre** and **%post** scripts are
executed. The argument passed to these scripts will be 1, since the the
number of `blather` packages installed is 1.
[<span class="footnote">\[4\]</span>](#FTN.AEN8113)

Continuing our example, a new version of the `blather` package, version
1.3, is now available. Clearly it's time to upgrade. What will the
scripts' values be during the upgrade? As `blather-1.3` is installing,
its **%pre** and **%post** scripts will have an argument equal to 2 (1
for version 1.0 already installed, plus 1 for version 1.3 being
installed). As the final part of the upgrade, it's then time to erase
`blather` version 1.0. As the package is being removed, its **%preun**
and **%postun** scripts are executed. Since there will be only one
`blather` package (version 1.3) installed after version 1.0 is erased,
the argument passed to version 1.0's scripts is 1.

To finally bring an end to this example, we've decided to erase
`blather` 1.3. We just don't need it anymore. As the package is being
erased, its **%preun** and **%postun** scripts will be executed. Since
there will be no `blather` packages installed once the erase completes,
the argument passed to the scripts is 0.

With all that said, of what possible use would this argument be? Well,
it has two very interesting properties:

1.  When the first version of a package is installed, its **%pre** and
    **%post** scripts will be passed an argument equal to 1.

2.  When the last version of a package is erased, its **%preun** and
    **%postun** scripts will be passed an argument equal to 0.

Based on these properties, it's trivial to write an install-time script
that can take certain actions in specific circumstances. Usually, the
argument is used in the **%preun** or **%postun** scripts to perform a
special task when the last instance of a package is being erased.

What is normally done during these scripts? The exact tasks may vary,
but in general, the tasks are any that need to be performed at these
points in the package's existence. One very common task is to run
**ldconfig** when shared libraries are installed or removed. But that's
not the only use for these scripts. It's even possible to use the
scripts to perform tests to ensure the package install/erasure should
proceed.

Since each of these scripts will be executing on whatever system
installs the package, it's necessary to choose the script's choice of
tools carefully. Unless you're sure a given program is going to be
available on *all* the systems that could possibly install your package,
you should not use it in these scripts.

<div class="sect3">

### <span id="s3-rpm-inside-pre-script">The **%pre** Script</span>

The **%pre** script executes just before the package is to be installed.
It is the rare package that requires anything to be done prior to
installation; none of the 350 packages that comprise Red Hat Linux Linux
4.0 make use of it.

<div class="sect4">

#### <span id="s4-rpm-inside-post-script">The **%post** Script</span>

The **%post** script executes after the package has been installed. One
of the most popular reasons a **%post** script is needed is to run
**ldconfig** to update the list of available shared libraries after a
new one has been installed. Of course, other functions can be performed
in a **%post** script. For example, packages that install shells use the
**%post** script to add the shell name to `/etc/shells`.

If a package uses a **%post** script to perform some function, quite
often it will include a **%postun** script that performs the inverse of
the **%post** script, after the package has been removed.

</div>

</div>

<div class="sect3">

### <span id="s3-rpm-inside-preun-script">The **%preun** Script</span>

If there's a time when your package needs to have one last look around
before the user erases it, the place to do it is in the **%preun**
script. Anything that a package needs to do immediately prior to RPM
taking any action to erase the package, can be done here.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-postun-script">The **%postun** Script</span>

The **%postun** script executes after the package has been removed. It
is the last chance for a package to clean up after itself. Quite often,
**%postun** scripts are used to run **ldconfig** to remove newly erased
shared libraries from `ld.so.cache`.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-inside-verifyscript-script">Verification-Time Script — The **%verifyscript** Script</span>

The **%verifyscript** executes whenever the installed package is
verified by RPM's verification command. The contents of this script is
entirely up to the package builder, but in general the script should do
whatever is necessary to verify the package's proper installation. Since
RPM automatically verifies the existence of a package's files, along
with other file attributes, the **%verifyscript** should concentrate on
different aspects of the package's installation. For example, the script
may ensure that certain configuration files contain the proper
information for the package being verified:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>for n in ash bsh; do
    echo -n &quot;Looking for $n in /etc/shells... &quot;
    if ! grep &quot;^/bin/${n}\$&quot; /etc/shells &gt; /dev/null; then
        echo &quot;missing&quot;
        echo &quot;${n} missing from /etc/shells&quot; &gt;&amp;2
    else
        echo &quot;found&quot;
    fi
done

          </code></pre></td>
</tr>
</tbody>
</table>

In this script, the config file `/etc/shells`, is checked to ensure that
it has entries for the shells provided by this package.

It is worth noting that the script sends informational and error
messages to stdout, and error messages only to stderr. Normally RPM will
only display error output from a verification script; the output sent to
stdout is only displayed when the verification is run in verbose mode.

</div>

</div>

### Notes

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><a href="s1-rpm-inside-scripts.md#AEN7910" id="FTN.AEN7910"><span class="footnote">[1]</span></a></td>
<td><p>Described in <a href="ch-rpm-b-command.md#s2-rpm-b-command-test-option">the Section called <em><strong>--test</strong> — Create, Save Build Scripts For Review</em> in Chapter 12</a>.</p></td>
</tr>
<tr class="even">
<td><a href="s1-rpm-inside-scripts.md#AEN8053" id="FTN.AEN8053"><span class="footnote">[2]</span></a></td>
<td><p>One popular hack to make spec files containing the <strong>%check</strong> script "work" with RPM versions older than 4.2 roughly similarly as in newer versions is to include it immediately after the <strong>%install</strong> script in the spec file and append "|| :" to it, like:</p>
<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%check || :

                </code></pre></td>
</tr>
</tbody>
</table></td>
</tr>
<tr class="odd">
<td><a href="s1-rpm-inside-scripts.md#AEN8074" id="FTN.AEN8074"><span class="footnote">[3]</span></a></td>
<td><p>Keep in mind that this command in a <strong>%clean</strong> script can wreak havoc if used with a build root of, say, <code class="filename">/</code>. <a href="ch-rpm-b-command.md#s3-rpm-b-command-buildroot-warning">the Section called <em>Using <strong>--buildroot</strong> Can Bite You!</em> in Chapter 12</a> discusses this in more detail.</p></td>
</tr>
<tr class="even">
<td><a href="s1-rpm-inside-scripts.md#AEN8113" id="FTN.AEN8113"><span class="footnote">[4]</span></a></td>
<td><p>Or it will be 1, once the package is completely installed. Remember, the number is based on the number of packages installed <em>after</em> the current package's install or erase has completed.</p></td>
</tr>
</tbody>
</table>

<div class="NAVFOOTER">

-----

|                                 |                          |                                                |
| :------------------------------ | :----------------------: | ---------------------------------------------: |
| [Prev](s1-rpm-inside-tags.md) |    [Home](index.md)    |              [Next](s1-rpm-inside-macros.md) |
| Tags: Data Definitions          | [Up](ch-rpm-inside.md) | Macros: Helpful Shorthand for Package Builders |

</div>
