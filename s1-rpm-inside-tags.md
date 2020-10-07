<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-inside.md)

Chapter 13. Inside the Spec File

[Next](s1-rpm-inside-scripts.md)

-----

<div class="sect1">

# <span id="s1-rpm-inside-tags">Tags: Data Definitions</span>

Looking at a spec file, the first thing you'll see are a number of
lines, all following the same basic format:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>&lt;something&gt;:&lt;something-else&gt;

        </code></pre></td>
</tr>
</tbody>
</table>

The `<something>` is known as a "tag", because it is used by RPM to name
or *tag* some data. The tag is separated from its associated data by a
colon. The data is represented by the `<something-else>` above. Tags are
grouped together at the top of the spec file, in a section known as the
preamble. Here's an example of a tag and its data:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Vendor: White Socks Software, Inc.

        </code></pre></td>
</tr>
</tbody>
</table>

In this example, the tag is "**Vendor**". Tags are not case-sensitive —
they may be all uppercase, all lowercase, or anything in-between. The
**Vendor** tag is used to define the name of the organization producing
the package. The data in this example is "**White Socks Software,
Inc.**". Therefore, RPM will store **White Socks Software, Inc.** as the
vendor of the package.

Note, also, that spacing between the tag, the colon, and the data is
unimportant. Given this, and the case-insensitivity of the tag, each of
the following lines are equivalent to the one above:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>VeNdOr : White Socks Software, Inc.
vendor:White Socks Software, Inc.
VENDOR    :    White Socks Software, Inc.

        </code></pre></td>
</tr>
</tbody>
</table>

The bottom line is that you can make tag lines as neat or as ugly as you
like — RPM won't mind either way. Note, however, the tag's data may need
to be formatted in a particular fashion. If there are any such
restrictions, we'll mention them. Below, we've grouped tags of similar
functions together for easier reference, starting with the tags that are
used to create the package name.

<div class="sect2">

## <span id="s2-rpm-inside-package-naming-tags">Package Naming Tags</span>

The following tags are used by RPM to produce the package's final name.
Since the name is always in the format:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>&lt;name&gt;-&lt;version&gt;-&lt;release&gt;

          </code></pre></td>
</tr>
</tbody>
</table>

it's only natural that the three tags are known as **name**,
**version**, and **release**.

<div class="sect3">

### <span id="s3-rpm-inside-name-tag">The **name** Tag</span>

The **name** tag is used to define the name of the software being
packaged. In most (if not all) cases, the name used for a package should
be identical in spelling and case to the software being packaged. The
name cannot contain any whitespace: If it does, RPM will only use the
first part of the name (up to the first space). Therefore, if the name
of the software being packaged is `cdplayer`, the **name** tag should be
something like:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Name: cdplayer

            </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect3">

### <span id="s3-rpm-inside-version-tag">The **version** Tag</span>

The **version** tag defines the version of the software being packaged.
The version specified should be as close as possible to the format of
the original software's version. In most cases, there should be no
problem specifying the version just as the software's original developer
did. However, there is a restriction. There can be no dashes in the
version. If you forget, RPM will remind you:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba cdplayer-1.0.spec
* Package: cdplayer
Illegal &#39;-&#39; char in version: 1.0-a

#
            </code></pre></td>
</tr>
</tbody>
</table>

Spaces in the version will also cause problems, in that anything after
the first space will be ignored by RPM. Bottom line: Stick with
alphanumeric characters and periods, and you'll never have to worry
about it. Here's a sample **version** tag:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Version: 1.2

            </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect3">

### <span id="s3-rpm-inside-release-tag">The **release** Tag</span>

The **release** tag can be thought of as the *package's* version. The
release is traditionally an integer — for example, when a specific piece
of software at a particular version is first packaged, the release
should be "**1**". If it is necessary to repackage that software at the
same version, the release should be incremented. When a new version of
the software becomes available, the release should drop back to "**1**"
when it is first packaged.

Note that we used the word "traditionally", above. The only hard and
fast restriction to the release format is that there can be no dashes in
it. Be aware that if you buck tradition, your users may not understand
what your release means.

It is up to the package builder to determine which build represents a
new release and to update the release manually. Here is what a typical
**release** tag might look like:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Release: 5

            </code></pre></td>
</tr>
</tbody>
</table>

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-inside-descriptive-tags">Descriptive Tags</span>

