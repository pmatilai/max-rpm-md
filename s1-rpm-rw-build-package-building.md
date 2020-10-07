<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-rw-build-initial-build-with-rpm.html)

Chapter 20. Real-World Package Building

[Next](ch-rpm-rpmlib.html)

-----

<div class="sect1">

# <span id="s1-rpm-rw-build-package-building">Package Building</span>

OK, let's go for broke and tell RPM to do the works, including the
creation of the binary and source packages:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba amanda-2.3.0.spec
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
…
+ echo Executing: %install
Executing: %install
+ cd /usr/src/redhat/BUILD
+ cd amanda-2.3.0
+ make install
Making install in common-src
…
+ echo Executing: special doc
Executing: special doc
…
Binary Packaging: amanda-client-2.3.0-1
Finding dependencies...
Requires (1): dump
1 block
Generating signature: 0
Wrote: /usr/src/redhat/RPMS/i386/amanda-client-2.3.0-1.i386.rpm
Binary Packaging: amanda-server-2.3.0-1
Finding dependencies...
1 block
Generating signature: 0
Wrote: /usr/src/redhat/RPMS/i386/amanda-server-2.3.0-1.i386.rpm
+ umask 022
+ echo Executing: %clean
Executing: %clean
+ cd /usr/src/redhat/BUILD
+ cd amanda-2.3.0
+ exit 0
Source Packaging: amanda-2.3.0-1
amanda-2.3.0.spec
amanda-2.3.0-linux.patch
amanda-2.3.0.tar.gz
374 blocks
Generating signature: 0
Wrote: /usr/src/redhat/SRPMS/amanda-2.3.0-1.src.rpm

# 
        </code></pre></td>
</tr>
</tbody>
</table>

Great\! Let's take a look at our handiwork:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># cd /usr/src/redhat/RPMS/i386/
# ls -l
total 2
-rw-r--r-- 1 root  root   1246 Nov 20 21:19 amanda-client-2.3.0-1.i386.rpm
-rw-r--r-- 1 root  root   1308 Nov 20 21:19 amanda-server-2.3.0-1.i386.rpm

# 
        </code></pre></td>
</tr>
</tbody>
</table>

Hmmm, those binary packages look sort of small. We'd better see what's
in there:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qilp amanda-*-1.i386.rpm
Name        : amanda-client         Distribution: (none)
Version     : 2.3.0                       Vendor: (none)
Release     : 1                       Build Date: Wed Nov 20 21:19:44 1996
Install date: (none)                  Build Host: moocow.rpm.org
Group       : System/Backup           Source RPM: amanda-2.3.0-1.src.rpm
Size        : 0
Summary     : Client-side Amanda package
Description :
The Amanda Network Backup system contains software necessary to
automatically perform backups across a network.  Amanda consists of
two packages -- a client (this package), and a server:

The client package enable a network-capable system to have its
filesystems backed up by a system running the Amanda server.

NOTE: In order for a system to perform backups of itself, install both
the client and server packages!
(contains no files)

Name        : amanda-server         Distribution: (none)
Version     : 2.3.0                       Vendor: (none)
Release     : 1                       Build Date: Wed Nov 20 21:19:44 1996
Install date: (none)                  Build Host: moocow.rpm.org
Group       : System/Backup           Source RPM: amanda-2.3.0-1.src.rpm
Size        : 0
Summary     : Server-side Amanda package
Description :
The Amanda Network Backup system contains software necessary to
automatically perform backups across a network.  Amanda consists of
two package -- a client, and a server (this package):

The server package enables a network-capable system to control one
or more Amanda client systems performing backups.  The server system
will direct all backups to a locally attached tape drive.  Therefore,
the server system requires a tape drive.

NOTE: In order for a system to perform backups of itself, install both
the client and server packages!
(contains no files)

# 
        </code></pre></td>
</tr>
</tbody>
</table>

What do they mean, (contains no files)? The spec file has perfectly good
**%files** lists…

Oops.

<div class="sect2">

## <span id="s2-rpm-rw-build-creating-files-list">Creating the **%files** list</span>

