<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-reloc-building-relocatable.html)

[Next](s1-rpm-anywhere-different-build-area.html)

-----

<div class="chapter">

# <span id="ch-rpm-anywhere"></span>Chapter 16. Making a Package That Can Build Anywhere

While RPM makes building packages as easy as possible, some of the
default design decisions might not work well in a particular situation.
Here are two situations where RPM's method of package building may cause
problems:

1.  You are unable to dedicate a system to RPM package building, or the
    software you're packaging would disrupt the build system's operation
    if it were installed.

2.  You would like to package software, but you don't have root access
    to an appropriate build system.

Either of these situations can be resolved by directing RPM to build,
install, and package the software in a different area on your build
system. It requires a bit of additional effort to accomplish this, but
taken a step at a time, it is not difficult. Basically, the process can
be summed up by addressing the following steps:

  - Writing the package's spec file to support a build root.

  - Directing RPM to build software in a user-specified build area.

  - Specifying file attributes that RPM needs to set on installation.

The methods discussed here are not required in every situation. For
example, a system administrator developing a package on a production
system may only need to add support for a build root. On the other hand,
a student wishing to build a package on a university system will need to
get around the lack of root access by implementing every method
described here.

<div class="sect1">

# <span id="s1-rpm-anywhere-using-build-roots">Using Build Roots in a Package</span>

Part of the process of packaging software with RPM is to actually build
the software and install it on the build system. The installation of
software can only be accomplished by someone with root access, so a
non-privileged user will certainly need to handle RPM's installation
phase differently. There are times, however, when even a person with
root access will not want RPM to copy new files into the system's
directories. As mentioned above, the reasons might be due to the fact
that the software being packaged is already in use on the build system.
Another reason might be as mundane as not having enough free space
available to perform the install into the default directories.

Whatever the reason, RPM provides the ability to direct a given package
to install into an alternate root. This alternate root is known as a
*build root*. Several requirements must be met in order for a build root
to be utilized:

  - A default build root must be defined in the package's spec file.

  - The installation method used by the software being packaged must be
    able to support installation in an alternate root.

The first part is easy. It entails adding the following line to the spec
file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>BuildRoot: &lt;root&gt;

        </code></pre></td>
</tr>
</tbody>
</table>

