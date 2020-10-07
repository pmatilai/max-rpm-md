<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-build-getting-sources.md)

Chapter 11. Building Packages: A Simple Example

[Next](s1-rpm-build-starting-build.md)

-----

<div class="sect1">

# <span id="s1-rpm-build-creating-spec-file">Creating the Spec File</span>

The way we direct RPM in the build process is to create a spec file. As
we saw in the previous chapter, the spec file contains eight different
sections, most of which are required. Let's go through each section and
create `cdplayer`'s spec file as we go.

<div class="sect2">

## <span id="s2-rpm-build-spec-file-preamble">The Preamble</span>

The preamble contains a wealth of information about the package being
built, and the people that built it. Here's `cdplayer`'s preamble:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#
# Example spec file for cdplayer app...
#
Summary: A CD player app that rocks!
Name: cdplayer
Version: 1.0
Release: 1
License: GPL
Group: Applications/Sound
Source: ftp://ftp.gnomovision.com/pub/cdplayer/cdplayer-1.0.tgz
URL: http://www.gnomovision.com/cdplayer/cdplayer.md
Distribution: WSS Linux
Vendor: White Socks Software, Inc.
Packager: Santa Claus &lt;sclaus@northpole.com&gt;

%description
It slices!  It dices!  It&#39;s a CD player app that
can&#39;t be beat.  By using the resonant frequency
of the CD itself, it is able to simulate 20X
oversampling.  This leads to sound quality that
cannot be equaled with more mundane software...

          </code></pre></td>
</tr>
</tbody>
</table>

In general, the preamble consists of entries, one per line, that start
with a *tag* followed by a colon, and then some information. For
example, the line starting with "**Summary:**" gives a short description
of the packaged software that can be displayed by RPM. The order of the
lines is not important, as long as they appear in the preamble.

Let's take a look at each line and see what function it performs:

<div class="sect3">

### <span id="s3-rpm-build-preamble-name">Name</span>

The **name** line defines what the package will actually be called. In
general, it's a good idea to use the name of the software. The name will
also be included in the package label, and the package filename.

</div>

<div class="sect3">

### <span id="s3-rpm-build-preamble-version">Version</span>

The **version** line should be set to the version of the software being
packaged. The version will also be included in the package label, and
the package filename.

</div>

<div class="sect3">

### <span id="s3-rpm-build-preamble-release">Release</span>

The **release** is a number that is used to represent the number of
times the software, at the present version, has been packaged. You can
think of it as the package's version number. The release is also part of
the package label and package filename.

</div>

<div class="sect3">

### <span id="s3-rpm-build-preamble-license">License</span>

The **license** line is used to hold the packaged software's license
information. This makes it easy to determine which packages can be
freely redistributed, and which cannot. In our case, `cdplayer` is made
available under the terms of the GNU General Public License, so we've
put GPL on the line.

</div>

<div class="sect3">

### <span id="s3-rpm-build-preamble-group">Group</span>

The **group** line is used to hold a string that defines how the
packaged software should be grouped with other packages. The string
consists of a series of words separated by slashes. From left to right,
the words describe the packaged software more explicitly. We grouped
`cdplayer` under `Applications`, because it is an application, and then
under `Sound`, since it is an application that is sound-related.

</div>

<div class="sect3">

### <span id="s3-rpm-build-preamble-source">Source</span>

The **source** line serves two purposes:

  - To document where the packaged software's sources can be found.

  - To give the name of the source file as it exists in the `SOURCES`
    subdirectory.

In our example, the `cdplayer` sources are contained in the file
`cdplayer-1.0.tgz`, which is available from `ftp.gnomovision.com`, in
the directory `/pub/cdplayer`. RPM actually ignores everything prior to
the last filename in the source line, so the first part of the source
string could be anything you'd like. Traditionally, the **source** line
usually contains a Uniform Resource Locator, or URL.

</div>

<div class="sect3">

### <span id="s3-rpm-build-preamble-url">URL</span>

The **URL** line is used to contain a URL, like the **source** line. How
are they different? While the **source** line is used to provide the
source filename to RPM, the **URL** line points to *documentation* for
the software being packaged.

</div>

<div class="sect3">

### <span id="s3-rpm-build-preamble-distribution">Distribution</span>

The **distribution** line contains the name of the product which the
packaged software is a part of. In the Linux world, the operating system
is often packaged together into a "distribution", hence the name. Since
we're using a fictional application in this example, we've filled in the
distribution line with the name of a fictional distribution. There's no
requirement that the spec file contain a **distribution** line, so
individuals will probably omit this.