Everything was going so smoothly, we forgot that the **%files** lists
were going to need files. No problem, we just need to put the filenames
in there, and we'll be all set. But is it *really* that easy?

<div class="sect3">

### <span id="s3-rpm-rw-build-how-to-find-files">How to find the installed files?</span>

Luckily, it's not too bad. Since we saved the output from our first
**make install**, we can see the filenames as they're installed. Of
course, it's important to make sure the install output is valid.
Fortunately for us, amanda didn't require much fiddling by the time we
got it built and tested. If it had, we would have had to get more recent
output from the installation phase.

It's time for more decisions. We have one list of installed files, and
two **%files** lists. It would be silly to put all the files in both
**%files** lists, so we have to decide which file goes where.

This is where experience with the software really pays off, because the
wrong decision made here can result in awkward, ill-featured packages.
Here's the **%files** list we came up with for the client subpackage:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%files client
/usr/lib/amanda/amandad
/usr/lib/amanda/sendsize
/usr/lib/amanda/calcsize
/usr/lib/amanda/sendbackup-dump
/usr/lib/amanda/selfcheck
/usr/lib/amanda/sendbackup-gnutar
/usr/lib/amanda/runtar
README
COPYRIGHT
docs/INSTALL
docs/SYSTEM.NOTES
docs/WHATS.NEW

            </code></pre></td>
</tr>
</tbody>
</table>

The files in `/usr/lib/amanda` are all the client-side amanda programs,
so that part was easy. The remaining files are part of the original
source archive. Amanda doesn't install them, but they contain
information that users should see.

Realizing that RPM can't package these files specified as they are,
let's leave the client **%files** list for a moment, and check out the
list for the server subpackage:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%files server
/usr/sbin/amadmin
/usr/sbin/amcheck
/usr/sbin/amcleanup
/usr/sbin/amdump
/usr/sbin/amflush
/usr/sbin/amlabel
/usr/sbin/amrestore
/usr/sbin/amtape
/usr/lib/amanda/taper
/usr/lib/amanda/dumper
/usr/lib/amanda/driver
/usr/lib/amanda/planner
/usr/lib/amanda/reporter
/usr/lib/amanda/getconf
/usr/lib/amanda/chg-generic
/usr/man/man8/amanda.8
/usr/man/man8/amadmin.8
/usr/man/man8/amcheck.8
/usr/man/man8/amcleanup.8
/usr/man/man8/amdump.8
/usr/man/man8/amflush.8
/usr/man/man8/amlabel.8
/usr/man/man8/amrestore.8
/usr/man/man8/amtape.8
README
COPYRIGHT
docs/INSTALL
docs/KERBEROS
docs/SUNOS4.BUG
docs/SYSTEM.NOTES
docs/TAPE.CHANGERS
docs/WHATS.NEW
docs/MULTITAPE
example

            </code></pre></td>
</tr>
</tbody>
</table>

The files in `/usr/sbin` are programs that will be run by the amanda
administrator in order to perform backups and restores. The files in
`/usr/lib/amanda` are the server-side programs that do the actual work
during backups. Following that are a number of man pages: one for each
program to be run by the amanda administrator, and one with an overview
of amanda.

Bringing up the rear are a number of files that are not installed, but
would be handy for the amanda administrator to have available. There is
some overlap with the files that will be part of the client subpackage,
but the additional files here discuss features that would interest only
amanda administrators. Included here is the `example` subdirectory,
which contains a few example configuration files for the amanda server.

As in the client **%files** list, these last files can't be packaged by
RPM as we've listed them. We need to use a few more of RPM's tricks to
get them packaged.

</div>

<div class="sect3">

### <span id="s3-rpm-rw-build-appying-directives">Applying Directives</span>

Since we'd like the client subpackage to include those files that are
not normally installed, and since the files are documentation, let's use
the **%doc** directive on them. That will accomplish two things:

1.  When the client subpackage is installed, it will direct RPM to place
    them in a package-specific directory in `/usr/doc`

