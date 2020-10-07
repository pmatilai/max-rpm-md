<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpmrc-file.md)

Appendix B. The `rpmrc` File

[Next](s1-rpmrc-file-rpmrc-file-syntax.md)

-----

<div class="sect1">

# <span id="s1-rpmrc-file-rpmrc-file-locations">Different Places an `rpmrc` File Resides</span>

RPM looks for `rpmrc` files in four places:

1.  In `/usr/lib/`, for a file called `rpmrc`.

2.  In `/etc/`, for a file called `rpmrc`.

3.  In a file called `.rpmrc` in the user's login directory.

4.  In a file specified by the **--rcfile** option, if the option is
    present on the command line.

The first three files are read in the order listed, such that if a given
`rpmrc` entry is present in each file, the value of the entry read
*last* is the one used by RPM. This means, for example, that an entry in
`.rpmrc` in the user's login directory will always override the same
entry in `/etc/rpmrc`. Likewise, an entry in `/etc/rpmrc` will always
override the same entry in `/usr/lib/rpmrc`.

If the **--rcfile** option is used, then only `/usr/lib/rpmrc` and the
file following the **--rcfile** option are read, in that order. The
`/usr/lib/rpmrc` file is *always* read first. This cannot be changed.

Let's look at each of these files, starting with `/usr/lib/rpmrc`.

<div class="sect2">

## <span id="s2-rpmrc-file-usr-lib-rpmrc">`/usr/lib/rpmrc`</span>

The file `/usr/lib/rpmrc` is *always* read. It contains information that
RPM uses to set some default values. *This file should never be
modified\!* Doing so may cause RPM to operate incorrectly.

After this stern warning, we should note that it's perfectly all right
to look at it. Here it is, in fact:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#############################################################
# Default values, often overridden in /etc/rpmrc

dbpath:     /var/lib/rpm
topdir:     /usr/src/redhat
tmppath:    /var/tmp
cpiobin:    cpio
defaultdocdir:  /usr/doc

#############################################################

# Please send new entries to rpm-list@redhat.com

#############################################################
# Values for RPM_OPT_FLAGS for various platforms

optflags: i386 -O2 -m486 -fno-strength-reduce
optflags: alpha -O2
optflags: sparc -O2
optflags: m68k -O2 -fomit-frame-pointer

#############################################################
# Canonical arch names and numbers

arch_canon: i986:   i986    1
arch_canon: i886:   i886    1
arch_canon: i786:   i786    1
arch_canon: i686:   i686    1
arch_canon: i586:   i586    1
arch_canon: i486:   i486    1
arch_canon: i386:   i386    1
arch_canon: alpha:  alpha   2
arch_canon:     sparc:  sparc   3
arch_canon:     sun4:   sparc   3
arch_canon:     sun4m:  sparc   3
arch_canon:     sun4c:  sparc   3
# This is really a place holder for MIPS.
arch_canon: mips:   mips    4
arch_canon: ppc:    ppc 5
# This is really a place holder for 68000
arch_canon: m68k:   m68k    6
# This is wrong. We really need globbing in here :-(
arch_canon: IP: sgi 7
arch_canon:     IP22:   sgi     7

arch_canon:    9000/712:       hppa1.1 9

arch_canon:    sun4u:   usparc  10

#############################################################
# Canonical OS names and numbers

os_canon:   Linux:  Linux   1
os_canon:   IRIX:   Irix    2
# This is wrong
os_canon:   SunOS5: solaris 3
os_canon:   SunOS4: SunOS   4

os_canon:      AmigaOS: AmigaOS 5
os_canon:          AIX: AIX     5
os_canon:        HP-UX: hpux10  6
os_canon:         OSF1: osf1    7
os_canon:      FreeBSD: FreeBSD 8

#############################################################
# For a given uname().machine, the default build arch

buildarchtranslate: osfmach3_i986: i386
buildarchtranslate: osfmach3_i886: i386
buildarchtranslate: osfmach3_i786: i386
buildarchtranslate: osfmach3_i686: i386
buildarchtranslate: osfmach3_i586: i386
buildarchtranslate: osfmach3_i486: i386
buildarchtranslate: osfmach3_i386: i386

buildarchtranslate: i986: i386
buildarchtranslate: i886: i386
buildarchtranslate: i786: i386
buildarchtranslate: i686: i386
buildarchtranslate: i586: i386
buildarchtranslate: i486: i386
buildarchtranslate: i386: i386

buildarchtranslate: osfmach3_ppc: ppc

#############################################################
# Architecture compatibility

arch_compat: alpha: axp

arch_compat: i986: i886
arch_compat: i886: i786
arch_compat: i786: i686
arch_compat: i686: i586
arch_compat: i586: i486
arch_compat: i486: i386

arch_compat: osfmach3_i986: i986 osfmach3_i886
arch_compat: osfmach3_i886: i886 osfmach3_i786
arch_compat: osfmach3_i786: i786 osfmach3_i686
arch_compat: osfmach3_i686: i686 osfmach3_i586
arch_compat: osfmach3_i586: i586 osfmach3_i486
arch_compat: osfmach3_i486: i486 osfmach3_i386
arch_compat: osfmach3_i386: i486

arch_compat: osfmach3_ppc: ppc

arch_compat: usparc: sparc

        </code></pre></td>
</tr>
</tbody>
</table>

Quite a bunch of stuff, isn't it? With the exception of the first five
lines, which indicate where several important directories and programs
are located, the remainder of this file contains `rpmrc` entries that
are related to RPM's architecture and operating system processing. As
you might imagine, any tinkering here will probably not be very
productive, so leave any modifications here to the RPM developers.

Next, we have `/etc/rpmrc`.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-etc-rpmrc">`/etc/rpmrc`</span>

The file `/etc/rpmrc`, unlike `/usr/lib/rpmrc`, is fair game for
modifications and additions. In fact, `/etc/rpmrc` isn't created by
default, so its contents are entirely up to you. It's the perfect place
to keep `rpmrc` entries of a system-wide or global nature.

The **vendor** entry is a great example of a good candidate for
inclusion in `/etc/rpmrc`. In most cases, a particular system is
dedicated to building packages for one vendor. In these instances,
setting the **vendor** entry in `/etc/rpmrc` is best.

Next in the hierarchy is a file named `.rpmrc`, residing in the user's
login directory.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-rpmrc-user-login">`.rpmrc` in the user's login directory</span>

As you might imagine, a file called `.rpmrc` in a user's login directory
is only going to be read by that user when he or she runs RPM. Like
`/etc/rpmrc`, this file is not created by default, but it can contain
the same `rpmrc` entries as the other files. The **packager** entry,
which should contain the name and contact information for the person who
built the package, is an appropriate candidate for `~/.rpmrc`.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-rcfile-option">File indicated by the **--rcfile** option</span>

The **--rcfile** option is best used only when a totally different RPM
configuration is desired for one or two packages. Since the only other
`rpmrc` file read is `/usr/lib/rpmrc` with its low-level default
settings, the file specified with the **--rcfile** option will have to
be more comprehensive than either `/etc/rpmrc` or `~/.rmprc`.

</div>

</div>

<div class="NAVFOOTER">

-----

|                            |                          |                                              |
| :------------------------- | :----------------------: | -------------------------------------------: |
| [Prev](ch-rpmrc-file.md) |    [Home](index.md)    | [Next](s1-rpmrc-file-rpmrc-file-syntax.md) |
| The `rpmrc` File           | [Up](ch-rpmrc-file.md) |                          `rpmrc` File Syntax |

</div>