Of course, you would replace `<root>` with the name of the directory in
which you'd like the software to install.
[<span class="footnote">\[1\]</span>](#FTN.AEN10138) If, for example,
you specify a build root of `/tmp/foo`, and the software you're
packaging installs a file `bar` in `/usr/bin`, you'll find `bar`
residing in `/tmp/foo/usr/bin` after the build.

A note for you non-root package builders: make sure you can actually
write to the build root you specify\! Those of you with root access
should also make sure you choose your build root carefully. For an
assortment of reasons, it's *not* a good idea to declare a build root of
"`/`"\! We'll get into the reasons why shortly.

The final requirement for adding build root support is to make sure the
software's installation method can support installing into an alternate
root. The difficulty in meeting this requirement can range from dead
simple to nearly impossible. There are probably as many different ways
of approaching this as there are packages to build. But in general, some
variant of the following approach is used:

  - The environment variable `RPM_BUILD_ROOT` is set by RPM and contains
    the value of the build root to be used when the software is built
    and installed.

  - The **%install** section of the spec file is modified to use
    `RPM_BUILD_ROOT` as part of the installation process.

  - If the software is installed using **make**, the makefile is
    modified to use `RPM_BUILD_ROOT` and to create any directories that
    may not exist at installation time.

Here's an example of how these components work together to utilize a
build root. First, there's the definition of the build root in the spec
file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>BuildRoot: /tmp/cdplayer

        </code></pre></td>
</tr>
</tbody>
</table>

This line defines the build root as being `/tmp/cdplayer`. All the files
installed by this software will be placed under the `cdplayer`
directory. Next is the spec file's **%install** section:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%install
make ROOT=&quot;$RPM_BUILD_ROOT&quot; install

        </code></pre></td>
</tr>
</tbody>
</table>

Since the software we're packaging uses **make** to perform the actual
install, we simply define the environment variable `ROOT` to be the path
defined by `RPM_BUILD_ROOT`. So far, so good. Things really start to get
interesting in the software's `Makefile`, though:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>install: cdp cdp.1.Z
#       chmod 755 cdp
#       cp cdp /usr/local/bin
        install -m 755 -o 0 -g 0 -d $(ROOT)/usr/local/bin/
        install -m 755 -o 0 -g 0 cdp $(ROOT)/usr/local/bin/cdp
#       ln -s /usr/local/bin/cdp /usr/local/bin/cdplay
        ln -s ./cdp $(ROOT)/usr/local/bin/cdplay
#       cp cdp.1 /usr/local/man/man1
        install -m 755 -o 0 -g 0 -d $(ROOT)/usr/local/man/man1/
        install -m 755 -o 0 -g 0 cdp.1 $(ROOT)/usr/local/man/man1/cdp.1

        </code></pre></td>
</tr>
</tbody>
</table>

In the example above, the commented lines were the original ones. The
uncommented lines perform the same function, but also support
installation in the root specified by the environment variable `ROOT`.

One point worth noting is that the `Makefile` now takes extra pains to
make sure the proper directory structure exists before installing any
files. This is often necessary, as build roots are deleted, in most
cases, after the software has been packaged. This is why **install** is
used with the **-d** option — to make sure the necessary directories
have been created.

Let's see how it works:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba cdplayer-1.0.spec
* Package: cdplayer
Executing: %prep
+ cd /usr/src/redhat/BUILD
…
+ exit 0
Executing: %build
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
…
+ exit 0
+ umask 022
Executing: %install
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
+ make ROOT=/tmp/cdplayer install
install -m 755 -o 0 -g 0 -d /tmp/cdplayer/usr/local/bin/
install -m 755 -o 0 -g 0 cdp /tmp/cdplayer/usr/local/bin/cdp
ln -s ./cdp /tmp/cdplayer/usr/local/bin/cdplay
install -m 755 -o 0 -g 0 -d /tmp/cdplayer/usr/local/man/man1/
install -m 755 -o 0 -g 0 cdp.1 /tmp/cdplayer/usr/local/man/man1/cdp.1
+ exit 0
Executing: special doc
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
+ DOCDIR=/tmp/cdplayer//usr/doc/cdplayer-1.0-1
+ rm -rf /tmp/cdplayer//usr/doc/cdplayer-1.0-1
+ mkdir -p /tmp/cdplayer//usr/doc/cdplayer-1.0-1
+ cp -ar README /tmp/cdplayer//usr/doc/cdplayer-1.0-1
+ exit 0
Binary Packaging: cdplayer-1.0-1
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
+ umask 022
+ echo Executing: %clean
Executing: %clean
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
+ exit 0
Source Packaging: cdplayer-1.0-1
cdplayer-1.0.spec
cdplayer-1.0.tgz
82 blocks
Generating signature: 0
Wrote: /usr/src/redhat/SRPMS/cdplayer-1.0-1.src.rpm

# 
        </code></pre></td>
</tr>
</tbody>
</table>

Looking over the output from the **%install** section, we first see that
the `RPM_BUILD_ROOT` environment variable in the **make install**
command, has been replaced with the path specified earlier in the spec
file on the **BuildRoot:** line. The `ROOT` environment variable used in
the makefile now has the appropriate value, as can be seen in the
various **install** commands that follow.

Note, also, that we use **install**'s **-d** option to ensure that every
directory in the path exists before we actually install the software.
Unfortunately, we can't do this and install the file in one command.

Looking at the section labeled Executing: special doc, we find that RPM
is doing something similar for us. It starts by making sure there is no
pre-existing documentation directory. Next, RPM creates the
documentation directory and copies files into it.

The remainder of this example is identical to that of a package being
built without a build root being specified. However, although the output
is identical, there is one crucial difference. When the binary package
is created, instead of simply using each line in the **%files** list
verbatim, RPM prepends the build root path first. If this wasn't done,
RPM would attempt to find the files, relative to the system's root
directory, and would, of course, fail. Because of the automatic
prepending of the build root, it's important to *not* include the build
root path in any **%files** list entry. Otherwise, the files would not
be found by RPM, and the build would fail.

Although RPM has to go through a bit of extra effort to locate the files
to be packaged, the resulting binary package is indistinguishable from
the same package created without using a build root.

<div class="sect2">

## <span id="s2-rpm-anywhere-things-to-consider">Some Things to Consider</span>

Once the necessary modifications have been made to support a build root,
it's necessary for the package builder to keep some issues in mind. The
first is that the build root specified in the spec file can be
overridden. RPM will set the build root (and therefore, the value of
`$RPM_BUILD_ROOT`) to one of the following values:

  - The value of **buildroot** in the spec file.

  - The value of **buildroot** in an `rpmmacros` file.

  - The value following the **--buildroot** option on the command line.

Because of this, it's important that the spec file and the makefile be
written in such a way that no assumptions about the build root are made.
The main issue is that the build root must not be hard-coded anywhere.
Always use the `RPM_BUILD_ROOT` environment variable\!

Another issue to keep in mind is cleaning up after the build. Once
software builds and is packaged successfully, it's probably no longer
necessary to leave the build root in place. Therefore, it's a good idea
to include the necessary commands in the spec file's **%clean** section.
Here's an example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%clean
rm -rf $RPM_BUILD_ROOT

          </code></pre></td>
</tr>
</tbody>
</table>

Since RPM executes the **%clean** section after the binary package has
been created, it's the perfect place to delete the build root tree. In
the example above, that's exactly what we're doing. We're also doing the
right thing by using the `RPM_BUILD_ROOT`, instead of a hard-coded path.

The last issue to keep in mind revolves around the **%clean** section we
just created. At the start of the chapter, we mentioned that it's not a
good idea to define a build root of "`/`". The **%clean** section is
why: If the build root was set to "`/`", the **%clean** section would
blow away your root filesystem\! Keep in mind that this can bite you,
even if the package's spec file doesn't specify "`/`" as a build root.
It's possible to use the **--buildroot** option to specify a dangerous
build root, too:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba --buildroot / cdplayer-1.0.spec
          </code></pre></td>
</tr>
</tbody>
</table>

But for all the possible hazards using build roots can pose for the
careless, it's the only way to prevent a build from disrupting the
operation of certain packages on the build system. And for the person
wanting to build packages without root access, it's the first of three
steps necessary to accomplish the task. The next step is to direct RPM
to build the software in a directory other than RPM's default one.

</div>

</div>

</div>

### Notes

|                                                                      |                                                                                                                                                                                                          |
| -------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](ch-rpm-anywhere.html#AEN10138) | Keep in mind that the build root can be overridden at build-time using the **--buildroot** option or the **buildroot** `rpmmacros` file entry. See [Chapter 12](ch-rpm-b-command.html) for more details. |

<div class="NAVFOOTER">

-----

|                                                |                    |                                                   |
| :--------------------------------------------- | :----------------: | ------------------------------------------------: |
| [Prev](s1-rpm-reloc-building-relocatable.html) | [Home](index.html) | [Next](s1-rpm-anywhere-different-build-area.html) |
| Building a Relocatable Package                 |  [Up](p5206.html)  |             Having RPM Use a Different Build Area |

</div>