2.  It will tag the files as being documentation, making it possible for
    users to easily track down the documentation with a simple **rpm
    -qd** command

In the course of looking over the **%files** lists, it becomes apparent
that the directory `/usr/lib/amanda` will contain only files from the
two amanda subpackages. If the subpackages are erased, the directory
will remain, which won't hurt anything, but it isn't as neat as it could
be. But if we add the directory to the list, RPM will automatically
package every file in the directory. Since the files in that directory
are part of both the client and the server subpackages, we'll need to
use the **%dir** directive to instruct RPM to package only the
directory.

After these changes, here's what the client **%files** list looks like
now:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%files client
%dir /usr/lib/amanda/
/usr/lib/amanda/amandad
/usr/lib/amanda/sendsize
/usr/lib/amanda/calcsize
/usr/lib/amanda/sendbackup-dump
/usr/lib/amanda/selfcheck
/usr/lib/amanda/sendbackup-gnutar
/usr/lib/amanda/runtar
%doc README
%doc COPYRIGHT
%doc docs/INSTALL
%doc docs/SYSTEM.NOTES
%doc docs/WHATS.NEW

            </code></pre></td>
</tr>
</tbody>
</table>

We've also applied the same directives to the server **%files** list:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%files server
/usr/sbin/amadmin
/usr/sbin/amcheck
/usr/sbin/amcleanup
/usr/sbin/amdump
/usr/sbin/amflush
/usr/sbin/amlabel
/usr/sbin/amrestore
/usr/sbin/amtape
%dir /usr/lib/amanda/
/usr/lib/amanda/taper
/usr/lib/amanda/dumper
/usr/lib/amanda/driver
/usr/lib/amanda/planner
/usr/lib/amanda/reporter
/usr/lib/amanda/getconf
/usr/lib/amanda/chg-generic
/usr/man/man8/amanda.8
/usr/man/man8/amadmin.8
/usr/man/man8/amcheck.8
/usr/man/man8/amcleanup.8
/usr/man/man8/amdump.8
/usr/man/man8/amflush.8
/usr/man/man8/amlabel.8
/usr/man/man8/amrestore.8
/usr/man/man8/amtape.8
%doc README
%doc COPYRIGHT
%doc docs/INSTALL
%doc docs/KERBEROS
%doc docs/SUNOS4.BUG
%doc docs/SYSTEM.NOTES
%doc docs/TAPE.CHANGERS
%doc docs/WHATS.NEW
%doc docs/MULTITAPE
%doc example

            </code></pre></td>
</tr>
</tbody>
</table>

You'll note that we neglected to use the **%doc** directive on the man
page files. The reason is that RPM automatically tags any file destined
for `/usr/man` as documentation. Now our spec file has a complete set of
tags, the two subpackages are defined, it has build-time scripts that
work, and now, **%files** lists for each subpackage. Why don't we try
that build again?

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba amanda-2.3.0.spec
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
…
+ echo Executing: %install
Executing: %install
+ cd /usr/src/redhat/BUILD
+ cd amanda-2.3.0
+ make install
Making install in common-src
…
+ echo Executing: special doc
Executing: special doc
…
Binary Packaging: amanda-client-2.3.0-6
Finding dependencies...
Requires (3): libc.so.5 libdb.so.2 dump
usr/doc/amanda-client-2.3.0-6
usr/doc/amanda-client-2.3.0-6/COPYRIGHT
usr/doc/amanda-client-2.3.0-6/INSTALL
…
usr/lib/amanda/sendbackup-gnutar
usr/lib/amanda/sendsize
1453 blocks
Generating signature: 0
Wrote: /usr/src/redhat/RPMS/i386/amanda-client-2.3.0-6.i386.rpm
Binary Packaging: amanda-server-2.3.0-6
Finding dependencies...
Requires (2): libc.so.5 libdb.so.2
usr/doc/amanda-server-2.3.0-6
usr/doc/amanda-server-2.3.0-6/COPYRIGHT
usr/doc/amanda-server-2.3.0-6/INSTALL
…
usr/sbin/amrestore
usr/sbin/amtape
3404 blocks
Generating signature: 0
Wrote: /usr/src/redhat/RPMS/i386/amanda-server-2.3.0-6.i386.rpm
…
Source Packaging: amanda-2.3.0-6
amanda-2.3.0.spec
amanda-2.3.0-linux.patch
amanda-rpm-instructions.tar.gz
amanda-2.3.0.tar.gz
393 blocks
Generating signature: 0
Wrote: /usr/src/redhat/SRPMS/amanda-2.3.0-6.src.rpm

