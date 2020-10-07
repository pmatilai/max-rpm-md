<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-rw-build-build-without-rpm.html)

Chapter 20. Real-World Package Building

[Next](s1-rpm-rw-build-package-building.html)

-----

<div class="sect1">

# <span id="s1-rpm-rw-build-initial-build-with-rpm">Initial Building With RPM</span>

Now that amanda has been configured, built, and is operational on our
build system, it's time to have RPM take over each of these tasks. The
first task is to have RPM make the necessary changes to the original
sources. To do that, RPM needs a patch file.

<div class="sect2">

## <span id="s2-rpm-rw-build-generating-patches">Generating patches</span>

The `amanda-2.3.0` directory tree is where we did all our work building
amanda. We need to take all the work we've done in that directory tree
and compare it against the original sources contained in the
`amanda-2.3.0-orig` directory tree. But before we do that, we need to
clean things up a bit.

<div class="sect3">

### <span id="s3-rpm-rw-build-cleaning-up-test-build-area">Cleaning up the test build area</span>

Looking through our work tree, it has all sorts of junk in it: emacs
save files, object files, and the executable programs. In order to
generate a clean set of patches, all these extraneous files must go.
Looking over amanda's makefiles, there is a **clean** target that should
take care of most of the junk:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># make clean
Making clean in common-src
…
rm -f *~ *.o *.a genversion version.c Makefile.out
…
Making clean in client-src
…
rm -f amandad sendsize calcsize sendbackup-dump
 sendbackup-gnutar runtar selfcheck  *~ *.o Makefile.out
…
Making clean in server-src
…
rm -f amrestore amadmin amflush amlabel amcheck amdump
 amcleanup amtape taper dumper driver planner reporter
 getconf *~ *.o Makefile.out
…
Making clean in changer-src
…
rm -f chg-generic *~ *.o Makefile.out
…
Making clean in man
…
rm -f *~ Makefile.out
…

# 
            </code></pre></td>
</tr>
</tbody>
</table>

Looking in the `tools` and `config` directories where we did all our
work, we see there are still emacs save files there. A bit of studying
confirms that the makefiles don't bother to clean these two directories.
That's a nice touch because a **make clean** won't wipe out old copies
of the config files, giving you a chance to go back to them in case
you've botched something. However, in our case, we're sure we won't need
the save files, so out they go:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># cd /usr/src/redhat/SOURCES/amanda-2.3.0
# find . -name &quot;*~&quot; -exec rm -vf \;
./config/config.h~
./config/options.h~
./tools/munge~

#
            </code></pre></td>
</tr>
</tbody>
</table>

We let **find** take a look at the whole directory tree, just in case
there was something still out there that we'd forgotten about. As you
can see, the only save files are from the three files we've been working
on.

You'll note that we've left our modified `munge` file, as well as the
`config.h` and `options.h` files we so carefully crafted. That's
intentional, as we want to make sure those changes are applied when RPM
patches the sources. Everything looks pretty clean, so it's time to make
the patches.

</div>

<div class="sect3">

### <span id="s3-rpm-rw-build-actually-generating-patches">Actually Generating patches</span>

This step is actually pretty anticlimactic:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># diff -uNr amanda-2.3.0-orig/ amanda-2.3.0/ &gt; amanda-2.3.0-linux.patch
#
            </code></pre></td>
</tr>
</tbody>
</table>

With that one command, we've compared each file in the untouched
directory tree (`amanda-2.3.0-orig`) with the directory tree we've been
working in (`amanda-2.3.0`). If we've done our homework, the only things
in the patch file should be related to the files we've changed. Let's
take a look through it to make sure:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># cd /usr/src/redhat/SOURCES
# cat amanda-2.3.0-linux.patch
diff -uNr amanda-2.3.0-orig/config/config.h amanda-2.3.0/config/config.h
--- amanda-2.3.0-orig/config/config.h   Wed Dec 31 19:00:00 1969
+++ amanda-2.3.0/config/config.h    Sat Nov 16 16:22:47 1996
@@ -0,0 +1,52 @@
…
diff -uNr amanda-2.3.0-orig/config/options.h amanda-2.3.0/config/options.h
--- amanda-2.3.0-orig/config/options.h  Wed Dec 31 19:00:00 1969
+++ amanda-2.3.0/config/options.h   Sat Nov 16 17:08:57 1996
@@ -0,0 +1,211 @@
…
diff -uNr amanda-2.3.0-orig/tools/munge amanda-2.3.0/tools/munge
--- amanda-2.3.0-orig/tools/munge   Sun May 19 22:11:25 1996
+++ amanda-2.3.0/tools/munge    Sat Nov 16 16:23:50 1996
@@ -35,10 +35,10 @@
 # Customize CPP to point to your system&#39;s C preprocessor.
 
 # if cpp is on your path:
