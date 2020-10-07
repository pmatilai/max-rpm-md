<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpmrc-file-rpmrc-file-syntax.html)

Appendix B. The `rpmrc` File

[Next](ch-rpm-commands.html)

-----

<div class="sect1">

# <span id="s1-rpmrc-file-rpmrc-file-entries">`rpmrc` File Entries</span>

In this section, we discuss the various entries that can be used in each
of the `rpmrc` files.

<div class="sect2">

## <span id="s2-rpmrc-file-arch-canon">**arch\_canon**</span>

The **arch\_canon** entry is used to define a table of architecture
names and their associated numbers. These canonical architecture names
and numbers are then used internally by RPM whenever
architecture-specific processing takes place. This entry's format is:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>arch_canon:&lt;label&gt;: &lt;string&gt; &lt;value&gt;

        </code></pre></td>
</tr>
</tbody>
</table>

The <span class="symbol">`<label>`</span> is compared against
information from **uname(2)** after it's been translated using the
appropriate **buildarchtranslate** entry. If a match is found, then
<span class="symbol">`<string>`</span> is used by RPM to reference the
system's architecture. When building a binary package, RPM uses
<span class="symbol">`<string>`</span> as part of the package's
filename, for instance.

The <span class="symbol">`<value>`</span> is a numeric value RPM uses
internally to identify the architecture. For example, this number is
written in the header of each package file so that the **file** command
can identify the architecture for which the package was built.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-os-canon">**os\_canon**</span>

The **os\_canon** entry is used to define a table of operating system
names and their associated numbers. These canonical operating system
names and numbers are then used internally by RPM whenever operating
system-specific processing takes place. This entry's format is:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>os_canon:&lt;label&gt;: &lt;string&gt; &lt;value&gt;

        </code></pre></td>
</tr>
</tbody>
</table>

