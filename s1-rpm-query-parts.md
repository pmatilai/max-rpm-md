<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-query.html)

Chapter 5. Getting Information About Packages

[Next](s1-rpm-query-handy-queries.html)

-----

<div class="sect1">

# <span id="s1-rpm-query-parts">The Parts of an RPM Query</span>

It becomes easy to construct a query command once you understand the
individual parts. First is the **-q** (You can also use **--query**, if
you like). After all, you need to tell RPM what function to perform,
right? The rest of a query command consists of two distinct parts:
package selection (or what packages you'd like to query), and
information selection (or what information you'd like to see). Let's
take a look at package selection first:

<div class="sect2">

## <span id="s2-rpm-query-package-selection">Query Commands, Part One: Package Selection</span>

The first thing you'll need to decide when issuing an RPM query is what
package (or packages) you'd like to query. RPM has several ways to
specify packages, so you have quite an assortment to choose from.

<div class="sect3">

### <span id="s3-rpm-query-package-label">The Package Label</span>

In earlier chapters, we discussed RPM's package label, a string that
uniquely identifies every installed package. Every label contains three
pieces of information:

1.  The *name* of the packaged software.

2.  The *version* of the packaged software.

3.  The package's *release* number.

When issuing a query command using package labels, you must always
include the package name. You can also include the version and even the
release, if you like. The only restrictions are that each part of the
package label specified must be complete, and that if any parts of the
package label are missing, all parts to the right must be omitted as
well. This second restriction is just a long way of saying that if you
specify the release, you must also specify the version as well. Let's
look at a few examples.

Say, for instance, you've recently installed a new version of the C
libraries, but you can't remember the version number:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q libc
libc-5.2.18-1

#
            </code></pre></td>
</tr>
</tbody>
</table>

In this type of query, RPM returns the complete package label for all
installed packages that match the given information. In the example
above, if version 5.2.17 of the C libraries was also installed, its
package label would have been displayed, too.

In this example, we've included the version as well as the package name:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q rpm-2.3
rpm-2.3-1

#
            </code></pre></td>
</tr>
</tbody>
</table>

Note, however, that RPM is a bit picky about specifying package names.
Here are some queries for the C library that *won't* work:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q LibC
package LibC is not installed

#
# rpm -q lib
package lib is not installed

#
# rpm -q &quot;lib*&quot;
package lib* is not installed

#
# rpm -q libc-5
package libc-5 is not installed

#
# rpm -q libc-5.2.1
package libc-5.2.1 is not installed

#
            </code></pre></td>
</tr>
</tbody>
</table>

As you can see, RPM is case sensitive about package names and cannot
match partial names, version numbers, or release numbers. Nor can it use
the wildcard characters we've come to know and love. As we've seen,
however, RPM can perform the query when more than one field of the
package label is present. In the above case, **rpm -q libc-5.2.18**, or
even **rpm -q libc-5.2.18-1** would have found the package,
`libc-5.2.18-1`.

Querying based on package labels may seem a bit restrictive. After all,
you need to know the exact name of a package in order to perform a query
on it. But there are other ways of specifying packages…

</div>

<div class="sect3">

### <span id="s3-rpm-query-a-option">**-a** — Query All Installed Packages</span>

Want lots of information fast? Using the **-a** option, you can query
every package installed on your system. For example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qa
ElectricFence-2.0.5-2
ImageMagick-3.7-2
…
tetex-xtexsh-0.3.3-8
lout-3.06-4

#
            </code></pre></td>
</tr>
</tbody>
</table>

(On a system installed using RPM, the number of packages can easily
number 200 or more; we've deleted most of the output.)

The **-a** option can produce mountains of output, which makes it a
prime candidate for piping through the many Linux/UNIX commands
available. One of the prime candidates would be a pager such as
**more**, so that the list of installed packages could be viewed a
screenful at a time.

Another handy command to pipe **rpm -qa**'s output through is **grep**.
In fact, using **grep**, it's possible to get around RPM's lack of
built-in wildcard processing:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qa | grep -i sysv
SysVinit-2.64-2

#
            </code></pre></td>
</tr>
</tbody>
</table>

In this example, we were able to find the `SysVinit` package, even
though we didn't have the complete package name, or capitalization.

</div>

<div class="sect3">

### <span id="s3-rpm-query-by-file">**-f `<file>`** — Query the Package Owning **`<file>`**</span>

How many times have you found a program sitting on your system and
wondered "what does it do?" Well, if the program was installed by RPM as
part of a package, it's easy to find out. Simply use the **-f** option.
Example: You find a strange program called **ls** in `/bin` (Okay, it
*is* a contrived example). Wonder what package installed it? Simple\!

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qf /bin/ls
fileutils-3.12-3

#
            </code></pre></td>
</tr>
</tbody>
</table>

If you happen to point RPM at a file it didn't install, you'll get a
message similar to the following:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qf .cshrc
file /home/ed/.cshrc is not owned by any package

#
            </code></pre></td>
</tr>
</tbody>
</table>

<div class="sect4">

#### <span id="s4-rpm-query-tricky-detail">A Tricky Detail</span>

It's possible that you'll get the "not owned by any package" message in
error. Here's an example of how it can happen:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qf /usr/X11/bin/xterm
file /usr/X11/bin/xterm is not owned by any package

#
              </code></pre></td>
</tr>
</tbody>
</table>

As you can see, we're trying to find out what package the `xterm`
program is part of. The first example failed, which might lead one to
believe that `xterm` really *isn't* owned by any package.

However, let's look at a directory listing:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -lF /usr
…
lrwxrwxrwx   1 root     root            5 May 13 12:46 X11 -&gt; X11R6/
drwxrwxr-x   7 root     root         1024 Mar 21 00:21 X11R6/
…

#
              </code></pre></td>
</tr>
</tbody>
</table>

(We've truncated the list; normally `/usr` is quite a bit more crowded
than this.)

The key here is the line ending with "X11 -\> X11R6/". This is known as
a "symbolic link". It's a way of referring to a file (here, a directory
file) by another name. In this case, if we used the path `/usr/X11`, or
`/usr/X11R6`, it shouldn't make a difference. It certainly doesn't make
a difference to programs that simply want access to the file. But it
does make a difference to RPM, because RPM doesn't use the filename to
access the file. RPM uses the filename as a key into its database. It
would be very difficult, if not impossible, to keep track of all the
symlinks on a system and try every possible path to a file during a
query.

What to do? There are two options:

1.  Make sure you always specify a path free of symlinks. This can be
    pretty tough, though. An alternative approach is to use **namei** to
    track down symlinks:
    
    <table>
    <colgroup>
    <col style="width: 100%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td><pre class="screen"><code># namei /usr/X11/bin/xterm
    f: /usr/X11/bin/xterm
     d /
     d usr
     l X11 -&gt; X11R6
       d X11R6
     d bin
     - xterm
    
    #
                        </code></pre></td>
    </tr>
    </tbody>
    </table>
    
    It's pretty easy to see the `X11` to `X11R6` symlink. Using this
    approach you can enter the non-symlinked path and get the desired
    results:
    
    <table>
    <colgroup>
    <col style="width: 100%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td><pre class="screen"><code># rpm -qf /usr/X11R6/bin/xterm
    XFree86-3.1.2-5
    
    #
                        </code></pre></td>
    </tr>
    </tbody>
    </table>

2.  Change your directory to the one holding the file you want to query.
    Even if you use a symlinked path to get there, querying the file
    should then work as you'd expect:
    
    <table>
    <colgroup>
    <col style="width: 100%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td><pre class="screen"><code># cd /usr/X11/bin
    # rpm -qf xterm
    XFree86-3.1.2-5
    
    #
                        </code></pre></td>
    </tr>
    </tbody>
    </table>

So if you get a "not owned by any package" error, and you think it may
not be true, try one of the approaches above.

</div>

</div>

<div class="sect3">

### <span id="s3-rpm-query-p-option">**-p `<file>`** — Query a Specific RPM Package File</span>

Up to now, every means of specifying a package to an RPM query focused
on packages that had already been installed. While it's certainly very
useful to be able to dredge up information about packages that are
already on your system, what about packages that haven't yet been
installed? The **-p** option can do that for you.

One situation where this capability would help, occurs when the name of
a package file has been changed. Since the name of the file containing a
package has *nothing* to do with the name of the package (though, by
tradition it's nice to name package files consistently), we can use this
option to find out exactly what package a file contains:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qp foo.bar
rpm-2.3-1

#
            </code></pre></td>
</tr>
</tbody>
</table>

With one command RPM gives you the answer.
[<span class="footnote">\[1\]</span>](#FTN.AEN2793)

The **-p** option can also use *Uniform Resource Locators* to specify
package files. See [the Section called *URLs — Another Way to Specify
Package Files* in Chapter
2](s1-rpm-install-performing-install.html#s2-rpm-install-urls) for more
information on using URLs.

There's one last trick up **-p**'s sleeve — it can also perform a query
by reading a package from standard input. Here's an example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># cat bother-3.5-1.i386.rpm | rpm -qp -
bother-3.5-1

#
            </code></pre></td>
</tr>
</tbody>
</table>

We piped the output of **cat** into RPM. The dash at the end of the
command line directs RPM to read the package from standard input.

</div>

<div class="sect3">

### <span id="s3-rpm-query-g-option">**-g `<group>`**: Query Packages Belonging To Group **`<group>`**</span>

When a package is built, the package builder must classify the package,
grouping it with other packages that perform similar functions. RPM
gives you the ability to query installed packages based on their groups.
For example, there is a group known as `Base`. This group consists of
packages that provide low-level structure for a Linux distribution.
Let's see what installed packages make up the `Base` group:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qg Base
setup-1.5-1
pamconfig-0.50-5
filesystem-1.2-1
crontabs-1.3-1
dev-2.3-1
etcskel-1.1-1
initscripts-2.73-1
mailcap-1.0-3
pam-0.50-17
passwd-0.50-2
redhat-release-4.0-1
rootfiles-1.3-1
termcap-9.12.6-5

#
            </code></pre></td>
</tr>
</tbody>
</table>

One thing to keep in mind is that group specifications are
case-sensitive. Issuing the command **rpm -qg base** won't produce any
output.

</div>

<div class="sect3">

### <span id="s3-rpm-query-whatprovides-option">**--whatprovides `<x>`**: Query the Packages That Provide Capability **`<x>`**</span>

RPM provides extensive support for dependencies between packages. The
basic mechanism used is that a package may *require* what another
package *provides*. The thing that is required and provided can be a
shared library's soname. It can also be a character string chosen by the
package builder. In any case, it's important to be able to display which
packages provide a given capability.

This is just what the **--whatprovides** option does. When the option,
followed by a capability, is added to a query command, RPM will select
those packages that provide the capability. Here's an example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --whatprovides module-info
kernel-2.0.18-5

#
            </code></pre></td>
</tr>
</tbody>
</table>

In this case, the only package that provides the `module-info`
capability is `kernel-2.0.18-5`.

</div>

<div class="sect3">

### <span id="s3-rpm-query-whatrequires">**--whatrequires `<x>`**: Query the Packages That Require Capability **`<x>`**</span>

The **--whatrequires** option is the logical complement to the
**--whatprovides** option described above. It is used to display which
packages require the specified capability. Expanding on the example we
started with **--whatprovides**, let's see which packages require the
`module-info` capability:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --whatrequires module-info
kernelcfg-0.3-2

#
            </code></pre></td>
</tr>
</tbody>
</table>

There's only one package that requires `module-info` —
`kernelcfg-0.3-2`.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-query-information-selection">Query Commands, Part Two: Information Selection</span>

After specifying the package (or packages) you wish to query, you'll
need to figure out just *what* information you'd like RPM to retrieve.
As we've seen, by default, RPM only returns the complete package label.
But there's much more to a package than that. Here, we'll explore every
information selection option available to us.

<div class="sect3">

### <span id="s3-rpm-query-i-option">**-i** — Display Package Information</span>

Adding **-i** to **rpm -q** tells RPM to give you some information on
the package or packages you've selected. For the sake of clarity, let's
take a look at what it gives you and explain what you're looking at:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qi rpm
Name        : rpm                    Distribution: Red Hat Linux Vanderbilt
Version     : 2.3                          Vendor: Red Hat Software
Release     : 1                        Build Date: Tue Dec 24 09:07:59 1996
Install date: Thu Dec 26 23:01:51 1996 Build Host: porky.redhat.com
Group       : Utilities/System         Source RPM: rpm-2.3-1.src.rpm
Size        : 631157
Summary     : Red Hat Package Manager
Description :
RPM is a powerful package manager, which can be used to build, install,
query, verify, update, and uninstall individual software packages. A
package consists of an archive of files, and package information, including
name, version, and description.

# 
            </code></pre></td>
</tr>
</tbody>
</table>

There's quite a bit of information here, so let's go through it entry by
entry:

  - Name — The name of the package you queried. Usually (but not always)
    it bears some resemblance to the name of the underlying software.

  - Version — The version number of the software, as specified by the
    software's original creator.

  - Release — The number of times a package consisting of this software
    has been packaged. If the version number should change, the release
    number should start over again at "1".

As you've probably noticed, these three pieces of information comprise
the package label we've come to know and love. Continuing, we have:

  - Install date — This is the time when the package was installed on
    your system.

  - Group — In our example, this looks suspiciously like a path. If you
    went searching madly for a directory tree by that name, you'd come
    up dry — it isn't a set of directories at all.
    
    When a package builder starts to create a new package, they enter a
    list of words that describe the software. The list, which goes from
    least specific to most specific, attempts to categorize the software
    in a concise manner. The primary use for the group is to enable
    graphically oriented package managers based on RPM to present
    packages grouped by function. Red Hat Linux's **glint** command does
    this.

  - Size — This is the size (in bytes) of every file in this package. It
    might make your decision to erase an unused package easier if you
    see six or more digits here.

  - Summary — This is a concise description of the packaged software.

  - Description — This is a verbose description of the packaged
    software. Some descriptions might be more, well, *descriptive* than
    others, but hopefully it will be enough to clue you in as to the
    software's role in the greater scheme of things.

  - Distribution — The word "distribution" is really not the best name
    for this field. "Product" might be a better choice. In any case,
    this is the name of the product this package is a part of.

  - Vendor — The organization responsible for building this package.

  - Build Date — The time the package was created.

  - Build Host — The name of the computer system that built the package.
    [<span class="footnote">\[2\]</span>](#FTN.AEN2938)

  - Source RPM — The process of building a package results in two files:
    
    1.  The package file used to install the packaged software. This is
        sometimes called the *binary* package.
    
    2.  The package file containing the source code and other files used
        to create the binary package file. This is known as the *source*
        RPM package file. This is the filename that is displayed in this
        field.
    
    Unless you want to make changes to the software, you probably won't
    need to worry about source packages. But if you do, stick around,
    because the second part of this book is for you…

</div>

<div class="sect3">

### <span id="s3-rpm-query-l-option">**-l** — Display the Package's File List</span>

Adding **-l** to **rpm -q** tells RPM to display the list of files that
are installed by the specified package or packages. If you've used
**ls** before, you won't be surprised by RPM's file list.

Here's a look at one of the smaller packages on Red Hat Linux —
`adduser`:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -ql adduser
/usr/sbin/adduser

#
            </code></pre></td>
</tr>
</tbody>
</table>

The `adduser` package consists of only one file, so there's only one
filename displayed.

<div class="sect4">

#### <span id="s4-rpm-query-v-option">**-v** — Display Additional Information</span>

In some cases, the **-v** option can be added to a query command for
additional information. The **-l** option we've been discussing is an
example of just such a case. Note how the **-v** option adds verbosity:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qlv adduser
-rwxr-xr-x-     root     root       3894 Feb 25 13:45 /usr/sbin/adduser

#
              </code></pre></td>
</tr>
</tbody>
</table>

Looks a lot like the output from **ls**, doesn't it? Looks can be
deceiving. Everything you see here is straight from RPM's database.
However, the format is identical to **ls**, so it's more easily
understood. If this is Greek to you, consult the **ls** man page.

</div>

</div>

<div class="sect3">

### <span id="s3-rpm-query-c-option">**-c** — Display the Package's List of Configuration Files</span>

When **-c** is added to an **rpm -q** command, RPM will display the
configuration files that are part of the specified package or packages.
As mentioned earlier in the book, config files are important, because
they control the behavior of the packaged software. Let's take a look at
the list of config files for `XFree86`:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qc XFree86
/etc/X11/fs/config
/etc/X11/twm/system.twmrc
/etc/X11/xdm/GiveConsole
/etc/X11/xdm/TakeConsole
/etc/X11/xdm/Xaccess
/etc/X11/xdm/Xresources
/etc/X11/xdm/Xservers
/etc/X11/xdm/Xsession
/etc/X11/xdm/Xsetup_0
/etc/X11/xdm/chooser
/etc/X11/xdm/xdm-config
/etc/X11/xinit/xinitrc
/etc/X11/xsm/system.xsm
/usr/X11R6/lib/X11/XF86Config

#
            </code></pre></td>
</tr>
</tbody>
</table>

These are the files you'd want to look at first if you were looking to
customize XFree86 for your particular needs. Just like **-l**, we can
also add **v** for more information:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qcv XFree86
-r--r--r---  root  root    423 Mar 21 00:17 /etc/X11/fs/config
…
lrwxrwxrwx-  root  root     30 Mar 21 00:29 /usr/X11R6/lib/X11/XF86Config -&gt; ../../../../etc/X11/XF86Config

#
            </code></pre></td>
</tr>
</tbody>
</table>

(Note that last file: RPM will display symbolic links, as well.)

</div>

<div class="sect3">

### <span id="s3-rpm-query-d-option">**-d** — Display a List of the Package's Documentation</span>

When **-d** is added to a query, we get a list of all files containing
documentation for the named package or packages. This is a great way to
get up to speed when you're having problems with unfamiliar software. As
with **-c** and **-l**, you'll see either a simple list of filenames, or
(if you've added **-v**) a more comprehensive list. Here's an example
that might look daunting at first, but really isn't:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qdcf /sbin/dump
/etc/dumpdates
/usr/doc/dump-0.3-5
/usr/doc/dump-0.3-5/CHANGES
/usr/doc/dump-0.3-5/COPYRIGHT
/usr/doc/dump-0.3-5/INSTALL
/usr/doc/dump-0.3-5/KNOWNBUGS
/usr/doc/dump-0.3-5/THANKS
/usr/doc/dump-0.3-5/dump-0.3.announce
/usr/doc/dump-0.3-5/dump.lsm
/usr/doc/dump-0.3-5/linux-1.2.x.patch
/usr/man/man8/dump.8
/usr/man/man8/rdump.8
/usr/man/man8/restore.8
/usr/man/man8/rmt.8
/usr/man/man8/rrestore.8

#
            </code></pre></td>
</tr>
</tbody>
</table>

Let's take that alphabet soup set of options, one letter at a time:

  - **q** — Perform a query.

  - **d** — List all documentation files.

  - **c** — List all config files.

  - **f** — Query the package that owns the specified file
    (`/sbin/dump`, in this case).

The list of files represents all the documentation and config files that
apply to the package owning `/sbin/dump`.

</div>

<div class="sect3">

### <span id="s3-rpm-query-s-option">**-s** — Display the State of Each File in the Package</span>

Unlike the past three sections, which dealt with a list of files of one
type or another, adding **-s** to a query will list the *state* of the
files that comprise one or more packages. I can hear you out there;
you're saying, "What is the *state* of a file?" For every file that RPM
installs, there is an associated state. There are four possible states:

1.  normal — A file in the normal state has not been modified by
    installing another package on the system.

2.  replaced — Files in the replaced state have been modified by
    installing another package on the system.

3.  not installed — A file is classified as not installed when it, er,
    isn't installed\! This state is normally seen only if the package
    was partially installed. An example of a partially installed package
    would be one that was installed with the **--excludedocs** option.
    Using this option, no documentation files would be installed. The
    RPM database would still contain entries for these missing files,
    but their state would be not installed.

4.  net shared — The net shared state is used to support client systems
    that NFS mount portions of their filesystems from a server. Since
    the server most likely exports filesystems to more than one client,
    if a client erased a package that contained files on a shared
    filesystem, other client systems would have incompletely installed
    packages. The net shared state is used to alert RPM to the fact that
    a file is on a shared filesystem and should not be erased. Files
    will be in the net shared state when two things happen:
    
    1.  The **netsharedpath** `rpmrc` file entry has been changed from
        its default (null) value.
        [<span class="footnote">\[3\]</span>](#FTN.AEN3090)
    
    2.  The file is to be installed in a directory within a net shared
        path.

Here's an example showing how file states appear:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qs adduser
normal        /usr/sbin/adduser

#
            </code></pre></td>
</tr>
</tbody>
</table>

(That normal at the start of the line is the state, followed by the file
name)

The file state is one of the tools RPM uses to determine the most
appropriate action to take when packages are installed or erased.

Now would the average person need to check the states of files? Not
really. But if there should be problems, this kind of information can
help get things back on track.

</div>

<div class="sect3">

### <span id="s3-rpm-query-provides-option">**--provides**: Display Capabilities Provided by the Package</span>

By adding **--provides** to a query command, we can see the capabilities
provided by one or more packages. If the package doesn't provide any
capabilities, the **--provides** option produces no output:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --provides rpm
#
            </code></pre></td>
</tr>
</tbody>
</table>

However, if a package *does* provide capabilities, they will be
displayed:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --provides foonly
index

#
            </code></pre></td>
</tr>
</tbody>
</table>

It's important to remember that capabilities are *not* filenames. In the
above example, the `foonly` package contains no file called `index`;
it's just a character string the package builder chose. This is no
different from the following example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --provides libc
libm.so.5
libc.so.5

#
            </code></pre></td>
</tr>
</tbody>
</table>

While there might be symlinks by those names in `/lib`, capabilities are
a property of the *package*, not a file contained in the package\!

</div>

<div class="sect3">

### <span id="s3-rpm-query-requires-option">**--requires**: Display Capabilities Required by the Package</span>

The **--requires** option (**-R** is equivalent) is the logical
complement to the **--provides** option. It displays the capabilities
required by the specified package(s). If a package has no requirements,
there's no output:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --requires adduser
#
            </code></pre></td>
</tr>
</tbody>
</table>

In cases where there *are* requirements, they are displayed as follows:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --requires rpm
libz.so.1
libdb.so.2
libc.so.5

#
            </code></pre></td>
</tr>
</tbody>
</table>

It's also possible that you'll come across something like this:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --requires blather
bother &gt;= 3.1

#
            </code></pre></td>
</tr>
</tbody>
</table>

Packages may also be built to require another package. This requirement
can also include specific versions. In the example above, the `bother`
package is required by `blather`; specifically, a version of `bother`
greater than or equal to 3.1.

Here's something worth understanding. Let's say we decide to track down
the `bother` that `blather` says it requires. If we use RPM's query
capabilities, we could use the **--whatprovides** package selection
option to try to find it:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --whatprovides bother
no package provides bother

#
            </code></pre></td>
</tr>
</tbody>
</table>

No dice. This might lead you to believe that the `blather` package has a
problem. The moral of this story is that, when trying to find out what
package fulfills another package's requirements, it's a good idea to
also try a simple query using the requirement as a package name.
Continuing our example above, let's see if there's a package called
`bother`:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q bother
bother-3.5-1

#
            </code></pre></td>
</tr>
</tbody>
</table>

Bingo\! However, if we see what capabilities the `bother` package
provides, we come up dry:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --provides bother
#
            </code></pre></td>
</tr>
</tbody>
</table>

The reason for the lack of output is that all packages, by default,
"provide" their package name (and version).

</div>

<div class="sect3">

### <span id="s3-rpm-query-dump-option">**--dump**: Display All Verifiable Information for Each File</span>

The **--dump** option is used to display every piece of information RPM
has on one or more files listed in its database. The information is
listed in a very concise fashion. Since the **--dump** option displays
file-related information, the list of files must be chosen by using the
**-l**, **-c**, or **-d** options (or some combination thereof):

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -ql --dump adduser
/usr/sbin/adduser 4442 841083888 ca5fa53dc74952aa5b5e3a5fa5d8904b 0100755
root root 0 0 0 X

#
            </code></pre></td>
</tr>
</tbody>
</table>

What does all this stuff mean? Let's go through it, item-by-item:

  - The `/usr/sbin/adduser` is simple: it's the name of the file being
    **dump**'ed.

  - 4442 is the size of the file, in bytes.

  - How about 841083888? It's the time the file was last modified, in
    seconds past the Unix zero date of January 1, 1970.

  - The ca5fa53dc74952aa5b5e3a5fa5d8904b is the MD5 checksum of the
    file's contents, all 128 bits of it.

  - If you guessed 0100755 was the file's mode, you'd be right.

  - The first root represents the file's owner.

  - The second root is the file's group.

  - We'll take the next part (0 0) in one chunk. The first zero shows
    whether the file is a config file. If zero, as in this case, then
    the file is not a config file. The next zero shows whether the file
    is documentation. Again, since there is a zero here, this file isn't
    documentation, either.

  - The final 0 represents the file's major and minor numbers. These are
    set only for device special files. Otherwise, it will be zero.

  - If the file were a symlink, the spot taken by the X would contain a
    path pointing to the linked file.

Normally, the **--dump** option is used by people that want to extract
the file-related information from RPM and process it somehow.

</div>

<div class="sect3">

### <span id="s3-rpm-query-scripts-option">**--scripts** — Show Scripts Associated With a Package</span>

If you add **--scripts** (that's *two* dashes) to a query, you get to
see a little bit more of RPM's underlying magic:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --scripts XFree86
preinstall script:
(none)

postinstall script:
/sbin/ldconfig
/sbin/pamconfig --add --service=xdm --password=none --sesslist=none

preuninstall script:
(none)

postuninstall script:
/sbin/ldconfig
if [ &quot;$1&quot; = 0 ] ; then
  /sbin/pamconfig --remove --service=xdm --password=none --sesslist=none
fi

verify script:
(none)


#
            </code></pre></td>
</tr>
</tbody>
</table>

In this particular case, the `XFree86` package has two scripts: one
labeled `postinstall`, and one labeled `postuninstall`. As you might
imagine, the postinstall script is executed just after the package's
files have been installed; the postuninstall script is executed just
after the package's files have been erased.

Based on the labels in this example, you'd probably imagine that a
package can have as many as five different scripts. You'd be right:

1.  The preinstall script, which is executed just *before* the package's
    files are installed.

2.  The postinstall script, which is executed just *after* the package's
    files are installed.

3.  The preuninstall script, which is executed just *before* the
    package's files are removed.

4.  The postuninstall script, which is executed just *after* the
    package's files are removed.

5.  And finally, the verify script. While it's easy to figure out the
    other scripts' functions based on their name, what does a script
    called *verify* do? Well, we haven't gotten to it yet, but packages
    can also be verified for proper installation. This script is used
    during verification.
    [<span class="footnote">\[4\]</span>](#FTN.AEN3279)

Is this something you'll need very often? As in the case of displaying
file states, not really. But when you need it, you *really* need it\!

</div>

<div class="sect3">

### <span id="s3-rpm-query-queryformat-option">**--queryformat** — Construct a Custom Query Response</span>

OK, say you're *still* not satisfied. You'd like some additional
information, or you think a different format would be easier on the
eyes. Maybe you want to take some information on the packages you've
installed and run it through a script for some specialized processing.
You can do it, using the **--queryformat** option. In fact, if you look
back at the output of the **-i** option, RPM was using **--queryformat**
internally. Here's how it works:

On the RPM command line, include **--queryformat**. Right after that,
enter a format string, enclosed in single quotes "'".

The format string can consist of a number of different components:

  - Literal text, including escape sequences.

  - Tags, with optional field width, formatting, and iteration
    information.

  - Array Iterators.

Let's look at each of these components.

<div class="sect4">

#### <span id="s4-rpm-query-queryformat-literal-text">Literal text</span>

Any part of a format string that is not associated with tags or array
iterators will be treated as literal text. Literal text is just that:
It's text that is printed just as it appears in the format string. In
fact, a format string can consist of nothing but literal text, although
the output wouldn't tell us much about the packages being queried. Let's
give the **--queryformat** option a try, using a format string with
nothing but literal text:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --queryformat &#39;This is a test!&#39; rpm
This is a test!#
              </code></pre></td>
</tr>
</tbody>
</table>

The RPM command might look a little unusual, but if you take out the
**--queryformat** option, along with its format string, you'll see this
is just an ordinary query of the `rpm` package. When the
**--queryformat** option is present, RPM will use the text immediately
following the option as a format string. In our case, the format string
is **'This is a test\!'**. The single quotes are required. Otherwise,
it's likely your shell will complain about some of the characters
contained in the average format string.

The output of this command appears on the second line. As we can see,
the literal text from the format string was printed exactly as it was
entered.

</div>

<div class="sect4">

#### <span id="s4-rpm-query-queryformat-carriage-control">Carriage Control Escape Sequences</span>

Wait a minute. What is that \# doing at the end of the output? Well,
that's our shell prompt. You see, we didn't direct RPM to move to a new
line after producing the output, so the shell prompt ended up being
tacked to the end of our output.

Is there a way to fix that? Yes, there is. We need to use an escape
sequence. An escape sequence is a sequence of characters that starts
with a backslash (**\\**). Escape sequences add carriage control
information to a format string. The following escape sequences can be
used:

  - **\\a** — Produces a bell or similar alert.

  - **\\b** — Backspaces one character.

  - **\\f** — Outputs a form-feed character.

  - **\\n** — Outputs a newline character sequence.

  - **\\r** — Outputs a carriage return character.

  - **\\t** — Causes a horizontal tab.

  - **\\v** — Causes a vertical tab.

  - **\\\\** — Displays a backslash character.

Based on this list, it seems that a **\\n** escape sequence at the end
of the format string will put our shell prompt on the next line:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --queryformat &#39;This is a test!\n&#39; rpm
This is a test!

#
              </code></pre></td>
</tr>
</tbody>
</table>

Much better…

</div>

<div class="sect4">

#### <span id="s4-rpm-query-queryformat-tags">Tags</span>

The most important parts of a format string are the tags. Each tag
specifies what information is to be displayed and can optionally include
field-width, as well as justification and data formatting instructions.
[<span class="footnote">\[5\]</span>](#FTN.AEN3375) But for now, let's
look at the basic tag. In fact, let's look at three — the tags that
print the package name, version, and release.

Strangely enough, these tags are called **NAME**, **VERSION**, and
**RELEASE**. In order to be used in a format string, the tag names must
be enclosed in curly braces and preceded by a percent sign. Let's give
it a try:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --queryformat &#39;%{NAME}%{VERSION}%{RELEASE}\n&#39; rpm
rpm2.31

#
              </code></pre></td>
</tr>
</tbody>
</table>

Let's add a dash between the tags and see if that makes the output a
little easier to read:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --queryformat &#39;%{NAME}-%{VERSION}-%{RELEASE}\n&#39; rpm
rpm-2.3-1

#
              </code></pre></td>
</tr>
</tbody>
</table>

Now our format string outputs standard package labels.

<div class="sect5">

##### <span id="s5-rpm-query-queryformat-widths">Field Width and Justification</span>

Sometimes it's desirable to allocate fields of a particular size for a
tag. This is done by putting the desired field width between the tag's
leading percent sign, and the opening curly brace. Using our
package-label-producing format string, let's allocate a 20-character
field for the version:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --queryformat &#39;%{NAME}-%20{VERSION}-%{RELEASE}\n&#39; rpm
rpm-                 2.3-1

#
                </code></pre></td>
</tr>
</tbody>
</table>

The result is a field of 20 characters: 17 spaces, followed by the three
characters that make up the version.

In this case, the version field is right justified; that is, the data is
printed at the far right of the output field. We can left justify the
field by preceding the field width specification with a dash:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --queryformat &#39;%{NAME}-%-20{VERSION}-%{RELEASE}\n&#39; rpm
rpm-2.3                 -1

#
                </code></pre></td>
</tr>
</tbody>
</table>

Now the version is printed at the far left of the output field. You
might be wondering what would happen if the field width specification
didn't leave enough room for the data being printed. The field width
specification can be considered the *minimum* width the field will take.
If the data being printed is wider, the field will expand to accommodate
the data.

</div>

<div class="sect5">

##### <span id="s5-rpm-query-queryformat-modifiers">Modifiers — Making Data More Readable</span>

While RPM does its best to appropriately display the data from a
**--queryformat**, there are times when you'll need to lend a helping
hand. Here's an example. Say we want to display the name of each
installed package, followed by the time the package was installed.
Looking through the available tags, we see **INSTALLTIME**. Great\!
Looks like this will be simple:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qa --queryformat &#39;%{NAME} was installed on %{INSTALLTIME}\n&#39;
setup was installed on 845414601
pamconfig was installed on 845414602
filesystem was installed on 845414607
…
rpm was installed on 851659311
pgp was installed on 846027549

#
                </code></pre></td>
</tr>
</tbody>
</table>

Well, that's a *lot* of output, but not very useful. What *are* those
numbers? RPM didn't lie -- they're the time the packages were installed.
The problem is, the times are being displayed in their numeric form used
internally by the operating system, and humans like to see the day,
month, year, and so on.

Fortunately, there's a modifier for just this situation. The name of the
modifier is **:date**, and it follows the tag name. Let's try our
example again, this time using **:date**:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qa --queryformat &#39;%{NAME} was installed on %{INSTALLTIME:date}\n&#39;
setup was installed on Tue Oct 15 17:23:21 1996
pamconfig was installed on Tue Oct 15 17:23:22 1996
filesystem was installed on Tue Oct 15 17:23:27 1996
…
rpm was installed on Thu Dec 26 23:01:51 1996
pgp was installed on Tue Oct 22 19:39:09 1996

#
                </code></pre></td>
</tr>
</tbody>
</table>

That sure is a lot easier to understand, isn't it?

Here's a list of the available modifiers:

  - The **:date** modifier displays dates in human-readable form. It
    transforms 846027549 into Tue Oct 22 19:39:09 1996.

  - The **:perms** modifier displays file permissions in an easy-to-read
    format. It changes -32275 to -rwxr-xr-x-.

  - The **:depflags** modifier displays the version comparison flags
    used in dependency processing, in human-readable form. It turns 12
    into \>=.

  - The **:fflags** modifier displays a c if the file has been marked as
    being a configuration file, a d if the file has been marked as being
    a documentation file, and blank otherwise. Thus, 2 becomes d.

</div>

</div>

<div class="sect4">

#### <span id="s4-rpm-query-queryformat-iterators">Array Iterators</span>

Until now, we've been using tags that represent single data items. There
is, for example, only one package name or installation date for each
package. However, there are other tags that can represent many different
pieces of data. One such tag is **FILENAMES**, which can be used to
display the names of every file contained in a package.

Let's put together a format string that will display the package name,
followed by the name of every file that package contains. We'll try it
on the `adduser` package first, since it contains only one file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --queryformat &#39;%{NAME}: %{FILENAMES}\n&#39; adduser
adduser: /usr/sbin/adduser

#
              </code></pre></td>
</tr>
</tbody>
</table>

Hey, not bad — got it on the first try. Now let's try it on a package
with more than one file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --queryformat &#39;%{NAME}: %{FILENAMES}\n&#39; etcskel
etcskel: (array)

#
              </code></pre></td>
</tr>
</tbody>
</table>

Hmmm. What went wrong? It worked before… Well, it worked before because
the `adduser` package contained only one file. The **FILENAMES** tag
points to an array of names, so when there is more than one file in a
package, there's a problem.

But there is a solution. It's called an *iterator*. An iterator can step
through each entry in an array, producing output as it goes. Iterators
are created when square braces enclose one or more tags and literal
text. Since we want to iterate through the **FILENAMES** array, let's
enclose that tag in the iterator:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --queryformat &#39;%{NAME}: [%{FILENAMES}]\n&#39; etcskel
etcskel: /etc/skel/etc/skel/.Xclients/etc/skel/.Xdefaults/etc/skel/.ba

#
              </code></pre></td>
</tr>
</tbody>
</table>

There was more output — it went right off the screen in one long line.
The problem? We didn't include a newline escape sequence inside the
iterator. Let's try it again:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --queryformat &#39;%{NAME}: [%{FILENAMES}\n]&#39; etcskel
etcskel: /etc/skel
/etc/skel/.Xclients
/etc/skel/.Xdefaults
/etc/skel/.bash_logout
/etc/skel/.bash_profile
/etc/skel/.bashrc
/etc/skel/.xsession

#
              </code></pre></td>
</tr>
</tbody>
</table>

That's more like it. If we wanted, we could put another file-related tag
inside the iterator. If we included the **FILESIZES** tag, we'd be able
to see the name of each file, as well as how big it was:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --queryformat &#39;%{NAME}: [%{FILENAMES} (%{FILESIZES} bytes)\n]&#39; etcskel
etcskel: /etc/skel (1024 bytes)
/etc/skel/.Xclients (551 bytes)
/etc/skel/.Xdefaults (3785 bytes)
/etc/skel/.bash_logout (24 bytes)
/etc/skel/.bash_profile (220 bytes)
/etc/skel/.bashrc (124 bytes)
/etc/skel/.xsession (9 bytes)

#
              </code></pre></td>
</tr>
</tbody>
</table>

That's pretty nice. But it would be even nicer if the package name
appeared on each line, along with the filename and size. Maybe if we put
the **NAME** tag inside the iterator:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --queryformat &#39;[%{NAME}: %{FILENAMES} \
?  (%{FILESIZES} bytes)\n]&#39; etcskel
etcskel: /etc/skel(parallel array size mismatch)#
              </code></pre></td>
</tr>
</tbody>
</table>

The error message says it all. The **FILENAMES** and **FILESIZES**
arrays are the same size. The **NAME** tag isn't even an array. Of
course the sizes don't match\!

<div class="sect5">

##### <span id="s5-rpm-query-interatoring-single-entry-tags">Iterating Single-Entry Tags</span>

If a tag only has one piece of data, it's possible to put it in an
iterator and have its one piece of data displayed with every iteration.
This is done by preceding the tag name with an equal sign. Let's try it
out on our current example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --queryformat &#39;[%{=NAME}: %{FILENAMES} (%{FILESIZES} bytes)\n]&#39; etcskel
etcskel: /etc/skel (1024 bytes)
etcskel: /etc/skel/.Xclients (551 bytes)
etcskel: /etc/skel/.Xdefaults (3785 bytes)
etcskel: /etc/skel/.bash_logout (24 bytes)
etcskel: /etc/skel/.bash_profile (220 bytes)
etcskel: /etc/skel/.bashrc (124 bytes)
etcskel: /etc/skel/.xsession (9 bytes)

#
                </code></pre></td>
</tr>
</tbody>
</table>

That's about all there is to format strings. Now, if RPM's standard
output doesn't give you what you want, you have no reason to complain.
Just **--queryformat** it\!

</div>

</div>

<div class="sect4">

#### <span id="s4-rpm-query-queryformat-listing-tags">In Case You Were Wondering…</span>

What's that? You say you don't know what tags are available? You can use
RPM's **--querytags** option. When used as the only option (ie, **rpm
--querytags**), it produces a list of available tags. It should be noted
that RPM displays the complete tag name. For instance, **RPMTAG\_ARCH**
is the complete name, yet you'll only need to use **ARCH** in your
format string. Here's a partial example of the **--querytags** option in
action:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm --querytags
RPMTAG_NAME
RPMTAG_VERSION
RPMTAG_RELEASE
…
RPMTAG_VERIFYSCRIPT

#
              </code></pre></td>
</tr>
</tbody>
</table>

Be forewarned: the full list is quite lengthy. At the time this book was
written, there were over 70 tags\! You'll notice that each tag is
printed in uppercase, and is preceded with **RPMTAG\_**. If we were to
use that last tag, **RPMTAG\_VERIFYSCRIPT**, in a format string, it
could be specified in any of the following ways:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%{RPMTAG_VERIFYSCRIPT}

%{RPMTAG_VerifyScript}

%{RPMTAG_VeRiFyScRiPt}

%{VERIFYSCRIPT}

%{VerifyScript}

%{VeRiFyScRiPt}

              </code></pre></td>
</tr>
</tbody>
</table>

The only hard-and-fast rule regarding tags is that if you include the
**RPMTAG\_** prefix, it *must* be all uppercase. The fourth example
above shows the traditional way of specifying a tag — prefix omitted,
all uppercase. The choice, however, is yours.

One other thing to keep in mind is that not every package will have
every type of tagged information available. In cases where the requested
information is not available, RPM will display (none) or (unknown).
There are also a few tags that, for one reason or another, will not
produce useful output when using in a format string. For a comprehensive
list of queryformat tags, please see [Appendix
D](ch-queryformat-tags.html).

</div>

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-query-vv-option">Getting a *lot* more information with **-vv**</span>

Sometimes it's necessary to have even *more* information than we can get
with **-v**. By adding another **v**, we can start to see more of RPM's
inner workings:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qvv rpm
D: opening database in //var/lib/rpm/
D: querying record number 2341208
rpm-2.3-1

#
          </code></pre></td>
</tr>
</tbody>
</table>

The lines starting with D: have been added by using **-vv**. We can see
where the RPM database is located and what record number contains
information on the `rpm-2.3-1` package. Following that is the usual
output.

In the vast majority of cases, it will not be necessary to use **-vv**.
It is normally used by software engineers working on RPM itself, and the
output can change without notice. However, it's a handy way to gain
insights into RPM's inner workings.

</div>

<div class="sect2">

## <span id="s2-rpm-query-root-option">**--root `<path>`**: Use **`<path>`** As An Alternate Root</span>

Adding **--root `<path>`** to a query command forces RPM to assume that
the directory specified by **`<path>`** is actually the "root"
directory. In addition, RPM expects its database to reside in the
directory specified by the **dbpath** `rpmrc` file entry, relative to
**`<path>`**. [<span class="footnote">\[6\]</span>](#FTN.AEN3606)

Normally this option is only used during an initial system install, or
when a system has been booted off a "rescue disk", and some packages
need to be re-installed in order to restore normal operation.

</div>

<div class="sect2">

## <span id="s2-rpm-query-rcfile-option">**--rcfile `<rcfile>`**: Use **`<rcfile>`** As An Alternate `rpmrc` File</span>

The **--rcfile** option is used to specify a file containing default
settings for RPM. Normally, this option is not needed. By default, RPM
uses `/etc/rpmrc` and a file named `.rpmrc`, located in your login
directory.

This option would be used if there was a need to switch between several
sets of RPM options. Software developer and package builders will be the
people using **--rcfile**. For more information on `rpmrc` files, see
[Appendix B](ch-rpmrc-file.html).

</div>

<div class="sect2">

## <span id="s2-rpm-query-dbpath-option">**--dbpath `<path>`**: Use **`<path>`** To Find RPM Database</span>

In order for RPM to do its handiwork, it needs access to an RPM
database. Normally, this database exists in the directory specified by
the `rpmrc` file entry, **dbpath**. By default, **dbpath** is set to
`/var/lib/rpm`.

Although the **dbpath** entry can be modified in the appropriate `rpmrc`
file, the **--dbpath** option is probably a better choice when the
database path needs to be changed temporarily. An example of a time the
**--dbpath** option would come in handy is when it's necessary to
examine an RPM database copied from another system. Granted, it's not a
common occurrence, but it's difficult to handle any other way.

</div>

</div>

### Notes

|                                                                        |                                                                                                                                                                                                               |
| ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-query-parts.html#AEN2793) | On most Linux systems, the **file** command can be used to obtain similar information. See [Appendix A](ch-rpm-file-format.html) for details on how to add this capability to your system's **file** command. |
| [<span class="footnote">\[2\]</span>](s1-rpm-query-parts.html#AEN2938) | Note to software packagers: Choose your build machine names wisely\! A silly or offensive name might be embarrassing…                                                                                         |
| [<span class="footnote">\[3\]</span>](s1-rpm-query-parts.html#AEN3090) | For more information on `rpmrc` file entries, please refer to [Appendix B](ch-rpmrc-file.html).                                                                                                               |
| [<span class="footnote">\[4\]</span>](s1-rpm-query-parts.html#AEN3279) | For more information on package verification, please see [the Section called ***rpm -V** — What Does it Do?* in Chapter 6](ch-rpm-verify.html#s1-rpm-verify-what-it-does).                                    |
| [<span class="footnote">\[5\]</span>](s1-rpm-query-parts.html#AEN3375) | RPM uses `printf` to do **--queryformat** formatting. Therefore, you can use any of the `printf` format modifiers discussed in the `printf(3)` man page.                                                      |
| [<span class="footnote">\[6\]</span>](s1-rpm-query-parts.html#AEN3606) | For more information on `rpmrc` file entries, see [Appendix B](ch-rpmrc-file.html).                                                                                                                           |

<div class="NAVFOOTER">

-----

|                                    |                         |                                         |
| :--------------------------------- | :---------------------: | --------------------------------------: |
| [Prev](ch-rpm-query.html)          |   [Home](index.html)    | [Next](s1-rpm-query-handy-queries.html) |
| Getting Information About Packages | [Up](ch-rpm-query.html) |                     A Few Handy Queries |

</div>
