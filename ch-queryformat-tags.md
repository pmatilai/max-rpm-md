<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-commands-rebuild-database-mode.md)

[Next](ch-rpm-specref.md)

-----

<div class="appendix">

# <span id="ch-queryformat-tags"></span>Appendix D. Available Tags For **--queryformat**

The following tags were defined at the time this book was written. For
the latest list of available queryformat tags, please issue the
following command:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>rpm --querytags
    </code></pre></td>
</tr>
</tbody>
</table>

Keep in mind that the list of tags produced by the **--querytags**
option is the complete list of all tags used by RPM internally; for
instance, during package builds. Because of this, some tags do not
produce meaningful output when used in a **--queryformat** format
string.

<div class="sect1">

# <span id="s1-queryformat-tags-queryformat-tags">List of **--queryformat** Tags</span>

For every tag in this section, there can be as many as three different
pieces of information:

1.  A short description of the tag.

2.  Whether the data specified by the tag is an array, and if so, how
    many members are present in the array.

3.  What modifiers can be used with the tag.

<div class="sect2">

## <span id="s2-queryformat-tags-name">The **NAME** Tag</span>

The **NAME** tag is used to display the name of the package.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-version">The **VERSION** Tag</span>

The **VERSION** tag is used to display the version of the packaged
software.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-release">The **RELEASE** Tag</span>

The **RELEASE** tag is used to display the release number of the
package.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-epoch">The **EPOCH** Tag</span>

The **EPOCH** tag is used to display the epoch number of the package.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-summary">The **SUMMARY** Tag</span>

The **SUMMARY** tag is used to display a one-line summation of the
packaged software.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-description">The **DESCRIPTION** Tag</span>

The **DESCRIPTION** tag is used to display a detailed summation of the
packaged software.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-buildtime">The **BUILDTIME** Tag</span>

The **BUILDTIME** tag is used to display the time and date the package
was created.

Array: No

Used with modifiers: **:date**

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-buildhost">The **BUILDHOST** Tag</span>

The **BUILDHOST** tag is used to display the hostname of the system that
built the package.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-installtime">The **INSTALLTIME** Tag</span>

The **INSTALLTIME** tag is used to display the time and date the package
was installed.

Array: No

Used with modifiers: **:date**

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-size">The **SIZE** Tag</span>

The **SIZE** tag is used to display the total size, in bytes, of every
file installed by this package.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-distribution">The **DISTRIBUTION** Tag</span>

The **DISTRIBUTION** tag is used to display the distribution this
package is a part of.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-vendor">The **VENDOR** Tag</span>

The **VENDOR** tag is used to display the organization responsible for
marketing the package.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-gif">The **GIF** Tag</span>

The **GIF** tag is not available for use with **--queryformat**.

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-xpm">The **XPM** Tag</span>

The **XPM** tag is not available for use with **--queryformat**.

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-license">The **LICENSE** Tag</span>

The **LICENSE** tag is used to display the distribution license of the
package.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-packager">The **PACKAGER** Tag</span>

The **PACKAGER** tag is used to display the person or persons
responsible for creating the package.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-group">The **GROUP** Tag</span>

The **GROUP** tag is used to display the group to which the package
belongs.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-changelog">The **CHANGELOG** Tag</span>

The **CHANGELOG** tag is reserved for a future version of RPM.

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-source">The **SOURCE** Tag</span>

The **SOURCE** tag is used to display the source archives contained in
the source package file.