# 
            </code></pre></td>
</tr>
</tbody>
</table>

If we take a quick look at the client and server subpackages, we find
that, sure enough, this time they contain files:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># cd /usr/src/redhat/RPMS/i386/
# ls -l amanda-*
-rw-r--r-- 1 root  root  211409 Nov 21 15:56 amanda-client-2.3.0-1.i386.rpm
-rw-r--r-- 1 root  root  512814 Nov 21 15:57 amanda-server-2.3.0-1.i386.rpm

# rpm -qilp amanda-*
Name        : amanda-client         Distribution: (none)
Version     : 2.3.0                       Vendor: (none)
Release     : 1                       Build Date: Thu Nov 21 15:55:59 1996
Install date: (none)                  Build Host: moocow.rpm.org
Group       : System/Backup           Source RPM: amanda-2.3.0-1.src.rpm
Size        : 737101
Summary     : Client-side Amanda package
Description :
The Amanda Network Backup system contains software necessary to
automatically perform backups across a network.  Amanda consists of
two packages -- a client (this package), and a server:

The client package enable a network-capable system to have its
filesystems backed up by a system running the Amanda server.

NOTE: In order for a system to perform backups of itself, install both
the client and server packages!

/usr/doc/amanda-client-2.3.0-1
/usr/doc/amanda-client-2.3.0-1/COPYRIGHT
/usr/doc/amanda-client-2.3.0-1/INSTALL
…
/usr/lib/amanda/sendbackup-gnutar
/usr/lib/amanda/sendsize

Name        : amanda-server         Distribution: (none)
Version     : 2.3.0                       Vendor: (none)
Release     : 1                       Build Date: Thu Nov 21 15:55:59 1996
Install date: (none)                  Build Host: moocow.rpm.org
Group       : System/Backup           Source RPM: amanda-2.3.0-1.src.rpm
Size        : 1733825
Summary     : Server-side Amanda package
Description :
The Amanda Network Backup system contains software necessary to
automatically perform backups across a network.  Amanda consists of
two package -- a client, and a server (this package):

The server package enables a network-capable system to control one
or more Amanda client systems performing backups.  The server system
will direct all backups to a locally attached tape drive.  Therefore,
the server system requires a tape drive.

NOTE: In order for a system to perform backups of itself, install both
the client and server packages!

/usr/doc/amanda-server-2.3.0-1
/usr/doc/amanda-server-2.3.0-1/COPYRIGHT
/usr/doc/amanda-server-2.3.0-1/INSTALL
…
/usr/sbin/amrestore
/usr/sbin/amtape

# 
            </code></pre></td>
</tr>
</tbody>
</table>

We're finally ready to test these packages\!

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-rw-build-testing-first-packages">Testing those first packages</span>

The system we've built the packages on already has amanda installed.
This is due to the build process itself. However, we can install the new
packages on top of the already-existing files:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># cd /usr/src/redhat/RPMS/i386
# rpm -ivh amanda-*-1.i386.rpm
amanda-client       ##################################################
amanda-server       ##################################################

#
          </code></pre></td>
</tr>
</tbody>
</table>