These tags provide information primarily for people who want to know a
bit more about the package, and who produced it. They are part of the
package file, and most of them can be seen by issuing an **rpm -qi**
command.

<div class="sect3">

### <span id="s3-rpm-inside-description-tag">The **%description** Tag</span>

The **%description** tag is used to provide an in-depth description of
the packaged software. The description should be several sentences
describing, to an uninformed user, what the software does.

The **%description** tag is a bit different than the other tags in the
preamble. For one, it starts with a percent sign. The other difference
is that the data specified by the **%description** tag can span more
than one line. In addition, a primitive formatting capability exists. If
a line starts with a space, that line will be displayed verbatim by RPM.
Lines that do not start with a space are assumed to be part of a
paragraph and will be formatted by RPM. It's even possible to mix and
match formatted and unformatted lines. Here are some examples:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%description
It slices!  It dices!  It&#39;s a CD player app that can&#39;t be beat.  By using
the resonant frequency of the CD itself, it is able to simulate 20X
oversampling.  This leads to sound quality that cannot be equaled with
more mundane software...

            </code></pre></td>
</tr>
</tbody>
</table>

The example above contains no explicit formatting. RPM will format the
text as a single paragraph, breaking lines as needed.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%description
 It slices!
 It dices!
 It&#39;s a CD player app that can&#39;t be beat.
By using the resonant frequency of the CD itself, it is able to simulate
20X oversampling.  This leads to sound quality that cannot be equaled with
more mundane software...

            </code></pre></td>
</tr>
</tbody>
</table>

In this example, the first three lines will be displayed by RPM,
verbatim. The remainder of the text will be formatted by RPM. The text
will be formatted as one paragraph.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%description
 It slices!
 It dices!
 It&#39;s a CD player app that can&#39;t be beat.

By using the resonant frequency of the CD itself, it is able to simulate
20X oversampling.  This leads to sound quality that cannot be equaled with
more mundane software...

            </code></pre></td>
</tr>
</tbody>
</table>

Above, we have a similar situation to the previous example, in that part
of the text is formatted and part is not. However, the blank line
separates the text into two paragraphs.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-summary-tag">The **summary** Tag</span>

The **summary** tag is used to define a one-line description of the
packaged software. Unlike **%description**, **summary** is restricted to
one line. RPM uses it when a succinct description of the package is
needed. Here is an example of a **summary** line:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Summary: A CD player app that rocks!

            </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect3">

### <span id="s3-rpm-inside-license-tag">The **license** Tag</span>

The **license** tag is used to define the license terms applicable to
the software being packaged. This tag is also known as the **copyright**
tag. In many cases, this might be nothing more than "**GPL**", for
software distributed under the terms of the GNU General Public License,
or something similar. For example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>License: GPL

            </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect3">

### <span id="s3-rpm-inside-distribution-tag">The **distribution** Tag</span>

The **distribution** tag is used to define a group of packages, of which
this package is a part. Since Red Hat is in the business of producing a
group of packages known as a Linux *distribution*, the name stuck. For
example, if a suite of applications known as "Doors '95" were produced,
each package that is part of the suite would define its **distribution**
line like this:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Distribution: Doors &#39;95

            </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect3">

### <span id="s3-rpm-inside-icon-tag">The **icon** Tag</span>

The **icon** tag is used to name a file containing an icon representing
the packaged software. The file may be in either GIF or XPM format,
although XPM is preferred. In either case, the background of the icon
should be transparent. The file should be placed in RPM's `SOURCES`
directory prior to performing a build, so no path is needed.

The icon is normally used by graphically-oriented front ends to RPM. RPM
itself doesn't use the icon, but it's stored in the package file and
retained in RPM's database after the package is installed. An example
**icon** tag might look like:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Icon: foo.xpm

            </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect3">

### <span id="s3-rpm-inside-vendor-tag">The **vendor** Tag</span>

The **vendor** tag is used to define the name of the entity that is
responsible for packaging the software. Normally, this would be the name
of an organization. Here's an example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Vendor: White Socks Software, Inc.

            </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect3">

### <span id="s3-rpm-inside-url-tag">The **url** Tag</span>

The **url** tag is used to define a Uniform Resource Locator that can be
used to obtain additional information about the packaged software. At
present, RPM doesn't actively make use of this tag. The data is stored
in the package however, and will be written into RPM's database when the
package is installed. It's only a matter of time before some web-based
RPM adjunct makes use of this information, so make sure you include
URLs\! Something like this is all you'll need:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>URL: http://www.gnomovision.com/cdplayer.md

            </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect3">