-CPP=cpp
+# CPP=cpp
 
 # if cpp is not on your path, try one of these:
-# CPP=/lib/cpp                 # traditional
+CPP=/lib/cpp                   # traditional
 # CPP=/usr/lib/cpp             # also traditional
 # CPP=/usr/ccs/lib/cpp         # Solaris 2.x
#

            </code></pre></td>
</tr>
</tbody>
</table>

The patch file contains complete copies of our `config.h` and
`options.h` files, followed by the changes we've made to `munge`. Looks
good\! Time to hand this grunt work over to RPM.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-rw-build-first-cut-spec-file">Making a first-cut spec file</span>

Since amanda comes in two parts, it's obvious we'll need to use
subpackages: one for the client software, and one for the server. Given
that, and the fact that the first part of any spec file consists of tags
that are easily filled in, let's sit down and fill in the blanks,
tag-wise:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Summary: Amanda Network Backup System
Name: amanda
Version: 2.3.08
Release: 1
Group: System/Backup
License: BSD-like, but see COPYRIGHT file for details
Packager: Edward C. Bailey &lt;bailey@rpm.org&gt;
URL: http://www.cs.umd.edu/projects/amanda/
Source: ftp://ftp.cs.umd.edu/pub/amanda/amanda-2.3.0.tar.gz
Patch: amanda-2.3.0-linux.patch
%description
Amanda is a client/server backup system.  It uses standard tape
devices and networking, so all you need is any working tape drive
and a network.  You can use it for local backups as well.

          </code></pre></td>
</tr>
</tbody>
</table>

That part was pretty easy. We set the package's release number to 1.
We'll undoubtedly be changing that as we continue work on the spec file.
You'll notice that we've included a **URL** tag line; the Uniform
Resource Locator there points to the homepage for the amanda project,
making it easier for the user to get additional information on amanda.

The **Source** tag above includes the name of the original source tar
file and is preceded by the URL pointing to the file's primary location.
Again, this makes it easy for the user to grab a copy of the sources
from the software's "birthplace".

Finally, the patch file that we've just created gets a line of its own
on the **Patch** tag line. Next, let's take a look at the tags for our
two subpackages. Let's start with the client:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%package client
Summary: Client-side Amanda package
Group: System/Backup
Requires: dump
%description client
The Amanda Network Backup system contains software necessary to
automatically perform backups across a network.  Amanda consists of
two packages -- a client (this package), and a server:

The client package enable a network-capable system to have its
filesystems backed up by a system running the Amanda server.

NOTE: In order for a system to perform backups of itself, install both
the client and server packages!

          </code></pre></td>
</tr>
</tbody>
</table>

The **%package** directive names the package. Since we wanted the
subpackages to be named `amanda-<something>`, we didn't use the **-n**
option. This means our client subpackage will be called `amanda-client`,
just as we wanted. RPM requires unique **summary**, **%description**,
and **group** tags for each subpackage, so we've included them. Of
course, it would be a good idea even if RPM *didn't* require them —
we've used the tags to provide client-specific information.

The **requires** tag is the only other tag in the client subpackage.
Since amanda uses **dump** on the client system, we included this tag so
that RPM will ensure that the **dump** package is present on client
systems.

Next, let's take a look at the tags for the server subpackage:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%package server
Summary: Server-side Amanda package
Group: System/Backup
%description server
The Amanda Network Backup system contains software necessary to
automatically perform backups across a network.  Amanda consists of
two package -- a client, and a server (this package):

The server package enables a network-capable system to control one
or more Amanda client systems performing backups.  The server system
will direct all backups to a locally attached tape drive.  Therefore,
the server system requires a tape drive.

NOTE: In order for a system to perform backups of itself, install both
the client and server packages!

          </code></pre></td>