Running some tests, it looks like everything is running well. But back
in [the Section called *Testing Newly Built Packages* in Chapter
11](s1-rpm-build-when-things-go-wrong.html#s2-rpm-build-testing-packages),
we mentioned that it was possible to install a newly-built package on
the build system, and not realize that the package was missing files.
Well, there's another reason why installing the package on the
build-system for testing is a bad idea. Let's bring our packages to a
different system, test them there, and see what happens.

<div class="sect3">

### <span id="s3-rpm-rw-build-installing-different-system">Installing the Package On A Different System</span>

Looks like we're almost through. Let's install the packages on another
system that had not previously run amanda, and test it there:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -ivh amanda-*-1.i386.rpm
amanda-client       ##################################################
amanda-server       ##################################################

# 
            </code></pre></td>
</tr>
</tbody>
</table>

The install went smoothly enough. However, testing did not. Why? Nothing
was set up\! The server configuration files, the `inetd.conf` entry for
the client, everything was missing. If we stop and think about it for a
moment that makes sense: we had gone through all those steps on the
build system, but none of those steps can be packaged as files.

After following the steps in the installation instructions, everything
works. While we could expect users to do most of the grunt work
associated with getting amanda configured, RPM *does* have the ability
to run scripts when packages are installed and erased. Why don't we use
that feature to make life easier for our users?

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-rw-build-finishing-touches">Finishing Touches</span>

At this point in the build process, we're on the home stretch. The
software builds correctly and is packaged. It's time to stop looking at
things from a "build the software" perspective, and time to starting
looking at things from a "package the software" point of view.

The difference lies in looking at the packages from the user's
perspective. Does the package install easily, or does it require a lot
of effort to make it operative? When the package is removed, does it
clean up after itself, or does it leave bits and pieces strewn
throughout the filesystem?

Let's put a bit more effort into this spec file, and make life easier on
our users.

<div class="sect3">

### <span id="s3-rpm-rw-build-creating-install-scripts">Creating Install Scripts</span>

When it comes to needing post-installation configuration, amanda
certainly is no slouch\! We'll work on the client first. Let's look at a
section of the script we wrote, comment on it, and move on:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%post client

# See if they&#39;ve installed amanda before...
# If they have, none of this should be necessary...

if [ &quot;$1&quot; = 1 ];
then

            </code></pre></td>
</tr>
</tbody>
</table>

First, we start the script with a **%post** statement, and indicate that
this script is for the `client` subpackage. As the comments indicate, we
only want to perform the following tasks if this is the first time the
client subpackage has been installed. To do this, we use the first and
only argument passed to the script. It is a number indicating how many
instances of this package will be installed after the current
installation is complete.

If the argument is equal to 1, that means that no other instances of the
client subpackage are presently installed, and that this one is the
first. Let's continue:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># Set disk devices so that bin can read them
# (This is actually done on Red Hat Linux; only need to add bin to
#  group disk)

if grep &quot;^disk::.*bin&quot; /etc/group &gt; /dev/null
then
        true
else

# If there are any members in group disk, add bin after a comma...
 sed -e &#39;s/\(^disk::[0-9]\{1,\}:.\{1,\}\)/\1,bin/&#39; /etc/group &gt; /etc/group.tmp

# If there are no members in group disk, add bin...
sed -e &#39;s/\(^disk::[0-9]\{1,\}:$\)/\1bin/&#39; /etc/group.tmp &gt; /etc/group

# clean up!
 rm -f /etc/group.tmp
fi

            </code></pre></td>
</tr>
</tbody>
</table>

One of amanda's requirements is that the user ID running the dumps on
the client needs to be able to read from every disk's device file. The
folks at Red Hat have done half the work for us by creating a group
`disk` and giving that group read/write access to every disk device.
Since our dumpuser is `bin`, we only need to add `bin` to the `disk`
group. Two lines of **sed**, and we're done\!

The next section is related to the last. It also focuses on making sure
`bin` can access everything it needs while doing backups:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># Also set /etc/dumpdates to be writable by group disk

chgrp disk /etc/dumpdates
chmod g+w /etc/dumpdates

            </code></pre></td>
</tr>
</tbody>
</table>

Since amanda uses **dump** to obtain the backups, and since **dump**
keeps track of the backups in `/etc/dumpdates`, it's only natural that
**bin** will need read/write access to the file. In a perfect world,
`/etc/dumpdates` would have already been set to allow group `disk` to
read and write, but we had to do it ourselves. It's not a big problem,
though.

Next, we need to create the appropriate network-related entries, so that
amanda clients can communicate with amanda servers, and vice versa:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># Add amanda line to /etc/services

if grep &quot;^amanda&quot; /etc/services &gt;/dev/null
then
        true
else
        echo &quot;amanda    10080/udp # Added by package amanda-client&quot; &gt;&gt;
/etc/services
fi

            </code></pre></td>
</tr>
</tbody>
</table>

By using **grep** to look for lines that begin with the letters
**amanda**, we can easily see if `/etc/services` is already configured
properly. It it isn't, we simply append a line to the end.

We also added a comment so that sysadmins will know where the entry came
from, and either take our word for it or issue an **rpm -q --scripts
amanda-client** command and see for themselves. We did it all on one
line because it makes the script simpler.

Let's look at the rest of the network-related part of this script:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># Add amanda line to /etc/inetd.conf

if grep &quot;^amanda&quot; /etc/inetd.conf &gt;/dev/null
then
        true 
else
        echo &quot;amanda dgram udp wait bin /usr/lib/amanda/amandad amandad
  # added by package amanda-client&quot; &gt;&gt;/etc/inetd.conf

# Kick inetd

if [ -f /var/run/inetd.pid ];
then
        kill -HUP `cat /var/run/inetd.pid`
fi
fi
fi

            </code></pre></td>
</tr>
</tbody>
</table>

Here, we've used the same approach to add an entry to `/etc/inetd.conf`.
We then HUP **inetd** so the change will take affect, and we're done\!

Oh, and that last **fi** at the end? That's to close the **if \[ "$1" =
1 \]** at the start of the script. Now let's look at the server's
post-install script:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%post server