The <span class="symbol">`<label>`</span> is compared against
information from **uname(2)** after it's been translated using the
appropriate **buildostranslate** entry.
[<span class="footnote">\[1\]</span>](#FTN.AEN14754) If a match is
found, then <span class="symbol">`<string>`</span> is used by RPM to
reference the operating system.

The <span class="symbol">`<value>`</span> is a numeric value used to
uniquely identify the operating system.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-buildarchtranslate">**buildarchtranslate**</span>

The **buildarchtranslate** entry is used in the process of defining the
architecture that RPM will use as the "build" architecture. As the name
implies, it is used to translate the raw information returned from
**uname(2)** to the canonical architecture defined by **arch\_canon**.

The format of the **buildarchtranslate** entry is slightly different
from most other `rpmrc` file entries. Instead of the usual
<span class="symbol">`<name>:<value>`</span> format, the
**buildarchtranslate** entry looks like this:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>buildarchtranslate:&lt;label&gt;: &lt;string&gt;

        </code></pre></td>
</tr>
</tbody>
</table>

The <span class="symbol">`<label>`</span> is compared against
information from **uname(2)**. If a match is found, then
<span class="symbol">`<string>`</span> is used by RPM to define the
build architecture.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-buildostranslate">**buildostranslate**</span>

The **buildostranslate** entry is used in the process of defining the
operating system RPM will use as the "build" operating system. As the
name implies, it is used to translate the raw information returned by
**uname(2)** to the canonical operating system defined by **os\_canon**.

The format of the **buildostranslate** entry is slightly different from
most other `rpmrc` file entries. Instead of the usual
<span class="symbol">`<name>`:`<value>`</span> format, the
**buildostranslate** entry looks like this:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>buildostranslate:&lt;label&gt;: &lt;string&gt;

        </code></pre></td>
</tr>
</tbody>
</table>

The <span class="symbol">`<label>`</span> is compared against
information from **uname(2)**. If a match is found, then
<span class="symbol">`<string>`</span> is used by RPM to define the
build operating system.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-arch-compat">**arch\_compat**</span>

The **arch\_compat** entry is used to define which architectures are
compatible with one another. This information is used when packages are
installed; in this way, RPM can determine whether a given package file
is compatible with the system. The format of the entry is:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>arch_compat:&lt;label&gt;: &lt;list&gt;

        </code></pre></td>
</tr>
</tbody>
</table>

The <span class="symbol">`<label>`</span> is an architecture string, as
defined by an **arch\_canon** entry. The
<span class="symbol">`<list>`</span> following it consists of one or
more architectures, also defined by **arch\_canon**. If there is more
than one architecture in the list, they should be separated by a space.

The architectures in the list are considered compatible to the
architecture specified in the label.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-os-compat">**os\_compat**</span>

Default value: (operating system-specific)

The **os\_compat** entry is used to define which operating systems are
compatible with one another. This information is used when packages are
installed; in this way, RPM can determine whether a given package file
is compatible with the system. The format of the entry is:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>&lt;name&gt;:&lt;label&gt;: &lt;list&gt;

        </code></pre></td>
</tr>
</tbody>
</table>

The <span class="symbol">`<label>`</span> is an operating system string,
as defined by an **os\_canon** entry. The
<span class="symbol">`<list>`</span> following it consists of one or
more operating systems, also defined by **os\_canon**. If there is more
than one operating system in the list, they should be separated by a
space.

The operating systems in the list are considered compatible to the
operating system specified in the label.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-builddir">**builddir**</span>

Default value: `<topdir>`/BUILD

The **builddir** entry is used to define the path to the directory in
which RPM will build packages. Its default value is taken from the value
of the **topdir** entry, with "`/BUILD`" appended to it. Note that if
you redefine **builddir**, you'll need to specify a complete path.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-buildroot">**buildroot**</span>

Default value: (none)

The **buildroot** entry defines the path used as the root directory
during the install phase of a package build. For more information on
using build roots, please see [the Section called *Using Build Roots in
a Package* in Chapter
16](ch-rpm-anywhere.html#s1-rpm-anywhere-using-build-roots).

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-cpiobin">**cpiobin**</span>

Default value: **cpio**

The **cpiobin** entry is used to define the name (and optionally, path)
of the **cpio** program. RPM uses **cpio** to perform a variety of
functions, and needs to know where the program can be found.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-dbpath">**dbpath**</span>

Default value: `/var/lib/rpm`

The **dbpath** entry is used to define the directory in which the RPM
database files are stored. It can be overridden by using the
**--dbpath** option on the RPM command line.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-defaultdocdir">**defaultdocdir**</span>

Default value: `/usr/doc`

The **defaultdocdir** entry is used to define the directory in which RPM
will store documentation for all installed packages. It is used only
during builds to support the **%doc** directive.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-distribution">**distribution**</span>

Default value: (none)

The **distribution** entry is used to define the distribution for each
package. The distribution can also be set by adding the **distribution**
tag to a particular spec file. The **distribution** tag in the spec file
overrides the **distribution** `rpmrc` file entry.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-excludedocs">**excludedocs**</span>

Default value: <span class="token">0</span>

The **excludedocs** entry is used to control if documentation should be
written to disk when a package is installed. By default, documentation
*is* installed; however, this can be overridden by setting the value of
**excludedocs** to <span class="token">1</span>. Note also that the
**--excludedocs** and **--includedocs** options can be added to the RPM
command line to override the **excludedocs** entry's behavior. For more
information on the **--excludedocs** and **--includedocs** options,
please refer to [Chapter 2](ch-rpm-install.html).

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-ftpport">**ftpport**</span>

Default value: (none)

The **ftpport** entry is used to define the port RPM should use when
manipulating package files via FTP. See [the Section called ***--ftpport
`<port>`**: Use **`<port>`** In FTP-based Installs* in Chapter
2](s1-rpm-install-additional-options.html#s2-rpm-install-ftpport) for
more information on how FTP ports are used by RPM.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-ftpproxy">**ftpproxy**</span>

Default value: (none)

The **ftpproxy** entry is used to define the hostname of the FTP proxy
system RPM should use when manipulating package files via FTP. See [the
Section called ***--ftpproxy `<host>`**: Use **`<host>`** As Proxy In
FTP-based Installs* in Chapter
2](s1-rpm-install-additional-options.html#s2-rpm-install-ftpproxy) for
more information on how FTP proxy systems are used by RPM.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-messagelevel">**messagelevel**</span>

Default value: 3

The **messagelevel** entry is used to define the desired verbosity
level. Levels less than 3 produce greater amounts of output, while
levels greater than 3 produce less output.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-netsharedpath">**netsharedpath**</span>

Default value: (none)

The **netsharedpath** entry is used to define one or more paths that, on
the local system, are shared with other systems. If more than one path
is specified, they must be separated with colons.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-optflags">**optflags**</span>

Default value: (architecture-specific)

The **optflags** entry is used to define a standard set of options that
can be used during the build process, specifically during compilation.

The format of the **optflags** entry is slightly different from most
other `rpmrc` file entries. Instead of the usual
<span class="symbol">`<name>`:`<value>`</span> format, the **optflags**
entry looks like this:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>optflags:&lt;architecture&gt; &lt;value&gt;

        </code></pre></td>
</tr>
</tbody>
</table>

For example, assume the following **optflags** entries were placed in an
`rpmrc` file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>optflags: i386 -O2 -m486 -fno-strength-reduce
optflags: sparc -O2

        </code></pre></td>
</tr>
</tbody>
</table>

If RPM was running on an Intel 80386-compatible architecture, the
**optflags** value would be set to **-O2 -m486 -fno-strength-reduce**.
If, however, RPM was running on a Sun SPARC-based system, **optflags**
would be set to **-O2**.

This entry sets the `RPM_OPT_FLAGS` environment variable, which can be
used in the **%prep**, **%build**, and **%install** scripts.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-packager">**packager**</span>

Default value: (none)

The **packager** entry is used to define the name and contact
information for the individual responsible for building the package. The
contact information is traditionally defined in the following format:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>packager:Erik Troan &lt;ewt@redhat.com&gt;

        </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-pgp-name">**pgp\_name**</span>

Default value: (none)

The **pgp\_name** entry is used to define the name of the PGP public key
that will be used to sign each package built. The value is not case
sensitive, but the key name entered here must match the actual key name
in every other aspect.

For more information on signing packages with PGP, please read [Chapter
17](ch-rpm-pgp.html).

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-pgp-path">**pgp\_path**</span>

Default value: (none)

The **pgp\_path** entry is used to point to a directory containing PGP
keyring files. These files will be searched for the public key specified
by the **pgp\_name** entry.

For more information on signing packages with PGP, please read [Chapter
17](ch-rpm-pgp.html).

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-require-distribution">**require\_distribution**</span>

Default value: <span class="token">0</span>

The **require\_distribution** entry is used to direct RPM to require
that every package built must contain distribution information. The
default value directs RPM to not enforce this requirement. If the entry
has a non-zero value, RPM will only build packages that define a
distribution.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-require-icon">**require\_icon**</span>

Default value: <span class="token">0</span>

The **require\_icon** entry is used to direct RPM to require that every
package built must contain an icon. The default value directs RPM to not
enforce this requirement. If the entry has a non-zero value, RPM will
only build packages that contain an icon.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-require-vendor">**require\_vendor**</span>

Default value: <span class="token">0</span>

The **require\_vendor** entry is used to direct RPM to require that
every package built must contain vendor information. The default value
directs RPM to not enforce this requirement. If the entry has a non-zero
value, RPM will only build packages that define a vendor.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-rpmdir">**rpmdir**</span>

Default value: `<topdir>`/RPMS

The **rpmdir** entry is used to define the path to the directory in
which RPM will write binary package files. Its default value is taken
from the value of the **topdir** entry, with "`/RPMS`" appended to it.
Note that if you redefine **rpmdir**, you'll need to specify a complete
path. RPM will automatically add an architecture-specific directory to
the end of the path. For example, on an Intel-based system, the actual
path would be:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>/usr/src/redhat/RPMS/i386

        </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-signature">**signature**</span>

Default value: (none)

The **signature** entry is used to define the type of signature that is
to be added to each package built. At the present time, only signatures
from PGP are supported. Therefore, the only acceptable value is
"<span class="token">pgp</span>".

For more information on signing packages with PGP, please read [Chapter
17](ch-rpm-pgp.html).

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-sourcedir">**sourcedir**</span>

Default value: `<topdir>`/SOURCES

The **sourcedir** entry is used to define the path to the directory in
which RPM will look for sources. Its default value is taken from the
value of the **topdir** entry, with "`/SOURCES`" appended to it. Note
that if you redefine **sourcedir**, you'll need to specify a complete
path.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-specdir">**specdir**</span>

Default value: `<topdir>`/SPECS

The **specdir** entry is used to define the path to the directory in
which RPM will look for spec files. Its default value is taken from the
value of the **topdir** entry, with "`/SPECS`" appended to it. Note that
if you redefine **specdir**, you'll need to specify a complete path.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-srcrpmdir">**srcrpmdir**</span>

Default value: `<topdir>`/SRPMS

The **srcrpmdir** entry is used to define the path to the directory in
which RPM will write source package files. Its default value is taken
from the value of the **topdir** entry, with "`/SRPMS`" appended to it.
Note that if you redefine **srcrpmdir**, you'll need to specify a
complete path.

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-timecheck">**timecheck**</span>

Default value: (none)

The **timecheck** entry is used to define the default number of seconds
to apply to the **--timecheck** option when building packages. For more
information on the **--timecheck** option, please see [the Section
called ***--timecheck `<secs>`** — Print a warning if files to be
packaged are over **`<secs>`** old* in Chapter
12](ch-rpm-b-command.html#s2-rpm-b-command-timecheck-option).

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-tmppath">**tmppath**</span>

Default value: `/var/tmp`

The **tmpdir** entry is used to define a path to the directory that RPM
will use for temporary work space. This normally consists of temporary
scripts that are used during the build process. It should be set to an
absolute path (ie, starting with "`/`").

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-topdir">**topdir**</span>

Default value: `/usr/src/redhat`

The **topdir** entry is used to define the path to the top-level
directory in RPM's build directory tree. It should be set to an absolute
path (ie, starting with "`/`"). The following entries base their default
values on the value of **topdir**:

  - **builddir**

  - **rpmdir**

  - **sourcedir**

  - **specdir**

  - **srcrpmdir**

</div>

<div class="sect2">

## <span id="s2-rpmrc-file-vendor">**vendor**</span>

Default value: (none)

The **vendor** entry is used to define the name of the organization that
is responsible for distributing the packaged software. Normally, this
would be the name of a business or other such entity.

</div>

</div>

### Notes

|                                                                                       |                                                                                                                                                                                |
| ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [<span class="footnote">\[1\]</span>](s1-rpmrc-file-rpmrc-file-entries.html#AEN14754) | The **buildostranslate** `rpmrc` file entry is discussed on [the Section called ***buildostranslate***](s1-rpmrc-file-rpmrc-file-entries.html#s2-rpmrc-file-buildostranslate). |

<div class="NAVFOOTER">

-----

|                                              |                          |                               |
| :------------------------------------------- | :----------------------: | ----------------------------: |
| [Prev](s1-rpmrc-file-rpmrc-file-syntax.html) |    [Home](index.html)    |  [Next](ch-rpm-commands.html) |
| `rpmrc` File Syntax                          | [Up](ch-rpmrc-file.html) | Concise RPM Command Reference |

</div>