</tr>
</tbody>
</table>

No surprises here, really. You'll note that the server subpackage has no
**requires** tag for the dump package. The reason for that is due to a
design decision we've made. Since amanda is comprised of a client and a
server component, in order for the server system to perform backups of
itself, the client component must be installed. Since we've already made
the client subpackage require **dump**, we've already covered the bases.

Since an amanda server cannot back itself up without the client
software, why don't we have the server subpackage require the client
subpackage? Well, that could be done, but the fact of the matter is that
there are cases where an amanda server won't need to back itself up. So
the server subpackage needs no package requirements.

<div class="sect3">

### <span id="s3-rpm-rw-build-adding-build-time-scripts">Adding the build-time scripts</span>

Next we need to add the build-time scripts. There's really not much to
them:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%prep
%setup

%build
make

%install
make install

            </code></pre></td>
</tr>
</tbody>
</table>

The **%prep** script consists of one line containing the simplest flavor
of **%setup** macro. Since we only need **%setup** to unpack one set of
sources, there are no options we need to add.

The **%build** script is just as simple, with the single **make**
command required to build amanda.

Finally, the **%install** script maintains our singe-line trend for
build-time scripts. Here a simple **make install** will put all the
files where they need to be for RPM to package them.

</div>

<div class="sect3">

### <span id="s3-rpm-rw-build-adding-files-lists">Adding **%files** Lists</span>

The last part of our initial attempt at a spec file is a **%files** list
for each package the spec file will build. Since we're planning on a
client and a server subpackage, we'll need two **%files** lists. For the
time being, we'll just add the **%files** lines — we'll be adding the
actual filenames later:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%files client

%file server

            </code></pre></td>
</tr>
</tbody>
</table>

There's certainly more to come, but this is enough to get us started.
And the first thing we want RPM to do is to unpack the amanda sources.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-rw-build-rpm-unpacking-sources">Getting the original sources unpacked</span>

In keeping with a step-by-step approach, RPM has an option that let's us
stop the build process after the **%prep** script has run. Let's give
the **-bp** option a try, and see how things look:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -bp amanda-2.3.0.spec
* Package: amanda
* Package: amanda-client
* Package: amanda-server
+ umask 022
+ echo Executing: %prep
Executing: %prep
+ cd /usr/src/redhat/BUILD
+ cd /usr/src/redhat/BUILD
+ rm -rf amanda-2.3.0
+ gzip -dc /usr/src/redhat/SOURCES/amanda-2.3.0.tar.gz
+ tar -xvvf -
drwxr-xr-x 3/20              0 May 19 22:10 1996 amanda-2.3.0/
-rw-r--r-- 3/20           1389 May 19 22:11 1996 amanda-2.3.0/COPYRIGHT
-rw-r--r-- 3/20           1958 May 19 22:11 1996 amanda-2.3.0/Makefile
-rw-r--r-- 3/20          11036 May 19 22:11 1996 amanda-2.3.0/README
…
-rw-r--r-- 3/20           2010 May 19 22:11 1996 amanda-2.3.0/man/amtape.8
drwxr-xr-x 3/20              0 May 19 22:11 1996 amanda-2.3.0/tools/
-rwxr-xr-x 3/20           2437 May 19 22:11 1996 amanda-2.3.0/tools/munge
+ [ 0 -ne 0 ]
+ cd amanda-2.3.0
+ cd /usr/src/redhat/BUILD/amanda-2.3.0
+ chown -R root.root .
+ chmod -R a+rX,g-w,o-w .
+ exit 0

# 
          </code></pre></td>
</tr>
</tbody>
</table>

By looking at the output, it would be pretty hard to miss the fact that
the sources were unpacked. If we look in RPM's default build area
(`/usr/src/redhat/BUILD`), we'll see an amanda directory tree:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># cd /usr/src/redhat/BUILD/
# ls -l
total 3
drwxr-xr-x  11 root     root         1024 May 19  1996 amanda-2.3.0

#
          </code></pre></td>
</tr>
</tbody>
</table>

After a quick look around, it seems like the sources were unpacked
properly. But wait — where are our carefully crafted configuration files
in `config`? Why isn't `tools/munge` modified?

</div>

<div class="sect2">

## <span id="s2-rpm-rw-build-applying-patches">Getting patches properly applied</span>