Array: Yes (Size: One entry per **source**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-patch">The **PATCH** Tag</span>

The **PATCH** tag is used to display the patch files contained in the
source package file.

Array: Yes (Size: One entry per **patch**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-url">The **URL** Tag</span>

The **URL** tag is used to display the Uniform Resource Locator that
points to additional information on the packaged software.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-os">The **OS** Tag</span>

The **OS** tag is used to display the operating system for which the
package was built.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-arch">The **ARCH** Tag</span>

The **ARCH** tag is used to display the architecture for which the
package was built.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-prein">The **PREIN** Tag</span>

The **PREIN** tag is used to display the package's pre-install script.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-postin">The **POSTIN** Tag</span>

The **POSTIN** tag is used to display the package's post-install script.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-preun">The **PREUN** Tag</span>

The **PREUN** tag is used to display the package's pre-uninstall script.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-postun">The **POSTUN** Tag</span>

The **POSTUN** tag is used to display the package's post-uninstall
script.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-filename">The **FILENAMES** Tag</span>

The **FILENAMES** tag is used to display the names of the files that
comprise the package.

Array: Yes (Size: One entry per **filenames**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-filesizes">The **FILESIZES** Tag</span>

The **FILESIZES** tag is used to display the size, in bytes, of each of
the files that comprise the package.

Array: Yes (Size: One entry per **filesizes**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-filestates">The **FILESTATES** Tag</span>

The **FILESTATES** tag is used to display the state of each of the files
that comprise the package.

Array: Yes (Size: One entry per **filestates**)

Used with modifiers: N/A
[<span class="footnote">\[1\]</span>](#FTN.AEN16282)

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-filemodes">The **FILEMODES** Tag</span>

The **FILEMODES** tag is used to display the permissions of each of the
files that comprise the package.

Array: Yes (Size: One entry per **filemodes**)

Used with modifiers: **:perms**

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-fileuids">The **FILEUIDS** Tag</span>

The **FILEUIDS** tag is used to display the user ID, in numeric form, of
each of the files that comprise the package.

Array: Yes (Size: One entry per **fileuids**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-filegids">The **FILEGIDS** Tag</span>

The **FILEGIDS** tag is used to display the group ID, in numeric form,
of each of the files that comprise the package.

Array: Yes (Size: One entry per **filegids**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-filerdevs">The **FILERDEVS** Tag</span>

The **FILERDEVS** tag is used to display the major and minor numbers for
each of the files that comprise the package. It will only be non-zero
for device special files.

Array: Yes (Size: One entry per **filerdevs**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-filemtimes">The **FILEMTIMES** Tag</span>

The **FILEMTIMES** tag is used to display the modification time and date
for each of the files that comprise the package.

Array: Yes (Size: One entry per **filemtimes**)

Used with modifiers: **:date**

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-filemd5">The **FILEMD5S** Tag</span>

The **FILEMD5S** tag is used to display the MD5 checksum for each of the
files that comprise the package.

Array: Yes (Size: One entry per **filemd5s**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-filelinktos">The **FILELINKTOS** Tag</span>

The **FILELINKTOS** tag is used to display the link string for symlinks.

Array: Yes (Size: One entry per **filelinktos**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-fileflags">The **FILEFLAGS** Tag</span>

The **FILEFLAGS** tag is used to indicate whether the files that
comprise the package have been flagged as being documentation or
configuration.

Array: Yes (Size: One entry per **fileflags**)

Used with modifiers: **:fflags**

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-root">The **ROOT** Tag</span>

The **ROOT** tag is not available for use with **--queryformat**.

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-fileusername">The **FILEUSERNAME** Tag</span>

The **FILEUSERNAME** tag is used to display the owner, in alphanumeric
form, of each of the files that comprise the package.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-filegroupname">The **FILEGROUPNAME** Tag</span>

The **FILEGROUPNAME** tag is used to display the group, in alphanumeric
form, of each of the files that comprise the package.

Array: Yes (Size: One entry per **filegroupname**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-exclude">The **EXCLUDE** Tag</span>

The **EXCLUDE** tag is deprecated and should no longer be used.

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-exclusive">The **EXCLUSIVE** Tag</span>

The **EXCLUSIVE** tag is deprecated and should no longer be used.

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-icon">The **ICON** Tag</span>

The **ICON** tag is not available for use with **--queryformat**.

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-sourcerpm">The **SOURCERPM** Tag</span>

The **SOURCERPM** tag is used to display the name of the source package
from which this binary package was built.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-fileverifyflags">The **FILEVERIFYFLAGS** Tag</span>

The **FILEVERIFYFLAGS** tag is used to display the numeric value of the
file verification flags for each of the files that comprise the package.

Array: Yes (Size: One entry per **fileverifyflags**)

Used with modifiers: N/A
[<span class="footnote">\[2\]</span>](#FTN.AEN16504)

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-arhivesize">The **ARCHIVESIZE** Tag</span>

The **ARCHIVESIZE** tag is used to display the size, in bytes, of the
archive portion of the original package file.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-provides">The **PROVIDES** Tag</span>

The **PROVIDES** tag is used to display the capabilities the package
provides.

Array: Yes (Size: One entry per **provides**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-requireflags">The **REQUIREFLAGS** Tag</span>

The **REQUIREFLAGS** tag is used to display the requirement flags for
each capability the package requires.

Array: Yes (Size: One entry per **requireflags**)

Used with modifiers: **:depflags**

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-requirename">The **REQUIRENAME** Tag</span>

The **REQUIRENAME** tag is used to display the capabilities the package
requires.

Array: Yes (Size: One entry per **requirename**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-requireversion">The **REQUIREVERSION** Tag</span>

The **REQUIREVERSION** tag is used to display the version-related aspect
of each capability the package requires.

Array: Yes (Size: One entry per **requireversion**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-nosource">The **NOSOURCE** Tag</span>

The **NOSOURCE** tag is used to display the source archives that are not
contained in the source package file.

Array: Yes (Size: One entry per **nosource**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-nopatch">The **NOPATCH** Tag</span>

The **NOPATCH** tag is used to display the patch files that are not
contained in the source package file.

Array: Yes (Size: One entry per **nopatch**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-conflictflags">The **CONFLICTFLAGS** Tag</span>

The **CONFLICTFLAGS** tag is used to display the conflict flags for each
capability the package conflicts with.

Array: Yes (Size: One entry per **conflictflags**)

Used with modifiers: **:depflags**

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-conflictname">The **CONFLICTNAME** Tag</span>

The **CONFLICTNAME** tag is used to display the capabilities that the
package conflicts with.

Array: Yes (Size: One entry per **conflictname**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-conflictversion">The **CONFLICTVERSION** Tag</span>

The **CONFLICTVERSION** tag is used to display the version-related
aspect of each capability the package conflicts with.

Array: Yes (Size: One entry per **conflictversion**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-defaultprefix">The **DEFAULTPREFIX** Tag</span>

The **DEFAULTPREFIX** tag is used to display the path that will, by
default, be used to install a relocatable package.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-buildroot">The **BUILDROOT** Tag</span>

The **BUILDROOT** tag is not available for use with **--queryformat**.

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-installprefix">The **INSTALLPREFIX** Tag</span>

The **INSTALLPREFIX** tag is used to display the actual path used when a
relocatable package was installed.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-excludearch">The **EXCLUDEARCH** Tag</span>

The **EXCLUDEARCH** tag is used to display the architectures that should
not install this package.

Array: Yes (Size: One entry per **excludearch**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-excludeos">The **EXCLUDEOS** Tag</span>

The **EXCLUDEOS** tag is used to display the operating systems that
should not install this package.

Array: Yes (Size: One entry per **excludeos**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-exclusivearch">The **EXCLUSIVEARCH** Tag</span>

The **EXCLUSIVEARCH** tag is used to display the architectures that are
the only ones that should install this package.

Array: Yes (Size: One entry per **exclusivearch**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-exclusiveos">The **EXCLUSIVEOS** Tag</span>

The **EXCLUSIVEOS** tag is used to display the operating systems that
are the only one that should install this package.

Array: Yes (Size: One entry per **exclusiveos**)

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-autoreqprov">The **AUTOREQPROV**, **AUTOREQ**, and **AUTOPROV** Tags</span>

The **AUTOREQPROV**, **AUTOREQ**, and **AUTOPROV** tags are not
available for use with **--queryformat**.

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-rpmversion">The **RPMVERSION** Tag</span>

The **RPMVERSION** tag is used to display the version of RPM that was
used to build the package.

Array: No

Used with modifiers: N/A

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-triggerscripts">The **TRIGGERSCRIPTS** Tag</span>

The **TRIGGERSCRIPTS** tag is reserved for a future version of RPM.

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-triggername">The **TRIGGERNAME** Tag</span>

The **TRIGGERNAME** tag is reserved for a future version of RPM.

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-triggerversion">The **TRIGGERVERSION** Tag</span>

The **TRIGGERVERSION** tag is reserved for a future version of RPM.

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-triggerflags">The **TRIGGERFLAGS** Tag</span>

The **TRIGGERFLAGS** tag is reserved for a future version of RPM.

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-triggerindex">The **TRIGGERINDEX** Tag</span>

The **TRIGGERINDEX** tag is reserved for a future version of RPM.

</div>

<div class="sect2">

## <span id="s2-queryformat-tags-verifyscript">The **VERIFYSCRIPT** Tag</span>

The **VERIFYSCRIPT** tag is used to display the script to be used for
package verification.

Array: No

Used with modifiers: N/A

</div>

</div>

</div>

### Notes

|                                                                          |                                                                                                                                                                                                                                                                                                                         |
| ------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](ch-queryformat-tags.md#AEN16282) | Since there is no modifier to display the file states in human-readable form, it will be necessary to manually interpret the flag values, based on the **RPMFILE\_STATE\_`xxx`** **\#define**s contained in `rpmlib.h`. This file is part of the **rpm-devel** package and is also present in the RPM source package.   |
| [<span class="footnote">\[2\]</span>](ch-queryformat-tags.md#AEN16504) | Since there is no modifier to display the verification flags in human-readable form, it will be necessary to manually interpret the flag values, based on the **RPMVERIFY\_`xxx`** **\#define**s contained in `rpmlib.h`. This file is part of the **rpm-devel** package and is also present in the RPM source package. |

<div class="NAVFOOTER">

-----

|                                                    |                    |                             |
| :------------------------------------------------- | :----------------: | --------------------------: |
| [Prev](s1-rpm-commands-rebuild-database-mode.md) | [Home](index.md) | [Next](ch-rpm-specref.md) |
| Rebuild Database Mode                              | [Up](p14028.md)  | Concise Spec File Reference |

</div>