### <span id="s3-rpm-inside-group-tag">The **group** Tag</span>

The **group** tag is used to group packages together by the types of
functionality they provide. The group specification looks like a path
and is similar in function, in that it specifies more general groupings
before more detailed ones. For example, a package containing a text
editor might have the following group:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Group: Applications/Editors

            </code></pre></td>
</tr>
</tbody>
</table>

In this example, the package is part of the `Editors` group, which is
itself a part of the `Applications` group. Likewise, a spreadsheet
package might have this group:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Group: Applications/Spreadsheets

            </code></pre></td>
</tr>
</tbody>
</table>

This **group** tag indicates that under the `Applications` group, we
would find `Editors` and `Spreadsheets`, and probably some other
subgroups as well.

How is this information used? It's primarily meant to permit graphical
front-ends to RPM, to display packages in a hierarchical fashion. Of
course, in order for groups to be as effective as possible, it's
necessary for all package builders to be consistent in their groupings.
In the case of packages for Linux, Red Hat has the definitive list.
Therefore, Linux package builders should give serious consideration to
using Red Hat's groups. The current group hierarchy is installed with
every copy of RPM, and is available in the RPM sources as well. Check
out the file `groups` in RPM's documentation directory (normally
`/usr/share/doc/rpm-<version>`), or in the top-level source directory.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-packager-tag">The **packager** Tag</span>

The **packager** tag is used to hold the name and contact information
for the person or persons who built the package. Normally, this would be
the person that actually built the package, or in a larger organization,
a public relations contact. In either case, contact information such as
an e-mail address or phone number should be included, so customers can
send either money or hate mail, depending on their satisfaction with the
packaged software. Here's an example of a **packager** tag:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Packager: Fred Foonly &lt;fred@gnomovision.com&gt;

            </code></pre></td>
</tr>
</tbody>
</table>

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-inside-dependency-tags">Dependency Tags</span>

One RPM feature that's been recently implemented is a means of ensuring
that if a package is installed, the system environment has everything
the package requires in order to operate properly. Likewise, when an
installed package is erased RPM can make sure no other package relies on
the package being erased. This dependency capability can be very helpful
when end users install and erase packages on their own. It makes it more
difficult for them to paint themselves into a corner, package-wise.

However, in order for RPM to be able to take more than basic dependency
information into account, the package builder must add the appropriate
dependency information to the package. This is done by using the
following tags. Note, however, that adding dependency information to a
package requires some forethought. For additional information on RPM's
dependency processing, please review [Chapter 14](ch-rpm-depend.md).

<div class="sect3">

### <span id="s3-rpm-inside-provides-tag">The **provides** Tag</span>

The **provides** tag is used to specify a *virtual package* that the
packaged software makes available when it is installed. Normally, this
tag would be used when different packages provide equivalent services.
For example, any package that allows a user to read mail might provide
the `mail-reader` virtual package. Another package that depends on a
mail reader of some sort, could require the `mail-reader` virtual
package. It would then install without dependency problems, if any one
of several mail programs were installed. Here's what a **provides** tag
might look like:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Provides: mail-reader

            </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect3">

### <span id="s3-rpm-inside-requires-tag">The **requires** Tag</span>

The **requires** tag is used to alert RPM to the fact that the package
needs to have certain capabilities available in order to operate
properly. These capabilities refer to the name of another package, or to
a virtual package provided by one or more packages that use the
**provides** tag. When the **requires** tag references a package name,
version comparisons may also be included by following the package name
with **\<**, **\>**, **=**, **\>=**, or **\<=**, and a version
specification. To get even more specific, a package's release may be
included as well. Here's a **requires** tag in action, with a specific
version requirement:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Requires: playmidi = 2.3

            </code></pre></td>
</tr>
</tbody>
</table>

If the **Requires** tag needs to perform a comparison against an epoch
number defined with the **epoch** tag (described below), then the proper
format would be:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Requires: playmidi &gt;= 4:2.3

            </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect3">

### <span id="s3-rpm-inside-conflicts-tag">The **conflicts** Tag</span>

The **conflicts** tag is the logical complement to the **requires** tag.
The **requires** tag is used to specify what packages *must* be present
in order for the current package to operate properly. The **conflicts**
tag is used to specify what packages *cannot* be installed if the
current package is to operate properly.