# See if they&#39;ve installed amanda before...

if [ &quot;$1&quot; = 1 ];
then

# Add amanda line to /etc/services

if grep &quot;^amanda&quot; /etc/services &gt;/dev/null
then
        true
else
        echo &quot;amanda    10080/udp # Added by package amanda-server&quot;
 &gt;&gt;/etc/services
fi

fi

            </code></pre></td>
</tr>
</tbody>
</table>

That was short\! And this huge difference brings up a good point about
writing install scripts: It's important to understand what you as the
package builder should do for the user, and what they should do for
themselves.

In the case of the client package, every one of the steps performed by
the post-install script was something that a fairly knowledgeable user
could have done. But each of these steps have one thing in common. No
matter how the user configures amanda, these steps will never change.
And given the nature of client/server applications, there's a good
chance that *many* more amanda client packages will be installed than
amanda servers. Would *you* like to be tasked with installing this
package on twenty systems, and performing each of the steps we've
automated, twenty times? We thought not.

There is one step that we did *not* automate for the client package. The
step we left out is the creation of a `.rhosts` file. Since this file
must contain the name of the amanda server, we have no way of knowing
what the file should look like. Therefore, that's one step we can't
automate.

The server's post-install script is so short because there's little else
that can be automated. The other steps required to set up an amanda
server include:

1.  Choosing a configuration name, which requires user input

2.  Creating a directory to hold the server configuration files, named
    according to the configuration name, which depends on the first step

3.  Modifying example configuration files to suit the site, which
    requires user input

4.  Adding crontab entries to run amanda nightly, which requires user
    input

Since every step depends on the user making decisions, the best way to
handle them is to not handle them at all. Let the user do it\!

</div>

<div class="sect3">

### <span id="s3-rpm-rw-build-uninstall-scripts">Creating Uninstall Scripts</span>

Where there are install scripts, there are uninstall scripts. While
there is no ironclad rule to that effect, it is a good practice.
Following this practice, we have an uninstall script for the client
package, and one for the server. Let's take the client first:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%postun client

# First, see if we&#39;re the last amanda-client package on the system...
# If not, then we don&#39;t need to do this stuff...

if [ &quot;$1&quot; = 0 ];
then

            </code></pre></td>
</tr>
</tbody>
</table>

