<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-build-creating-spec-file.md)

Chapter 11. Building Packages: A Simple Example

[Next](s1-rpm-build-when-things-go-wrong.md)

-----

<div class="sect1">

# <span id="s1-rpm-build-starting-build">Starting the Build</span>

Now it's time to begin the build. First, we change directory into the
directory holding `cdplayer`'s spec file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># cd /usr/src/redhat/SPECS
#
        </code></pre></td>
</tr>
</tbody>
</table>

Next, we start the build with an **rpmbuild** command:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba cdplayer-1.0.spec
        </code></pre></td>
</tr>
</tbody>
</table>

The **a** following the **-b** option directs RPM to perform all phases
of the build process. Sometimes it is necessary to stop at various
phases during the initial build to resolve problems that crop up while
writing the spec file. In these cases, other letters can be used after
the **-b** in order to stop the build at the desired phase. For this
example however, we will continue through the entire build process.

In this example, the only other argument to the build command is the
name of the package's spec file. This can be wild-carded to build more
than one package, but in our example, we'll stick with one.

Let's look at RPM's output during the build:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>* Package: cdplayer
+ umask 022
+ echo Executing: %prep
Excuting: %prep
+ cd /usr/src/redhat/BUILD
+ cd /usr/src/redhat/BUILD
+ rm -rf cdplayer-1.0
+ gzip -dc /usr/src/redhat/SOURCES/cdplayer-1.0.tgz
+ tar -xvvf -
drwxrwxr-x root/users        0 Aug  4 22:30 1996 cdplayer-1.0/
-rw-r--r-- root/users    17982 Nov 10 01:10 1995 cdplayer-1.0/COPYING
-rw-r--r-- root/users      627 Nov 10 01:10 1995 cdplayer-1.0/ChangeLog
-rw-r--r-- root/users      482 Nov 10 01:11 1995 cdplayer-1.0/INSTALL
…
-rw-r--r-- root/users     2720 Nov 10 01:10 1995 cdplayer-1.0/struct.h
-rw-r--r-- root/users      730 Nov 10 01:10 1995 cdplayer-1.0/vol.c
-rw-r--r-- root/users     2806 Nov 10 01:10 1995 cdplayer-1.0/volume.c
-rw-r--r-- root/users     1515 Nov 10 01:10 1995 cdplayer-1.0/volume.h
+ [ 0 -ne 0 ]
+ cd cdplayer-1.0
+ cd /usr/src/redhat/BUILD/cdplayer-1.0
+ chown -R root.root .
+ chmod -R a+rX,g-w,o-w .
+ exit 0

        </code></pre></td>
</tr>
</tbody>
</table>

The output continues, but let's stop here for a moment, and discuss what
has happened so far.

At the start of the output, RPM displays the package name (`cdplayer`),
sets the umask, and starts executing the **%prep** section. Thanks to
the **%setup** macro, RPM then changes directory into the build area,
removes any existing old sources, and extracts the sources from the
original compressed tar file. Although each file is listed as it is
extracted, we've omitted most of the files listed, to save space.

The **%setup** macro continues by changing directory into `cdplayer`'s
top-level source directory and setting the file ownership and
permissions properly. As you can see, it does quite a bit of work for
you.

Let's take a look at the output from the **%build** section next:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>+ umask 022
+ echo Excuting: %build
Excuting: %build
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
+ make
gcc -Wall -O2  -c -I/usr/include/ncurses  cdp.c 
gcc -Wall -O2  -c -I/usr/include/ncurses  color.c 
gcc -Wall -O2  -c -I/usr/include/ncurses  display.c 
gcc -Wall -O2  -c -I/usr/include/ncurses  misc.c 
gcc -Wall -O2  -c -I/usr/include/ncurses  volume.c 
volume.c: In function `mix_set_volume&#39;:
volume.c:67: warning: implicit declaration of function `ioctl&#39;
gcc -Wall -O2  -c -I/usr/include/ncurses  hardware.c 
gcc -Wall -O2  -c -I/usr/include/ncurses  database.c 
gcc -Wall -O2  -c -I/usr/include/ncurses  getline.c 
gcc -o cdp cdp.o color.o display.o misc.o volume.o hardware.o database.o
getline.o  -I/usr/include/ncurses  -L/usr/lib -lncurses
groff -Tascii -man cdp.1 | compress &gt;cdp.1.Z
+ exit 0

        </code></pre></td>
</tr>
</tbody>
</table>

There are no surprises here. After setting the umask and changing
directory into `cdplayer`'s top-level directory, RPM issues the **make**
command we put into the spec file. The rest of the output comes from
**make** as it actually builds the software. Next comes the **%install**
section:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>+ umask 022
+ echo Excuting: %install
Excuting: %install
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
+ make install
chmod 755 cdp
chmod 644 cdp.1.Z
cp cdp /usr/local/bin
ln -s /usr/local/bin/cdp /usr/local/bin/cdplay
cp cdp.1 /usr/local/man/man1
+ exit 0

        </code></pre></td>
</tr>
</tbody>
</table>