The **conflicts** tag has the same format as the **requires** tag —
namely, the tag is followed by a real or virtual package name. Like
**requires**, the **conflicts** tag also accepts version and release
specifications:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Conflicts: playmidi = 2.3-1

            </code></pre></td>
</tr>
</tbody>
</table>

If the **conflicts** tag needs to perform a comparison against an epoch
number defined with the **epoch** tag (described below), then the proper
format would be:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Conflicts: playmidi = 4:

            </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect3">

### <span id="s3-rpm-inside-epoch-tag">The **epoch** Tag</span>

The **epoch** tag is another part of RPM's dependency and upgrade
processing. It is also known as the **serial** tag. The need for it is
somewhat obscure, but goes something like this:

1.  The package being built (call it package `A`) uses a version
    numbering scheme sufficiently obscure so that RPM cannot determine
    if one version is older or newer than another version.

2.  Another package (package `B`) requires that package `A` be
    installed. More specifically, it requires RPM to compare package
    `A`'s version against a specified minimum (or maximum) version.

Since RPM is unable to compare package `A`'s version against the version
specified by package `B`, there is no way to determine if package `B`'s
dependency requirements can be met. What to do?

The **epoch** tag provides a way to get around this tricky problem. By
specifying a simple integer epoch number for each version, you are, in
essence, directing how RPM interprets the relative age of the package.
The key point to keep in mind is that in order for this to work, a
unique epoch number must be defined for each version of the software
being packaged. In addition, the epoch number must increment along with
the version. Finally, the package that requires the epoched software
needs to specify *its* version requirements in terms of the epoch
number.

Does it sound like a lot of trouble? You're right\! If you find yourself
in the position of needing to use this tag, take a deep breath and
seriously consider changing the way you assign version numbers. If
you're packaging someone else's software, perhaps you can convince them
to make the change. Chances are, if RPM can't figure out the version
number, most people can't, either\! An example **epoch** tag would look
something like this:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Epoch: 4

            </code></pre></td>
</tr>
</tbody>
</table>

Note that RPM considers a package with an epoch number as newer than a
package without an epoch number.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-autoreqprov-tag">The **autoreqprov**, **autoreq**, and **autoprov** Tags</span>

The **autoreqprov**, **autoreq**, and **autoprov** tags are used to
control the automatic dependency processing performed when the package
is being built. Normally, as each package is built, the following steps
are performed:

  - All executable programs and libraries being packaged are analyzed to
    determine their shared library requirements as well as interpreters.
    These requirements are automatically added to the package's
    requirements.

  - The soname of each shared library being packaged is automatically
    added to the package's list of "provides" information.

  - The required modules for all Perl scripts and modules being packaged
    are automatically added to the package's requirements.

By doing this, RPM reduces the need for package builders to manually add
dependency information to their packages. However, there are times when
RPM's automatic dependency processing may not be desirable. In those
cases the **autoreqprov**, **autoreq**, and **autoprov** tags can be
used to disable automatic dependency processing altogether
(**autoreqprov**), for requirements only (**autoreq**), or for
"provides" only (**autoprov**).

To disable automatic dependency processing both for requirements and
"provides", add the following line:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>AutoReqProv: no

            </code></pre></td>
</tr>
</tbody>
</table>

To disable automatic processing of requirements, add the following line:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>AutoReq: no

            </code></pre></td>
</tr>
</tbody>
</table>

To disable automatic processing of "provides", add the following line:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>AutoProv: no

            </code></pre></td>
</tr>
</tbody>
</table>

(The number zero may be used instead of **no**) Although RPM defaults to
performing automatic dependency processing, the effect of the
**autoreqprov**, **autoreq**, and **autoprov** tags can be reversed by
changing **no** to **yes**. (The number one may be used instead of
**yes**)

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-inside-arch-and-os-tags">Architecture- and Operating System-Specific Tags</span>

As RPM gains in popularity, more people are putting it to work on
different types of computer systems. While this would not normally be a
problem, things start to get a little tricky when one of the following
two situations becomes commonplace:

1.  A particular operating system is ported to several different
    hardware platforms, or architectures.

2.  A particular architecture runs several different operating systems.

