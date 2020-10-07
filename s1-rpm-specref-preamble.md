<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-specref.html)

Appendix E. Concise Spec File Reference

[Next](s1-rpm-specref-scripts.html)

-----

<div class="sect1">

# <span id="s1-rpm-specref-preamble">The Preamble</span>

This section outlines the tags that comprise a spec file's preamble.

<div class="sect2">

## <span id="s2-rpm-specref-package-naming-tags">Package Naming Tags</span>

This section outlines the tags that are used to name a package.

<div class="sect3">

### <span id="s3-rpm-specref-name">The **Name:** Tag</span>

The **Name:** tag is used to define the name of the software being
packaged.

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

See also: [the Section called *The **name** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-name-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-version">The **Version:** Tag</span>

The **Version:** tag defines the version of the software being packaged.

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

See also: [the Section called *The **version** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-version-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-release">The **Release:** Tag</span>

The **Release:** tag can be thought of as the *package's* version.

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

See also: [the Section called *The **release** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-release-tag).

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-specref-descriptive-tags">Descriptive Tags</span>

<div class="sect3">

### <span id="s3-rpm-specref-description">**%description** Directive -- Describe the packages intended use.</span>

The **%description** tag is used to define an in-depth description of
the packaged software. In the descriptive text, a space in the first
column indicates that that line of text should be presented to user
as-is, with no formatting done by RPM. Blank lines in the descriptive
text denote paragraphs.

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

The **%description** tag can be made specific to a particular subpackage
by adding the subpackage name, and optionally, the `-n` option:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%description bar

%description -n bar

          </code></pre></td>
</tr>
</tbody>
</table>

The subpackage name and usage of the `-n` option must match those
defined with the **%package** directive.

See also: [the Section called *The **%description** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-description-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-summary">The **Summary:** Tag</span>

The **Summary:** tag is used to define a one-line description of the
packaged software.

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

See also: [the Section called *The **summary** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-summary-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-license">The **License:** Tag</span>

The **License:** tag is used to define the license terms applicable to
the software being packaged. This tag is also known as the
**Copyright:** tag.

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

See also: [the Section called *The **license** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-license-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-distribution">The **Distribution:** Tag</span>

The **Distribution:** tag is used to define a group of packages, of
which this package is a part.

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

See also: [the Section called *The **distribution** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-distribution-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-icon">The **Icon:** Tag</span>

The **Icon:** tag is used to name a file containing an icon representing
the packaged software. The file may be in either GIF or XPM format,
although XPM is preferred. In either case, the background of the icon
should be transparent.

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

See also: [the Section called *The **icon** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-icon-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-vendor">The **Vendor:** Tag</span>

The **Vendor:** tag is used to define the name of the entity that is
responsible for packaging the software.

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

See also: [the Section called *The **vendor** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-vendor-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-url">The **URL:** Tag</span>

The **URL:** tag is used to define a Uniform Resource Locator that can
be used to obtain additional information about the packaged software.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>URL: http://www.gnomovision.com/cdplayer.html

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **url** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-url-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-group">The **Group:** Tag</span>

The **Group:** tag is used to group packages together by the types of
functionality they provide.

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

See also: [the Section called *The **group** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-group-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-packager">The **Packager:** Tag</span>

The **Packager:** tag is used to hold the name and contact information
for the person or persons who built the package.

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

See also: [the Section called *The **packager** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-packager-tag).

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-specref-dependency-tags">Dependency Tags</span>

<div class="sect3">

### <span id="s3-rpm-specref-provides">The **Provides:** Tag</span>

The **Provides:** tag is used to specify a "virtual package" that the
packaged software makes available when it is installed.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Provides: module-info

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **provides** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-provides-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-requires">The **Requires:** Tag</span>

The **Requires:** tag is used to alert RPM to the fact that the package
needs to have certain capabilities available in order to operate
properly.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Requires: playmidi

          </code></pre></td>
</tr>
</tbody>
</table>

A version may be specified, following the package specification. The
following comparison operators may be placed between the package and
version:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>&lt;, &gt;, =, &gt;=, or &lt;=

          </code></pre></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Requires: playmidi &gt;= 2.3

          </code></pre></td>
</tr>
</tbody>
</table>

If the **Requires:** tag needs to perform a comparison against an epoch
numbered defined with the **Epoch:** tag, then the proper format would
be:

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

See also: [the Section called *The **requires** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-requires-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-epoch">The **Epoch:** Tag</span>

The **Epoch:** tag is used to define an epoch number for a package. It
replaces the now obsoleted **Serial:** tag. This is only necessary if
RPM is unable to determine the ordering of a package's version numbers.

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

See also: [the Section called *The **epoch** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-epoch-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-conflicts">The **Conflicts:** Tag</span>

The **Conflicts:** tag is used to alert RPM to the fact that the package
is not compatible with other packages.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Conflicts: playmidi

          </code></pre></td>
</tr>
</tbody>
</table>

A version may be specified, following the package specification. The
following comparison operators may be placed between the package and
version:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>&lt;, &gt;, =, &gt;=, or &lt;=

          </code></pre></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Conflicts: playmidi &gt;= 2.3

          </code></pre></td>
</tr>
</tbody>
</table>

If the **Conflicts:** tag needs to perform a comparison against an epoch
numbered defined with the **Epoch:** tag, then the proper format would
be:

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

See also: [the Section called *The **conflicts** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-conflicts-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-autoreqprov">The **AutoReqProv:**, **AutoReq:**, and **AutoProv:** Tags</span>

The **AutoReqProv:** tag is used to control the automatic dependency
processing performed when the package is being built. To disable
automatic dependency processing, add the following line:

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

(The number `0` may be used instead of `no`) Although RPM defaults to
performing automatic dependency processing, the effect of the
**AutoReqProv:** tag can be reversed by changing `no` to `yes`. (The
number `1` may be used instead of `yes`)

The **AutoReq:** and **AutoProv:** tags can be used to disable automatic
processing of requirements or "provides" only, respectively.

See also: [the Section called *The **autoreqprov**, **autoreq**, and
**autoprov** Tags* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-autoreqprov-tag).

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-specref-arch-os-specific-tag">Architecture- and Operating System-Specific Tags</span>

<div class="sect3">

### <span id="s3-rpm-specref-excludearch">The **ExcludeArch:** Tag</span>

The **ExcludeArch:** tag is used to direct RPM to ensure that the
package does *not* attempt to build on the excluded architecture(s).

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

See also: [the Section called *The **excludearch** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-excludearch-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-exclusivearch">The **ExclusiveArch:** Tag</span>

The **ExclusiveArch:** tag is used to direct RPM to ensure the package
is *only* built on the specified architecture(s).

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

See also: [the Section called *The **exclusivearch** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-exclusivearch-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-excludeos">The **ExcludeOs:** Tag</span>

The **ExcludeOs:** tag is used to direct RPM to ensure that the package
does *not* attempt to build on the excluded operating system(s).

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

See also: [the Section called *The **excludeos** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-excludeos).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-exclusiveos">The **ExclusiveOs:** Tag</span>

The **ExclusiveOs:** tag is used to denote which operating system(s)
should *only* be be permitted to build the package.

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

See also: [the Section called *The **exclusiveos** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-exclusiveos-tag).

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-specref-directory-related-tags">Directory-related Tags</span>

<div class="sect3">

### <span id="s3-rpm-specref-prefix">The **Prefix:** Tag</span>

The **Prefix:** tag is used to define part of the path RPM will use when
installing the package's files. The prefix can be redefined by the user
when the package is installed, thereby changing where the package is
installed.

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

See also: [the Section called *The **prefix** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-prefix-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-buildroot">The **BuildRoot:** Tag</span>

The **BuildRoot:** tag is used to define an alternate build root, where
the software will be installed during the build process.

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

See also: [the Section called *The **buildroot** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-buildroot-tag).

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-specref-source-patch-tags">Source and Patch Tags</span>

<div class="sect3">

### <span id="s3-rpm-specref-source">The **Source:** Tag</span>

The **Source:** tag is used to define the filename of the sources to be
packaged. When there is more than one **Source:** tag in a spec file,
each one must be numbered so they are unique, starting with the number
`0`. When there is only one tag, it does not need to be numbered.

By convention, the source filename is usually preceded by a URL pointing
to the location of the original sources, but RPM does not require this.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Source0: ftp://ftp.gnomovision.com/pub/cdplayer-1.0.tgz
Source1: foo.tgz

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **source** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-source-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-nosource">The **NoSource:** Tag</span>

The **NoSource:** tag is used to alert RPM to the fact that one or more
source files should be excluded from the source package file. The tag is
followed by one or more numbers. The numbers correspond to the numbers
following the **Source:** tags that are to be excluded from packaging.

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

See also: [the Section called *The **nosource** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-nosource-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-patch">The **Patch:** Tag</span>

The **Patch:** tag is used to define the name of a patch file to be
applied to the package's sources. When there is more than one **Patch:**
tag in a spec file, each one must be numbered so they are unique,
starting with the number `0`. When there is only one tag, it does not
need to be numbered.

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

See also: [the Section called *The **patch** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-patch-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-nopatch">The **NoPatch:** Tag</span>

The **NoPatch:** tag is used to alert RPM to the fact that one or more
patch files should be excluded from the source package file. The tag is
followed by one or more numbers. The numbers correspond to the numbers
following the **Patch:** tags that are to be excluded from packaging.

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

See also: [the Section called *The **nopatch** Tag* in Chapter
13](s1-rpm-inside-tags.html#s3-rpm-inside-nopatch-tag).

</div>

</div>

</div>

<div class="NAVFOOTER">

-----

|                             |                           |                                     |
| :-------------------------- | :-----------------------: | ----------------------------------: |
| [Prev](ch-rpm-specref.html) |    [Home](index.html)     | [Next](s1-rpm-specref-scripts.html) |
| Concise Spec File Reference | [Up](ch-rpm-specref.html) |                          Scriptlets |

</div>