As before, we start out with a declaration of the type of script this
is, and which subpackage it is for. Following that we have an **if**
statement similar to the one we used in the install scripts, save one
difference. Here, we're comparing the argument against zero. The reason
is that we are trying to see if there will be zero instances of this
package after the uninstall is complete. If this is the case, the
remainder of the script needs to be run, since there are no other amanda
client packages left.

Next, we remove `bin` from the `disk` group:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># First, get rid of bin from the disk group...

if grep &quot;^disk::.*bin&quot; /etc/group &gt; /dev/null
then

#       Nuke bin at the end of the line...
        sed -e &#39;s/\(^disk::[0-9]\{1,\}:.\{1,\}\),bin$/\1/&#39; /etc/group &gt; /etc/group.tmp

#       Nuke bin on the line by itself...
        sed -e &#39;s/\(^disk::[0-9]\{1,\}:\)bin$/\1/&#39; /etc/group.tmp &gt; /etc/group1.tmp

#       Nuke bin in the middle of the line...
        sed -e &#39;s/\(^disk::[0-9]\{1,\}:.\{1,\}\),bin,\(.\{1,\}\)/\1,\2/&#39; /etc/group1.tmp &gt; /etc/group2.tmp

#       Nuke bin at the start of the line...
        sed -e &#39;s/\(^disk::[0-9]\{1,\}:\)bin,\(.\{1,\}\)/\1\2/&#39; /etc/group2.tmp &gt; /etc/group

#       Clean up after ourselves...
        rm -f /etc/group.tmp /etc/group1.tmp /etc/group2.tmp
fi

            </code></pre></td>
</tr>
</tbody>
</table>

No surprises there. Continuing our uninstall, we start on the
network-related tasks:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># Next, lose the amanda line in /etc/services...
# We only want to do this if the server package isn&#39;t installed
# Look for /usr/sbin/amdump, and leave it if there...

if [ ! -f /usr/sbin/amdump ];
then

        if grep &quot;^amanda&quot; /etc/services &gt; /dev/null
        then
                grep -v &quot;^amanda&quot; /etc/services &gt; /etc/services.tmp
                mv -f /etc/services.tmp /etc/services
        fi
fi

            </code></pre></td>
</tr>
</tbody>
</table>

That's odd. Why are we looking for a file from the server package? If
you look back at the install scripts for the client and server packages,
you'll find that the one thing they have in common is that both the
client and the server require the same entry in `/etc/services`.

If an amanda server is going to back itself up, it also needs the amanda
client software. Therefore, both subpackages need to add an entry to
`/etc/services`. But what if one of the packages is removed? Perhaps the
server is being demoted to a client, or maybe the server is no longer
going to be backed up using amanda. In these cases, the entry in
`/etc/services` must stay. So, in the case of the client, we look for a
file from the server subpackage, and if it's there, we leave the entry
alone.

Granted, this is a somewhat unsightly way to see if a certain package is
installed. Some of you are probably even saying, "Why can't RPM be used?
Just do an **rpm -q amanda-server**, and decide what to do based on
that." And that would be the best way to do it, except for one small
point:

Only one invocation of RPM can run at any given time.

Since RPM is running to perform the uninstall, if the uninstall-script
were to attempt to run RPM again, it would fail. The reason it would
fail is because only one copy of RPM can access the database at a time.
So we are stuck with our unsightly friend.

Continuing the network-related uninstall tasks:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># Finally, the amanda entry in /etc/inetd.conf

if grep &quot;^amanda&quot; /etc/inetd.conf &gt; /dev/null
then
        grep -v &quot;^amanda&quot; /etc/inetd.conf &gt; /etc/inetd.conf.tmp
        mv -f /etc/inetd.conf.tmp /etc/inetd.conf

# Kick inetd

if [ -f /var/run/inetd.pid ];
then
        kill -HUP `cat /var/run/inetd.pid`
fi
fi

fi

            </code></pre></td>
</tr>
</tbody>
</table>

Here, we're using **grep**'s ability to return lines that *don't* match
the search string, in order to remove every trace of amanda from
`/etc/inetd.conf`. After issuing a HUP on inetd, we're done.

On to the server. If you've been noticing a pattern between the various
scripts, you won't be disappointed here:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%postun server