The real bind hits when RPM is used to package software for several of
these different system environments. Without methods of controlling the
build process based on architecture and operating system, package
builders that develop software for more than one architecture or
operating system will have a hard time indeed. The only alternative
would be to maintain parallel RPM build environments and accept all the
coordination headaches that would entail.

Fortunately, RPM makes it all easier than that. With the following tags,
it's possible to support package building under multiple environments,
all from a single set of sources, patches, and a single spec file. For a
more complete discussion of multi-architecture package building, please
see [Chapter 19](ch-rpm-multi.md).

<div class="sect3">

### <span id="s3-rpm-inside-excludearch-tag">The **excludearch** Tag</span>

The **excludearch** tag directs RPM to ensure that the package does
*not* attempt to build on the excluded architecture(s). The reasons for
preventing a package from building on a certain architecture might
include:

  - The software has not yet been ported to the excluded architecture.

  - The software would serve no purpose on the excluded architecture.

An example of the first case might be that the software was designed
based on the assumption that an integer is a 32-bit quantity. Obviously,
this assumption is not valid on a 64-bit processor.

In the second case, software that depended on or manipulated low-level
features of a given architecture, should be excluded from building on a
different architecture. Assembly language programs would fall into this
category.

One or more architectures may be specified after the **excludearch**
tag, separated by either spaces or commas. Here is an example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>ExcludeArch: sparc alpha

            </code></pre></td>
</tr>
</tbody>
</table>

In this example, RPM would not attempt to build the package on either
the Sun SPARC or Digital Alpha/AXP architectures. The package would
build on any other architectures, however. If a build is attempted on an
excluded architecture, the following message will be displayed, and the
build will fail:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba cdplayer-1.0.spec
Arch mismatch!
cdplayer-1.0.spec doesn&#39;t build on this architecture

#
            </code></pre></td>
</tr>
</tbody>
</table>

Note that if your goal is to ensure that a package will only build on
*one* architecture, then you should use the **exclusivearch** tag.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-exclusivearch-tag">The **exclusivearch** Tag</span>

The **exclusivearch** tag is used to direct RPM to ensure the package is
*only* built on the specified architecture(s). The reasons for this are
similar to the those mentioned in the section on the **excludearch** tag
above. However, the **exclusivearch** tag is useful when the package
builder needs to ensure that *only* the specified architectures will
build the package. This tag ensures that no future architectures will
mistakenly attempt to build the package. This would not be the case if
the **excludearch** tag were used to specify every architecture known at
the time the package is built.

The syntax of the **exclusivearch** tag is identical to that of
**excludearch**:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>ExclusiveArch: sparc alpha

            </code></pre></td>
</tr>
</tbody>
</table>

In this example, the package will only build on a Sun SPARC or Digital
Alpha/AXP system.

Note that if your goal is to ensure that a package will *not* build on
specific architectures, then you should use the **excludearch** tag.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-excludeos">The **excludeos** Tag</span>

The **excludeos** tag is used to direct RPM to ensure that the package
does *not* attempt to build on the excluded operating system(s). This is
usually necessary when a package is to be built on more than one
operating system, but it is necessary to keep a particular operating
system from attempting a build.

Note that if your goal is to ensure that a package will only build on
*one* operating system, then you should use the **exclusiveos** tag.
Here's a sample **excludeos** tag:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>ExcludeOS: linux irix

            </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect3">

### <span id="s3-rpm-inside-exclusiveos-tag">The **exclusiveos** Tag</span>

The **exclusiveos** tag has the same syntax as **excludeos**, but it has
the opposite logic. The **exclusiveos** tag is used to denote which
operating system(s) should *only* be be permitted to build the package.
Here's **exclusiveos** in action:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>ExclusiveOS: linux

            </code></pre></td>
</tr>
</tbody>
</table>

Note that if your goal is to ensure that a package will *not* build on a
specific operating system, then you should use the **excludeos** tag.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-inside-directory-related-tags">Directory-related Tags</span>

A number of tags are used to specify directories and paths that are used
in various phases of RPM's build and install processes. There's not much
more to say collectively about these tags, so let's dive right in and
look them over.

<div class="sect3">

### <span id="s3-rpm-inside-prefix-tag">The **prefix** Tag</span>

The **prefix** tag is used when a relocatable package is to be built. A
relocatable package can be installed normally or can be installed in a
user-specified directory, by using RPM's **--prefix** install-time
option. The data specified after the **prefix** tag should be the part
of the package's path that may be changed during installation. For
example, if the following **prefix** line was included in a spec file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Prefix: /opt

            </code></pre></td>
