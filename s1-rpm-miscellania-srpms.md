<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-miscellania-rpm2cpio.md)

Chapter 8. Miscellanea

[Next](p5206.md)

-----

<div class="sect1">

# <span id="s1-rpm-miscellania-srpms">Source Package Files and How To Use Them</span>

One day, you may run across a package file with a name similar to the
following:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>etcskel-1.0-3.src.rpm
        </code></pre></td>
</tr>
</tbody>
</table>

Notice the `src`. Is that a new kind of computer? If you use RPM on an
Intel-based computer, you'd normally expect to find `i386` there. Maybe
someone messed up the name of the file. Well, we know that the **file**
command can display information about a package file, even if the
filename has been changed. We've used it before to figure out what
package a file contains:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># file foo.bar
foo.bar: RPM v2 bin i386 eject-1.2-2

#
        </code></pre></td>
</tr>
</tbody>
</table>

In this example, `foo.bar` is an RPM version 2 file, containing an
executable package — hence, the "`bin`" — built for Intel processors —
the "`i386`". The package is eject version 1.2, release 2.

Let's try the **file** command on this mystery file and see what we can
find out about it:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># file etcskel-1.0-3.src.rpm
etcskel-1.0-3.src.rpm: RPM v2 src i386 etcskel-1.0-3

#
        </code></pre></td>
</tr>
</tbody>
</table>

Well, it's a package file all right — for version 1.0, release 3 of the
`etcskel` package. It's in RPM version 2 format, and built for
Intel-based systems. But what does the "`src`" mean?

<div class="sect2">

## <span id="s2-rpm-miscellania-srpms-source-code">A gentle introduction to source code</span>

This package file contains not the executable, or "binary", files that a
normal package contains, but rather the "source" files required to
create those binaries. When programmers create a new program, they write
the instructions, often called "code", in one or more files. The source
code is then compiled into a binary that can be executed by the
computer.

As part of the process of building package files (a process we cover in
great detail in the second half of this book), two types of package
files are created:

1.  The binary, or executable, package file

2.  The source package file

The source package contains everything needed to recreate not only the
programs and associated files that are contained in the binary package
file, but the binary and source package files themselves.

</div>

<div class="sect2">

## <span id="s2-rpm-miscellania-srpms-info-needed">Do you *really* need more information than this?</span>

The following discussion is going to get rather technical. Unless you're
the type of person who likes to take other people's code and modify it,
chances are you won't need much more information than this. But if
you're still interested, let's explore further.

</div>

<div class="sect2">

## <span id="s2-rpm-miscellania-srpms-what-can-i-do">So what can I do with it?</span>

In the case of source package files, one of the things that can be done
with them is that they can be installed. Let's try an install of a
source package:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -i cdp-0.33-3.src.rpm
#
          </code></pre></td>
</tr>
</tbody>
</table>

Well that doesn't tell us very much and, take our word for it, adding
**-v** doesn't improve the situation appreciably. Let's haul out the big
guns and try **-vv**:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -ivv cdp-0.33-3.src.rpm
D: installing cdp-0.33-3.src.rpm
Installing cdp-0.33-3.src.rpm
D: package is a source package major = 2
D: installing a source package
D: sources in: ///usr/src/redhat/SOURCES
D: spec file in: ///usr/src/redhat/SPECS
D: file &quot;cdp-0.33-cdplay.patch&quot; complete
D: file &quot;cdp-0.33-fsstnd.patch&quot; complete
D: file &quot;cdp-0.33.spec&quot; complete
D: file &quot;cdp-0.33.tgz&quot; complete
D: renaming ///usr/src/redhat/SOURCES/cdp-0.33.spec to ///usr/src/redhat/SPECS/cdp-0.33.spec

#
          </code></pre></td>
</tr>
</tbody>
</table>

What does this output tell us? Well, RPM recognizes that the file is a
source package. It mentions that sources (we know what *they* are) are
in `/usr/src/redhat/SOURCES`. Let's take a look:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -al /usr/src/redhat/SOURCES/
-rw-rw-r--   1 root     root          364 Apr 24 22:35 cdp-0.33-cdplay.patch
-rw-r--r--   1 root     root          916 Jan  8 12:07 cdp-0.33-fsstnd.patch
-rw-r--r--   1 root     root       148916 Nov 10  1995 cdp-0.33.tgz

#
          </code></pre></td>
</tr>
</tbody>
</table>

There are some files that seem to be related to `cdp` there. The two
files ending with "`.patch`" are patches to the source. RPM permits
patches to be processed when building binary packages. The patches are
bundled along with the original, unmodified sources in the source
package.

The last file is a gzipped tar file. If you've gotten software off the
Internet, you're probably familiar with **tar** files, gzipped or not.
If we look inside the file, we should see all the usual kinds of things:
`README` files, a `Makefile` or two, and some source code:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># tar ztf cdp-0.33.tgz
cdp-0.33/COPYING
cdp-0.33/ChangeLog
cdp-0.33/INSTALL
cdp-0.33/Makefile
cdp-0.33/README
cdp-0.33/cdp
cdp-0.33/cdp-0.33.lsm
cdp-0.33/cdp.1
cdp-0.33/cdp.1.Z
cdp-0.33/cdp.c
cdp-0.33/cdp.h

#
          </code></pre></td>
</tr>
</tbody>
</table>

There's more, but you get the idea. OK, so there are the sources. But
what is that "spec" file mentioned in the output? It mentions something
about "`/usr/src/redhat/SPECS`", so let's see what we have in that
directory:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -al /usr/src/redhat/SPECS
-rw-r--r--   1 root     root          397 Apr 24 22:36 cdp-0.33.spec

          </code></pre></td>
</tr>
</tbody>
</table>

Without making a long story too short, a spec file contains information
used by RPM to create the binary and source packages. Using the spec
file, RPM:

  - Unpacks the sources.

  - Applies patches (if any exist).

  - Builds the software.

  - Creates the binary package file.

  - Creates the source package file.

  - Cleans up after itself.

The neatest part of this is that RPM does this all automatically, under
the control of the spec file. That's about all we're going to say about
how RPM builds packages. For more information, please refer to the
second half of this book.

</div>

<div class="sect2">

## <span id="s2-rpm-miscellania-stick-with-us">Stick with us\!</span>

As we've noted several times, we'll be covering the entire subject of
building packages with RPM, in the second half of the book. Be
forewarned, however: Package building, while straightforward, is not a
task for people new to programming. But if you've written a program or
two, you'll probably find RPM's package building a piece of cake.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                          |                               |                                                                           |
| :--------------------------------------- | :---------------------------: | ------------------------------------------------------------------------: |
| [Prev](s1-rpm-miscellania-rpm2cpio.md) |      [Home](index.md)       |                                                        [Next](p5206.md) |
| Using **rpm2cpio**                       | [Up](ch-rpm-miscellania.md) | RPM and Developers — How to Distribute Your Software More Easily With RPM |

</div>