# See if we&#39;re the last server package on the system...
# If not, we don&#39;t need to do any of this stuff...

if [ &quot;$1&quot; = 0 ];
then

# Lose the amanda line in /etc/services...
# We only want to do this if the client package isn&#39;t installed
# Look for /usr/lib/amandad, and leave it if there...

if [ ! -f /usr/lib/amanda/amandad ];
then

        if grep &quot;^amanda&quot; /etc/services &gt; /dev/null
        then
                grep -v &quot;^amanda&quot; /etc/services &gt; /etc/services.tmp
                mv -f /etc/services.tmp /etc/services
        fi
fi

fi

            </code></pre></td>
</tr>
</tbody>
</table>

By now the opening **if** statement is an old friend. As you might have
expected, we are verifying whether the client package is installed, by
looking for a file from that package. If the client package isn't there,
the entry is removed from `/etc/services`. And that, is that.

Obviously, these scripts must be carefully tested. In the case of
amanda, since the two subpackages have some measure of interdependency,
it's necessary to try different sequences of installing and erasing the
two packages to make sure the `/etc/services` logic works properly in
all cases.

After a bit of testing, our install and uninstall scripts pass with
flying colors. From a technological standpoint, the client and server
subpackages are ready to go.

</div>

<div class="sect3">

### <span id="s3-rpm-rw-build-bits-and-pieces">Bits and Pieces</span>

However, just because a package has been properly built, and installs
and can be erased without problems, doesn't mean that the package
builder's job is done. It's necessary to look at each newly-built
package from the user's perspective. Does the package contain everything
the user needs in order to deploy it effectively? Or will the user need
to fiddle with it, guessing as they go?

In the case of our amanda packages, it was obvious that some additional
documentation was required so that the user would know what needed to be
done in order to finalize the installation. Simply directing the user to
the standard amanda documentation wasn't the right solution, either.
Many of the steps outlined in the `INSTALL` document had already been
done by the post-install scripts. No, an interim document was required.
Two, actually: one for the client, and one for the server.

So two files were created, one to be added to each subpackage. The
question was, how to do it? Essentially, there were two options:

1.  Put the files in the amanda directory tree that had been used to
    perform the initial builds and generate a new patch file

2.  Create a **tar** file containing the two files, and modify the spec
    file to unpack the documentation into the amanda directory tree.

3.  Drop the files directly into the amanda directory tree without using
    **tar**.

Since the second approach was more interesting, that's the approach we
chose. It required an additional **source** tag in the spec file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Source1: amanda-rpm-instructions.tar.gz

            </code></pre></td>
</tr>
</tbody>
</table>

Also required was an additional **%setup** macro in the **%prep**
script:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%setup -T -D -a 1

            </code></pre></td>
</tr>
</tbody>
</table>

While the **%setup** macro might look intimidating, it wasn't that hard
to construct. Here's what each options means:

  - **-T** — Do not perform the default archive unpacking.

  - **-D** — Do not delete the directory before unpacking.

  - **-a1** — Unpack the archive specified by the **source1** tag after
    changing directory.

Finally, two additions to the **%files** lists were required. One for
the client:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%doc amanda-client.README

            </code></pre></td>
</tr>
</tbody>
</table>

And one for the server:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%doc amanda-server.README

            </code></pre></td>
</tr>
</tbody>
</table>

At this point, the packages were complete. Certainly there is software
out there that doesn't require this level of effort to package. Just as
certainly there is software that is much more of a challenge. Hopefully
this chapter has given you some idea about how to approach package
building for more complex applications.

</div>

</div>

</div>

<div class="NAVFOOTER">

-----

|                                                     |                            |                                |
| :-------------------------------------------------- | :------------------------: | -----------------------------: |
| [Prev](s1-rpm-rw-build-initial-build-with-rpm.html) |     [Home](index.html)     |     [Next](ch-rpm-rpmlib.html) |
| Initial Building With RPM                           | [Up](ch-rpm-rw-build.html) | A Guide to the RPM Library API |

</div>