</div>

<div class="sect3">

### <span id="s3-rpm-build-preamble-vendor">Vendor</span>

The **vendor** line identifies the organization that distributes the
software. Maintaining our fictional motif, we've invented fictional
company, White Socks Software, to add to our spec file. Individuals will
probably omit this as well.

</div>

<div class="sect3">

### <span id="s3-rpm-build-preamble-packager">Packager</span>

The **packager** line is used to identify the organization that actually
*packaged* the software, as opposed to the author of the software. In
our example, we've chosen the greatest packager of them all, Santa
Claus, to work at White Socks Software. Note that we've included contact
information, in the form of an e-mail address.

</div>

<div class="sect3">

### <span id="s3-rpm-build-preamble-description">Description</span>

The **description** line is a bit different, in that it starts with a
percent sign. It is also different because the information can take up
more than one line. It is used to provide a more detailed description of
the packaged software than the **summary** line.

</div>

<div class="sect3">

### <span id="s3-rpm-build-preamble-comments">A Comment on Comments</span>

At the top of the spec file, there are three lines, each starting with a
pound sign. These are comments and can be sprinkled throughout the spec
file to make it more easily understood.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-build-prep-section">The **%prep** Section</span>

With the preamble, we provided a wealth of information. The majority of
this information is meant for human consumption. Only the **name**,
**version**, **release**, and **source** lines have a direct bearing on
the package building process. However, in the **%prep** section, the
focus is entirely on directing RPM through the process of preparing the
software for building.

It is in the **%prep** section that the build environment for the
software is created, starting with removing the remnants of any previous
builds. Following this, the source archive is expanded. Here is what the
**%prep** section looks like in our example spec file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%prep
rm -rf $RPM_BUILD_DIR/cdplayer-1.0
zcat $RPM_SOURCE_DIR/cdplayer-1.0.tgz | tar -xvf -

          </code></pre></td>
</tr>
</tbody>
</table>

If the **%prep** section looks like a script, that's because it is. Any
**sh** constructs can be used here, including expansion of environment
variables (Like the `$RPM_BUILD_DIR` variable defined by RPM), and
piping the output of **zcat** through **tar**.
[<span class="footnote">\[1\]</span>](#FTN.AEN5679)

In this case, we perform a recursive delete in the build directory to
remove any old builds. We then uncompress the gzipped **tar** file, and
extract its contents into the build directory.

Quite often, the sources may require patching in order to build
properly. The **%prep** section is the appropriate place to patch the
sources, but in this example, no patching is required. Fear not,
however, as we'll explore patching in all its glory in [Chapter
20](ch-rpm-rw-build.md), when we build a more complex package.

<div class="sect3">

### <span id="s3-rpm-build-macros">Making Life Easier With Macros</span>

While the **%prep** section as we've described it isn't *that* difficult
to understand, RPM provides macros to make life even easier. In this
simple example, there's precious little that can be made easier, but
macros will prevent a wealth of headaches when it's time to build more
complex packages. The macro we'll introduce here is the **%setup**
macro.

The average gzipped **tar** file is **%setup**'s stock in trade. Like
the hand-crafted **%prep** section we described above, it cleans up old
build trees and then uncompresses and extracts the files from the
original source. While **%setup** has a number of options that we'll
cover in later chapters, for now all we need for a **%prep** section is:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%prep
%setup

            </code></pre></td>
</tr>
</tbody>
</table>

That is simpler than our **%prep** section, so let's use the **%setup**
macro instead. The **%setup** macro has a number of options to handle
many different situations. For more information on this and other
macros, please see [the Section called *Macros: Helpful Shorthand for
Package Builders* in Chapter 13](s1-rpm-inside-macros.md).

In our example here, the **%prep** section is complete. Next comes the
actual build.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-build-build-section">The **%build** Section</span>

Not surprisingly, the part of the spec file that is responsible for
performing the build, is the **%build** section. Like the **%prep**
section, the **%build** section is an ordinary **sh** script. Unlike the
**%prep** section, there are no macros. The reason for this is that the
process of building software is either going to be very easy, or highly
complicated. In either case, macros won't help much. In our example, the
build process is simple:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%build
make 

          </code></pre></td>
</tr>
</tbody>
</table>

Thanks to the **make** utility, only one command is necessary to build
the `cdplayer` application. In the case of an application with more
esoteric build requirements, the **%build** section could get a bit more
interesting.

</div>

<div class="sect2">

## <span id="s2-rpm-build-install-section">The **%install** Section</span>