Just like the previous sections, RPM again sets the umask and changes
directory into the proper directory. It then executes `cdplayer`'s
install target, installing the newly built software on the build system.
Those of you that carefully studied the spec file might have noticed
that the `README` file is not part of the install section. It's not a
problem, as we see here:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>+ umask 022
+ echo Excuting: special doc
Excuting: special doc
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
+ DOCDIR=//usr/doc/cdplayer-1.0-1
+ rm -rf //usr/doc/cdplayer-1.0-1
+ mkdir -p //usr/doc/cdplayer-1.0-1
+ cp -ar README //usr/doc/cdplayer-1.0-1
+ exit 0

        </code></pre></td>
</tr>
</tbody>
</table>

After the customary **umask** and **cd** commands, RPM constructs the
path that will be used for `cdplayer`'s documentation directory. It then
cleans out any preexisting directory and copies the `README` file into
it. The `cdplayer` app is now installed on the build system. The only
thing left to do is to create the actual package files, and perform some
housekeeping. The binary package file is created first:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Binary Packaging: cdplayer-1.0-1
Finding dependencies...
Requires (2): libc.so.5 libncurses.so.2.0
usr/doc/cdplayer-1.0-1
usr/doc/cdplayer-1.0-1/README
usr/local/bin/cdp
usr/local/bin/cdplay
usr/local/man/man1/cdp.1
93 blocks
Generating signature: 0
Wrote: /usr/src/redhat/RPMS/i386/cdplayer-1.0-1.i386.rpm

        </code></pre></td>
</tr>
</tbody>
</table>

The first line says it all: RPM is creating the binary package for
`cdplayer` version 1.0, release 1. Next, RPM determines what packages
are required by `cdplayer-1.0-1`. Part of this process entails running
**ldd** on each executable program in the package. In this example, the
package requires the libraries `libc.so.5`, and `libncurses.so.2.0`.
Other dependency information can be included in the spec file, but for
our example we'll keep it simple.

Following the dependency information, there is a list of every directory
and file included in the package. The list displayed is actually the
output of **cpio**, which is the archiving software used by RPM to
bundle the package's files. The "93 blocks" is also printed by **cpio**.

The line "Generating signature: 0" means that RPM has not been directed
to add a PGP signature to the package file. During this time, however,
RPM still adds two signatures that can be used to verify the size and
the MD5 checksum of the package file. Finally, we see confirmation that
RPM has created the binary package file.

At this point, the application has been built, and the application's
files have been packaged. There is no longer any need for any files
created during the build, so they may be removed. In the case of the
sources extracted into RPM's build directory, we can see that, at worst,
they will be removed the next time the package is built. But what if
there *were* files that we needed to remove? Well, they could be deleted
here, in the **%clean** section:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>+ umask 022
+ echo Excuting: %clean
Excuting: %clean
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
+ exit 0

        </code></pre></td>
</tr>
</tbody>
</table>

In our example, there are no other files outside of the build directory
that are created during `cdplayer`'s build, so we don't need to expend
any additional effort to clean things up.

The very last step performed by RPM is to create the source package
file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Source Packaging: cdplayer-1.0-1
cdplayer-1.0.spec
cdplayer-1.0.tgz
80 blocks
Generating signature: 0
Wrote: /usr/src/redhat/SRPMS/cdplayer-1.0-1.src.rpm

# 
        </code></pre></td>
</tr>
</tbody>
</table>

This file includes everything needed to recreate a binary package file,
as well as a copy of itself. In this example, the only files needed to
do that are the original sources and the spec file. In cases where the
original sources needed to be modified, the source package includes one
or more patch files. As when the binary package was created, we see
**cpio**'s output listing each file archived, along with the archive's
block size.

Just like a binary package, a source package file can have a PGP
signature attached to it. In our case, we see that a PGP signature was
not attached. The last message from RPM is to confirm the creation of
the source package. Let's take a look at the end products. First, the
binary package:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -lF /usr/src/redhat/RPMS/i386/cdplayer-1.0-1.i386.rpm
-rw-r--r--   1 root     root        24698 Aug  6 22:22 RPMS/i386/cdplayer-1.0-1.i386.rpm

#
        </code></pre></td>
</tr>
</tbody>
</table>

Note that we built `cdplayer` on an Intel-based system, so RPM placed
the binary package files in the `i386` subdirectory.

Next, the source package file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -lF /usr/src/redhat/SRPMS/cdplayer-1.0-1.src.rpm
-rw-r--r--   1 root     root        41380 Aug  6 22:22 SRPMS/cdplayer-1.0-1.src.rpm

#
        </code></pre></td>
</tr>
</tbody>
</table>

Everything went perfectly — we now have binary and source package files
ready to use. But sometimes things don't go so well.

</div>

<div class="NAVFOOTER">

-----

|                                              |                         |                                                |
| :------------------------------------------- | :---------------------: | ---------------------------------------------: |
| [Prev](s1-rpm-build-creating-spec-file.md) |   [Home](index.md)    | [Next](s1-rpm-build-when-things-go-wrong.md) |
| Creating the Spec File                       | [Up](ch-rpm-build.md) |                           When Things Go Wrong |

</div>