Ah, perhaps our **%prep** script was a bit *too* simple. We need to
apply our patch. So let's add two things to our spec file:

1.  A **patch** tag line pointing to our patch file

2.  A **%patch** macro in our **%prep** script

Easy enough. At the top of the spec file, along with the other tags,
let's add:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Patch: amanda-2.3.0-linux.patch

          </code></pre></td>
</tr>
</tbody>
</table>

Then we'll make our **%prep** script look like this:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%prep
%setup
%patch -p 1

          </code></pre></td>
</tr>
</tbody>
</table>

There, that should do it. Let's give that **-bp** option another try:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -bp amanda-2.3.0.spec
* Package: amanda
* Package: amanda-client
* Package: amanda-server
+ umask 022
+ echo Executing: %prep
Executing: %prep
+ cd /usr/src/redhat/BUILD
+ cd /usr/src/redhat/BUILD
+ rm -rf amanda-2.3.0
+ gzip -dc /usr/src/redhat/SOURCES/amanda-2.3.0.tar.gz
+ tar -xvvf -
drwxr-xr-x 3/20              0 May 19 22:10 1996 amanda-2.3.0/
-rw-r--r-- 3/20           1389 May 19 22:11 1996 amanda-2.3.0/COPYRIGHT
-rw-r--r-- 3/20           1958 May 19 22:11 1996 amanda-2.3.0/Makefile
-rw-r--r-- 3/20          11036 May 19 22:11 1996 amanda-2.3.0/README
…
-rw-r--r-- 3/20           2010 May 19 22:11 1996 amanda-2.3.0/man/amtape.8
drwxr-xr-x 3/20              0 May 19 22:11 1996 amanda-2.3.0/tools/
-rwxr-xr-x 3/20           2437 May 19 22:11 1996 amanda-2.3.0/tools/munge
+ [ 0 -ne 0 ]
+ cd amanda-2.3.0
+ cd /usr/src/redhat/BUILD/amanda-2.3.0
+ chown -R root.root .
+ chmod -R a+rX,g-w,o-w .
+ echo Patch #0:
Patch #0:
+ patch -p1 -s
+ exit 0

# 
          </code></pre></td>
</tr>
</tbody>
</table>

Not much difference, until the very end, where we see the patch being
applied. Let's take a look into the build area and see if our
configuration files are there:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># cd /usr/src/redhat/BUILD/amanda-2.3.0/config
# ls -l
total 58
-rw-r--r--   1 root     root         7518 May 19  1996 config-common.h
-rw-r--r--   1 root     root         1846 Nov 20 20:46 config.h
-rw-r--r--   1 root     root         2081 May 19  1996 config.h-aix
-rw-r--r--   1 root     root         1690 May 19  1996 config.h-bsdi1
…
-rw-r--r--   1 root     root         1830 May 19  1996 config.h-ultrix4
-rw-r--r--   1 root     root            0 Nov 20 20:46 config.h.orig
-rw-r--r--   1 root     root         7196 Nov 20 20:46 options.h
-rw-r--r--   1 root     root         7236 May 19  1996 options.h-vanilla
-rw-r--r--   1 root     root            0 Nov 20 20:46 options.h.orig

# 
          </code></pre></td>
</tr>
</tbody>
</table>

Much better. Those zero-length `.orig` files are a dead giveaway that
patch has been here, as are the dates on `config.h`, and `options.h`. In
the `tools` directory, `munge` has been modified, too. These sources are
ready for building\!

</div>

<div class="sect2">

## <span id="s2-rpm-rw-build-rpm-building">Letting RPM do the Building</span>

We know that the sources are ready. We know that the **%build** script
is ready. There shouldn't be much in the way of surprises if we let RPM
build amanda. Let's use the **-bc** option to stop things after the
**%build** script completes:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -bc amanda-2.3.0.spec
* Package: amanda
* Package: amanda-client
* Package: amanda-server
…
 echo Executing: %build
