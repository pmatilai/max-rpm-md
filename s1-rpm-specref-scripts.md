<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-specref-preamble.md)

Appendix E. Concise Spec File Reference

[Next](s1-rpm-specref-macros.md)

-----

<div class="sect1">

# <span id="s1-rpm-specref-scripts">Scriptlets</span>

This section lists the various scriptlets found in a spec file.

<div class="sect2">

## <span id="s2-rpm-specref-build-time-scripts">Build Scriptlets</span>

Every build scriptlet has the following environment variables defined:

  - `RPM_SOURCE_DIR`

  - `RPM_BUILD_DIR`

  - `RPM_DOC_DIR`

  - `RPM_OPT_FLAGS`

  - `RPM_ARCH`

  - `RPM_OS`

  - `RPM_ROOT_DIR`

  - `RPM_BUILD_ROOT`

  - `RPM_PACKAGE_NAME`

  - `RPM_PACKAGE_VERSION`

  - `RPM_PACKAGE_RELEASE`

For more information on these environment variables, and build
scriptlets in general, please see [the Section called *Build-time
Scripts* in Chapter
13](s1-rpm-inside-scripts.md#s2-rpm-inside-build-time-scripts).

<div class="sect3">

### <span id="s3-rpm-specref-prep">**%prep** Directive -- Unpack archives and apply patches.</span>

The **%prep** scriptlet is executed first during a build, The scriptlet
normally prepares the contents of a source package for building, usually
by unpacking archives and applying patches. The scriptlet can contain
any valid **sh** commands.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%prep

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%prep** Script* in Chapter
13](s1-rpm-inside-scripts.md#s3-rpm-inside-prep-script).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-build">**%build** Directive -- Configure and compile components to be packaged.</span>

The **%build** scriptlet is the second scriptlet executed during a
build, immediately after **%prep**. The scriptlet normally builds the
components to be included in a binary package, usually by configuring
and compiling source code from the previously unpacked and patched
archives. The scriptlet can contain any valid **sh** commands.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%build

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%build** Script* in Chapter
13](s1-rpm-inside-scripts.md#s3-rpm-inside-build-script).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-install">**%install** Directive -- Install components to be packaged.</span>

The **%install** scriptlet is the third scriptlet executed during a
build, immediately after **%build**. The scriptlet normally installs
components to be included in a binary package, usually by copying files
from the build directory tree to an install directory tree. The
scriptlet can contain any valid **sh** commands.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%install

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%install** Script* in Chapter
13](s1-rpm-inside-scripts.md#s3-rpm-inside-install-script).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-check">**%check** Directive -- Run included tests.</span>

The **%check** scriptlet is the fourth scriptlet executed during a
build, immediately after **%install**. The scriptlet normally runs the
test suite for the built components if one is available. The scriptlet
can contain any valid **sh** commands.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%check

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%check** Script* in Chapter
13](s1-rpm-inside-scripts.md#s3-rpm-inside-check-script).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-clean">**%clean** Directive -- Remove build components.</span>

The **%clean** scriptlet is executed at the end of a build. The
scriptlet cleans up files produced during a build, usually by removing
the install directory tree. The scriptlet can contain any valid **sh**
commands.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%clean

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%clean** Script* in Chapter
13](s1-rpm-inside-scripts.md#s3-rpm-inside-clean-script).

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-specref-install-erase-scripts">Install/Erase Scriptlets</span>

These scriptlets are executed whenever the package is installed or
erased. Each scriptlet can contain any valid **sh** commands.

Note: Each of the following scriptlet can be made specific to a
particular subpackage by adding the subpackage name, and optionally, the
`-n` option:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%post bar

%preun -n bar

        </code></pre></td>
</tr>
</tbody>
</table>

The subpackage name and usage of the `-n` option must match those
defined with the **%package** directive.

Each scriptlet has the following environment variable defined:

  - `RPM_INSTALL_PREFIX`

For more information on this environment variable please see [the
Section called *Install/Erase-time Scripts* in Chapter
13](s1-rpm-inside-scripts.md#s2-rpm-inside-erase-time-scripts).

<div class="sect3">

### <span id="s3-rpm-specref-pre">The **%pre** Script</span>

The **%pre** scriptlet executes just before the package is to be
installed.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%pre

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%pre** Script* in Chapter
13](s1-rpm-inside-scripts.md#s3-rpm-inside-pre-script).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-post">The **%post** Script</span>

The **%post** scriptlet executes just after the package is to be
installed.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%post

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%post** Script* in Chapter
13](s1-rpm-inside-scripts.md#s4-rpm-inside-post-script).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-preun">The **%preun** Script</span>

The **%preun** scriptlet executes just before the package is to be
erased.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%preun

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%preun** Script* in Chapter
13](s1-rpm-inside-scripts.md#s3-rpm-inside-preun-script).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-postun">**%postun** Directive</span>

The **%postun** scriptlet executes just after the package is to be
erased.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%postun

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%postun** Script* in Chapter
13](s1-rpm-inside-scripts.md#s3-rpm-inside-postun-script).

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-specref-verification-script">**%verifyscript** Directive</span>

This section describes the verification script.

<div class="sect3">

### <span id="s3-rpm-specref-verifyscript">The **%verifyscript** Script</span>

The **%verifyscript** scriptlet executes whenever the package is
verified using RPM's `-V` option. The scriptlet can contain any valid
**sh** commands.

See also: [the Section called *Verification-Time Script — The
**%verifyscript** Script* in Chapter
13](s1-rpm-inside-scripts.md#s2-rpm-inside-verifyscript-script).

</div>

</div>

</div>

<div class="NAVFOOTER">

-----

|                                      |                           |                                    |
| :----------------------------------- | :-----------------------: | ---------------------------------: |
| [Prev](s1-rpm-specref-preamble.md) |    [Home](index.md)     | [Next](s1-rpm-specref-macros.md) |
| The Preamble                         | [Up](ch-rpm-specref.md) |                             Macros |

</div>