The **%install** section is executed as a **sh** script, just like
**%prep** and **%build**. If the application is built with **make** and
has an "install" target, the **%install** section will also be
straightforward. The `cdplayer` application is a good example of this:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%install
make install

          </code></pre></td>
</tr>
</tbody>
</table>

If the application doesn't have a means of automatically installing
itself, it will be necessary to create a script to do so, and place it
in the **%install** section.

</div>

<div class="sect2">

## <span id="s2-rpm-build-files-section">The **%files** Section</span>

The **%files** section is different from the others, in that it contains
a list of the files that are part of the package. Always remember — if
it isn't in the file list, it won't be put in the package\!

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%files
%doc README
/usr/local/bin/cdp
/usr/local/bin/cdplay
/usr/local/man/man1/cdp.1

          </code></pre></td>
</tr>
</tbody>
</table>

The line starting with **%doc** is an example of RPM's handling of
different file types. As you might guess, **%doc** stands for
documentation. The **%doc** directive is used to mark files as being
documentation. In the example above, the `README` file will be placed in
a package-specific directory, located in `/usr/doc`, and called
`cdplayer-1.0-1`. It's also possible to mark files as documentation and
have them installed in other directories. This is covered in more detail
in [the Section called *The **%doc** Directive* in Chapter
13](s1-rpm-inside-files-list-directives.md#s3-rpm-inside-flist-doc-directive).

The rest of the files in the example are shown with complete paths. This
is necessary as the files will actually be installed in those
directories by the application's makefile. Since RPM needs to be able to
find the files prior to packaging them, complete paths are required.

<div class="sect3">

### <span id="s3-rpm-build-creating-files-list">How Do You Create the File List?</span>

Since RPM automates so many aspects of software installation, it's easy
to fall into the trap of assuming that RPM does *everything* for you.
Not so\! One task that is still a manual process is creating the file
list. While it may seem at first glance, that it could be automated
somehow, it's actually a more difficult problem than it seems.

Since the majority of an application's files are installed by its
makefile, RPM has no control over that part of the build process, and
therefore, cannot automatically determine which files should be part of
the package. Some people have attempted to use a modified version of
**install** that logs the name of every file it installs. But not every
makefile uses **install**, or if it does, uses it sporadically.

Another approach tried was to obtain a list of every file on the build
system, immediately before and after a build, and use the differences as
the file list. While this approach will certainly find every file that
the application installed, it can also pick up extraneous files, such as
system logs, files in `/tmp`, and the like. The only way to begin to
make this approach workable would be to do nothing else on the build
system, which is highly inconvenient. This approach also precludes
building more than one package on the system at any given time.

At present, the best way to create the file list is to read the makefile
to see what files it installs, verify this against the files installed
on the build system, and create the list.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-build-missing-sections">The Missing Spec File Sections</span>

Since our example spec file is somewhat simplistic, it's missing two
sections that might be used in more complex situations. We'll go over
each one briefly here. More complete information on these sections will
be covered at various points in the book.

<div class="sect3">

### <span id="s3-rpm-build-install-uninstall-scripts">The Install/Uninstall Scripts</span>

One missing section to our spec file is the section that would define
one or more of four possible scripts. The scripts are executed at
various times when a package is installed or erased.

The scripts can be executed:

  - Before a package is installed.

  - After a package is installed.

  - Before a package is erased.

  - After a package is erased.

We'll see how these scripts are used in [Chapter
20](ch-rpm-rw-build.md).

</div>

<div class="sect3">

### <span id="s3-rpm-build-clean-section">The **%clean** Section</span>

The other missing section has the rather descriptive title of
**%clean**. This section can be used to clean up any files that are not
part of the application's normal build area. For example, if the
application creates a directory structure in `/tmp` as part of its
build, it will not be removed. By adding a **sh** script to the
**%clean** section, such situations can be handled gracefully, right
after the binary package is created.

</div>

</div>

</div>

### Notes

|                                                                                     |                                                                                                                                                                                                                         |
| ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-build-creating-spec-file.md#AEN5679) | For more information on the environment variables used in the build-time scripts, please refer to [the Section called *Build-time Scripts* in Chapter 13](s1-rpm-inside-scripts.md#s2-rpm-inside-build-time-scripts). |

<div class="NAVFOOTER">

-----

|                                           |                         |                                          |
| :---------------------------------------- | :---------------------: | ---------------------------------------: |
| [Prev](s1-rpm-build-getting-sources.md) |   [Home](index.md)    | [Next](s1-rpm-build-starting-build.md) |
| Getting the Sources                       | [Up](ch-rpm-build.md) |                       Starting the Build |

</div>