Executing: %build
+ cd /usr/src/redhat/BUILD
+ cd amanda-2.3.0
+ make
Making all in common-src
make[1]: Entering directory `/usr/src/redhat/BUILD/amanda-2.3.0/common-src&#39;
../tools/munge Makefile.in Makefile.out
make[2]: Entering directory `/usr/src/redhat/BUILD/amanda-2.3.0/common-src&#39;
cc  -g  -I. -I../config   -c error.c -o error.o
cc  -g  -I. -I../config   -c alloc.c -o alloc.o
…
Making all in man
make[1]: Entering directory `/usr/src/redhat/BUILD/amanda-2.3.0/man&#39;
../tools/munge Makefile.in Makefile.out
make[2]: Entering directory `/usr/src/redhat/BUILD/amanda-2.3.0/man&#39;
make[2]: Nothing to be done for `all&#39;.
make[2]: Leaving directory `/usr/src/redhat/BUILD/amanda-2.3.0/man&#39;
make[1]: Leaving directory `/usr/src/redhat/BUILD/amanda-2.3.0/man&#39;
+ exit 0

# 
          </code></pre></td>
</tr>
</tbody>
</table>

As we thought, no surprises. A quick look through the build area shows a
full assortment of binaries, all ready to be installed. So it seems that
the most natural thing to do next would be to let RPM install amanda.

</div>

<div class="sect2">

## <span id="s2-rpm-rw-build-rpm-installing">Letting RPM do the Installing</span>

And that's just what we're going to do\! Our **%install** script has the
necessary **make install** command, so let's give it a shot:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -bi amanda-2.3.0.spec
* Package: amanda
* Package: amanda-client
* Package: amanda-server
…
 echo Executing: %build
Executing: %build
+ cd /usr/src/redhat/BUILD
+ cd amanda-2.3.0
+ make
Making all in common-src
make[1]: Entering directory `/usr/src/redhat/BUILD/amanda-2.3.0/common-src&#39;
../tools/munge Makefile.in Makefile.out
make[2]: Entering directory `/usr/src/redhat/BUILD/amanda-2.3.0/common-src&#39;
cc  -g  -I. -I../config   -c error.c -o error.o
cc  -g  -I. -I../config   -c alloc.c -o alloc.o
…
+ umask 022
+ echo Executing: %install
Executing: %install
+ cd /usr/src/redhat/BUILD
+ cd amanda-2.3.0
+ make install
Making install in common-src
make[1]: Entering directory `/usr/src/redhat/BUILD/amanda-2.3.0/common-src&#39;
…
  install -c -o bin amrestore.8 /usr/man/man8
  install -c -o bin amtape.8 /usr/man/man8
make[2]: Leaving directory `/usr/src/redhat/BUILD/amanda-2.3.0/man&#39;
make[1]: Leaving directory `/usr/src/redhat/BUILD/amanda-2.3.0/man&#39;
+ exit 0
+ umask 022
+ echo Executing: special doc
Executing: special doc
+ cd /usr/src/redhat/BUILD
+ cd amanda-2.3.0
+ DOCDIR=//usr/doc/amanda-2.3.0-1
+ DOCDIR=//usr/doc/amanda-client-2.3.0-1
+ DOCDIR=//usr/doc/amanda-server-2.3.0-1
+ exit 0

# 
          </code></pre></td>
</tr>
</tbody>
</table>

Everything looks pretty good. At this point, the amanda software, built
by RPM, has been installed on the build system. Since performed all the
configuration steps before, when we were manually building amanda,
everything should still be configured properly to test this new build.
[<span class="footnote">\[1\]</span>](#FTN.AEN12213) So why don't we
give the new binaries a try?

</div>

<div class="sect2">

## <span id="s2-rpm-rw-build-building-packages">Testing RPM's Handiwork</span>

After a quick double-check to ensure that all the configuration steps
were still in place from our manual build, we reran our tests. No
problems were found. It's time to build some packages\!

</div>

</div>

### Notes

|                                                                                             |                                                                                                                                                            |
| ------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-rw-build-initial-build-with-rpm.html#AEN12213) | Of course, if the process of installing the software changed some necessary config files, they would have to be redone, but in this case it didn't happen. |

<div class="NAVFOOTER">

-----

|                                                |                            |                                               |
| :--------------------------------------------- | :------------------------: | --------------------------------------------: |
| [Prev](s1-rpm-rw-build-build-without-rpm.html) |     [Home](index.html)     | [Next](s1-rpm-rw-build-package-building.html) |
| Initial Building Without RPM                   | [Up](ch-rpm-rw-build.html) |                              Package Building |

</div>
