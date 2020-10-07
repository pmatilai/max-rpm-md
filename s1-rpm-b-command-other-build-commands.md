<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-b-command.md)

Chapter 12. **rpmbuild** Command Reference

[Next](ch-rpm-inside.md)

-----

<div class="sect1">

# <span id="s1-rpm-b-command-other-build-commands">Other Build-related Commands</span>

There are two other commands that also perform build-related functions.
However, they do not use the **rpmbuild** command syntax that we've been
studying so far. Instead of specifying the name of the spec file, as
with **rpmbuild**, it's necessary to specify the name of the source
package file.

Why the difference in syntax? The reason has to do with the differing
functions of these commands. Unlike **rpmbuild**, where the name of the
game is to get software packaged into binary and source package files,
these commands use an already-existing source package file as input.
Let's take a look at them:

<div class="sect2">

## <span id="s2-rpm-b-command-recompile-option">**rpmbuild** **--recompile** — What Does it Do?</span>

The **--recompile** option directs RPM to perform the following steps:

  - Install the specified source package file.

  - Unpack the original sources.

  - Build the software.

  - Install the software.

  - Run the tests.

While you might think this sounds a great deal like an install of the
source package file, followed by an **rpmbuild** **-bi**, this is not
entirely the case. Using **--recompile**, the only file required is the
source package file. After the software is built and installed, the only
thing left, other than the newly installed software, is the original
source package file.

The **--recompile** option is normally used when a previously installed
package needs to be recompiled. **--recompile** comes in handy when
software needs to be compiled against a new version of the kernel.

Here's what RPM displays during a **--recompile**:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild --recompile cdplayer-1.0-1.src.rpm
Installing cdplayer-1.0-1.src.rpm
* Package: cdplayer
Executing(%prep):
…
+ exit 0
Executing(%build):
…
+ exit 0
Executing(%install):
…
+ exit 0
Executing(%check):
…
+ exit 0
Executing(%doc):
…
+ exit 0

# 
          </code></pre></td>
</tr>
</tbody>
</table>

The very first line shows RPM installing the source package. After that
are ordinary executions of the **%prep**, **%build**, and **%install**
sections of the spec file.

Since **rpm** **-i** or **rpm** **-U** are not being used to install the
software, the RPM database is not updated during a **--recompile**. This
means that doing a **--recompile** on an already-installed package may
result in problems down the road, when RPM is used to upgrade or verify
the package.

</div>

<div class="sect2">

## <span id="s2-rpm-b-command-rebuild-option">**rpmbuild** **--rebuild** — What Does it Do?</span>

Package builders, particularly those that create packages for multiple
architectures, often need to build their packages starting from the
original sources. The **--rebuild** option does this, starting from a
source package file. Here is the list of steps it performs:

  - Install the specified source package file.

  - Unpack the original sources.

  - Build the software.

  - Install the software.

  - Run the tests.

  - Create a binary package file.

  - Remove the software's build directory tree and run the **%clean**
    script.

Unlike the **--recompile** option, **--rebuild** cleans up after itself.
The other difference between the two commands is the fact that
**--rebuild** also creates a binary package file. The only remnants of a
**--rebuild** are the original source package, the newly installed
software, and a new binary package file.

Package builders find this command especially handy, as it allows them
to create new binary packages using one command, with no additional
cleanups required. There are several times when **--rebuild** is
normally used:

  - When the build environment (eg. compilers, libraries, etc.) has
    changed.

  - When binary packages for a different architecture are to be built.

Here's an example of the **--rebuild** option in action:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild --rebuild cdplayer-1.0-1.src.rpm
Installing cdplayer-1.0-1.src.rpm
* Package: cdplayer
Executing(%prep):
…
+ exit 0
Executing(%build):
…
+ exit 0
Executing(%install):
…
+ exit 0
Executing(%check):
…
+ exit 0
Executing(%doc):
…
+ exit 0
Binary Packaging: cdplayer-1.0-1
…
Executing(%clean):
…
+ exit 0
Executing(--clean):
…
+ exit 0

#
          </code></pre></td>
</tr>
</tbody>
</table>

The very first line shows RPM installing the source package. The lines
after that are ordinary executions of the **%prep**, **%build**,
**%install** and **%check** (if any) sections of the spec file. Next, a
binary package file is created. Finally, the spec file's **%clean**
section (if one exists) is executed. The cleanup of the software's build
directory takes place, just as if the **--clean** option had been
specified.

That completes our overview of the commands used to build packages with
RPM. In the next chapter, we'll look at the various macros that are
available and how they can make life easier for the package builder.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                |                             |                            |
| :----------------------------- | :-------------------------: | -------------------------: |
| [Prev](ch-rpm-b-command.md)  |     [Home](index.md)      | [Next](ch-rpm-inside.md) |
| **rpmbuild** Command Reference | [Up](ch-rpm-b-command.md) |       Inside the Spec File |

</div>