</tr>
</tbody>
</table>

and the following file was specified in the spec file's **%files** list:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>/opt/blather/foonly

            </code></pre></td>
</tr>
</tbody>
</table>

then the file `foonly` would be installed in `/opt/blather` if the
package was installed normally. It would be installed in
`/usr/local/blather` if the package was installed with the **--prefix
`/usr/local`** option.

For more information about creating relocatable packages, see [Chapter
15](ch-rpm-reloc.md).

</div>

<div class="sect3">

### <span id="s3-rpm-inside-buildroot-tag">The **buildroot** Tag</span>

The **buildroot** tag is used to define an alternate build root. The
name is a bit misleading, as the build root is actually used when the
software is *installed* during the build process. In order for a build
root to be defined and actually used, a number of issues must be taken
into account. These issues are covered in [Chapter
16](ch-rpm-anywhere.md). This is what a **buildroot** tag would look
like:

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

The **buildroot** tag can be overridden at build-time by using the
**--buildroot** command-line option.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-inside-source-and-patch-tags">Source and Patch Tags</span>

In order to build and package software, RPM needs to know where to find
the original sources. But it's not quite that simple. There might be
more than one set of sources that need to be part of a particular build.
In some cases, it might be necessary to prevent some sources from being
packaged.

And then there is the matter of patches. It's likely that changes will
need to be made to the sources, so it's necessary to specify a patch
file. But the same issues that apply to source specifications are also
applicable to patches. There might be more than one set of patches
required.

The tags that follow are crucial to RPM, so it pays to have a firm grasp
of how they are used.

<div class="sect3">

### <span id="s3-rpm-inside-source-tag">The **source** Tag</span>

The **source** tag is central to nearly every spec file. Although it has
only one piece of data associated with it, it actually performs two
functions:

1.  It shows where the software's developer has made the original
    sources available.

2.  It gives RPM the name of the original source file.

While there is no hard and fast rule, for the first function, it's
generally considered best to put this information in the form of a
Uniform Resource Locator (URL). The URL should point directly to the
source file itself. This is due to the **source** tag's second function.

As mentioned above, the **source** tag also needs to direct RPM to the
source file on the build system. How does it do this? There's only one
requirement, and it is ironclad: The source filename must be at the end
of the line as the final element in a path. Here's an example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Source: ftp://ftp.gnomovision.com/pub/cdplayer-1.0.tgz

            </code></pre></td>
</tr>
</tbody>
</table>

Given this **source** line, RPM will search its `SOURCES` directory for
`cdplayer-1.0.tgz`. Everything prior to the filename is ignored by RPM.
It's there strictly for any interested humans.

A spec file may contain more than one **source** tag. This is necessary
for those cases where the software being packaged is contained in more
than one source file. However, the **source** tags must be uniquely
identified. This is done by appending a number to the end of the tag
itself. In fact, RPM does this internally for the first **source** tag
in a spec file, in essence turning it into **source0**. Therefore, if a
package contains two source files, they may either be specified as:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Source: blather-4.5.tar.gz
Source1: bother-1.2.tar.gz

            </code></pre></td>
</tr>
</tbody>
</table>

or as:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Source0: blather-4.5.tar.gz
Source1: bother-1.2.tar.gz

            </code></pre></td>
</tr>
</tbody>
</table>

Either approach may be used. The choice is yours.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-nosource-tag">The **nosource** Tag</span>

The **nosource** tag is used to direct RPM to omit one or more source
files from the source package. Why would someone want to go to the
trouble of specifying a source file, only to exclude it? The reasons for
this can be varied, but let's look at one example: The software known as
Pretty Good Privacy, or PGP.

PGP contains encryption routines of such high quality that the United
States government restricts their export.
[<span class="footnote">\[1\]</span>](#FTN.AEN7791) While it would be
nice to create a PGP package file, the resulting package could not
legally be transferred between the U.S. and other countries, or
vice-versa.

However, what if all files other than the original source, were packaged
using RPM? Well, a binary package made without PGP would be of little
use, but what about the source package? It would contain the spec file,
maybe some patches, and perhaps even an icon file. Since the
controversial PGP software was not a part of the source package, this
sanitized source package could be downloaded legally in any country. The
person that downloaded a copy could then go about legally obtaining the
PGP sources themselves, place them in RPM's `SOURCES` directory, and
create a binary package. They wouldn't even need to change the
**nosource** tag. One **rpmbuild** **-ba** command later, and the user
would have a perfectly usable PGP binary package file.

Since there may be more than one **source** tag in a spec file, the
format of the **nosource** tag is as follows:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>nosource: &lt;src-num&gt;, &lt;src-num&gt;…&lt;src-num&gt;

            </code></pre></td>
</tr>
</tbody>
</table>

The **`<src-num>`** represents the number following the **source** tag.
If there is more than one number in the list, they may be separated by
either commas or spaces. For example, consider a package containing the
following **source** tags:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>source: blather-4.5.tar.gz
Source1: bother-1.2.tar.gz
source2: blather-lib-4.5.tar.gz
source3: bother-lib-1.2.tar.gz

            </code></pre></td>
</tr>
</tbody>
</table>

If the source files for `blather` and `blather-lib` were not to be
included in the package, the following **nosource** line could be added:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>NoSource: 0, 3

            </code></pre></td>
</tr>
</tbody>
</table>

What about that **0**? Keep in mind that the first unnumbered **source**
tag in a spec file is automatically numbered 0 by RPM.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-patch-tag">The **patch** Tag</span>

The **patch** tag is used to identify which patches are associated with
the software being packaged. The patch files are kept in RPM's `SOURCES`
directory, so only the name of the patch file should be specified. Here
is an example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Patch: cdp-0.33-fsstnd.patch

            </code></pre></td>
</tr>
</tbody>
</table>

There are no hard and fast requirements for naming the patch files, but
traditionally the filename starts with the software name and version,
separated by dashes. The next part of the patch file name usually
includes one or more words indicating the reason for the patch. In our
example above, the patch file contains changes necessary to bring the
software into compliance with the Linux File System Standard, hence the
fsstnd magic incantation.

RPM processes **patch** tags the same way it does **source** tags.
Therefore, it's acceptable to use a Uniform Resource Locator (URL) on a
**patch** line, too.

A spec file may contain more than one **patch** tag. This is necessary
for those cases where the software being packaged requires more than one
patch. However, the **patch** tags must be uniquely identified. This is
done by appending a number to the end of the tag itself. In fact, RPM
does this internally for the first **patch** tag in a spec file, in
essence turning it into **patch0**. Therefore, if a package contains
three patches, the following two methods of specifying them are
equivalent:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Patch: blather-4.5-bugfix.patch
Patch1: blather-4.5-config.patch
Patch2: blather-4.5-somethingelse.patch

            </code></pre></td>
</tr>
</tbody>
</table>

This is the same as:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Patch0: blather-4.5-bugfix.patch
Patch1: blather-4.5-config.patch
Patch2: blather-4.5-somethingelse.patch

            </code></pre></td>
</tr>
</tbody>
</table>

Either approach may be used, but the second method looks nicer.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-nopatch-tag">The **nopatch** Tag</span>

The **nopatch** tag is similar to the **nosource** tag discussed
earlier. Just like the **nosource** tag, the **nopatch** tag is used to
direct RPM to omit something from the source package. In the case of
**nosource**, that "something" was one or more sources. For the
**nopatch** tag, the "something" is one or more patches.

Since each **patch** tag in a spec file must be numbered, the
**nopatch** tag uses those numbers to specify which patches are not to
be included in the package. The **nopatch** tag is used in this manner:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>NoPatch: 2 3

            </code></pre></td>
</tr>
</tbody>
</table>

In this example, the source files specified on the **source2** and
**source3** lines are not to be included in the build.

This concludes our study of RPM's tags. In the next section, we'll look
at the various scripts that RPM uses to build, as well as to install,
and erase, packages.

</div>

</div>

</div>

### Notes

|                                                                        |                                                                                                                     |
| ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-inside-tags.md#AEN7791) | There is also an "international" version that may be used in non-US countries. See [Appendix G](ch-pgp-intro.md). |

<div class="NAVFOOTER">

-----

|                            |                          |                                    |
| :------------------------- | :----------------------: | ---------------------------------: |
| [Prev](ch-rpm-inside.md) |    [Home](index.md)    | [Next](s1-rpm-inside-scripts.md) |
| Inside the Spec File       | [Up](ch-rpm-inside.md) |           Scripts: RPM's Workhorse |

</div>
