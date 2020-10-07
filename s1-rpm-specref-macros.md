<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-specref-scripts.md)

Appendix E. Concise Spec File Reference

[Next](s1-rpm-specref-files-list.md)

-----

<div class="sect1">

# <span id="s1-rpm-specref-macros">Macros</span>

This section describes the various macros used by RPM.

<div class="sect2">

## <span id="s2-rpm-specref-setup">The **%setup** Macro</span>

The **%setup** macro is used to unpack the original sources in
preparation for the build. It is used in the **%prep** script:

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

See also: [the Section called *The **%setup** Macro* in Chapter
13](s1-rpm-inside-macros.md#s2-rpm-inside-setup-macro).

<div class="sect3">

### <span id="s3-rpm-specref-setup-n-option">The `-n <name>` Option</span>

The `-n` option is used to set the name of the software's build
directory. This is necessary only when the source archive unpacks into a
directory named other than **`<name>`- `<version>`**.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%setup -n cd-player

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called ***-n `<name>`** — Set Name of Build
Directory* in Chapter
13](s1-rpm-inside-macros.md#s3-rpm-inside-setup-n-option).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-setup-q-option">The `-q` Option</span>

The `-q` option is used to direct **%setup** to quiet its output.
Verbose file listings won't be displayed when unpacking archives with
this option.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%setup -c

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called ***-c** — Create Directory (and change to
it) Before Unpacking* in Chapter
13](s1-rpm-inside-macros.md#s3-rpm-inside-setup-c-option).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-setup-c-option">The `-c` Option</span>

The `-c` option is used to direct **%setup** to create the top-level
build directory before unpacking the sources.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%setup -c

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called ***-c** — Create Directory (and change to
it) Before Unpacking* in Chapter
13](s1-rpm-inside-macros.md#s3-rpm-inside-setup-c-option).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-setup-d-option">The `-D` Option</span>

The `-D` option is used to direct **%setup** to not delete the build
directory prior to unpacking the sources. This option is used when more
than one source archive is to be unpacked into the build directory,
normally with the `-b` or `-a` options.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%setup -D -T -b 3

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called ***-D** — Do Not Delete Directory Before
Unpacking Sources* in Chapter
13](s1-rpm-inside-macros.md#s3-rpm-inside-setup-d-option).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-setup-t-option">The `-T` Option</span>

The `-T` option is used to direct **%setup** to not perform the default
unpacking of the source archive specified by the first **Source:** tag.
It is used with the `-a` or `-b` options.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%setup -D -T -a 1

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called ***-T** — Do Not Perform Default Archive
Unpacking* in Chapter
13](s1-rpm-inside-macros.md#s3-rpm-inside-setup-t-option).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-setup-b-option">The `-b <n>` Option</span>

The `-b` option is used to direct **%setup** to unpack the source
archive specified on the *n*th **Source:** tag line before changing
directory into the build directory.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%setup -D -T -b 2

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called ***-b `<n>`** — Unpack The *n*th Sources
Before Changing Directory* in Chapter
13](s1-rpm-inside-macros.md#s3-rpm-inside-setup-b-option).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-setup-a-option">The `-a <n>` Option</span>

The `-a` option is used to direct **%setup** to unpack the source
archive specified on the *n*th **Source:** tag line after changing
directory into the build directory.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%setup -D -T -a 5

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called ***-a `<n>`** — Unpack The *n*th Sources
After Changing Directory* in Chapter
13](s1-rpm-inside-macros.md#s3-rpm-inside-setup-a-option).

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-specref-patch-macro">The **%patch** Macro</span>

The **%patch** macro, as its name implies, is used to apply patches to
the unpacked sources. With no additional options specified, it will
apply the patch file specified by the **Patch:** (or **Patch0:**) tag.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%patch

        </code></pre></td>
</tr>
</tbody>
</table>

When there is more than one **Patch:** tag line in a spec file, they can
be specified by appending the number of the **Patch:** tag to the
**%patch** macro name itself.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%patch2

        </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%patch** Macro* in Chapter
13](s1-rpm-inside-macros.md#s2-rpm-inside-patch-macro).

<div class="sect3">

### <span id="s3-rpm-specref-patch-p-option">The `-P <n>` Option</span>

The `-P` option is another method of applying a specific patch. The
number from the **Patch:** tag follows the `-P` option. The following
**%patch** macros both apply the patch specified on the **Patch2:** tag
line:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%patch -P 2

%patch2

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *Specifying Which **patch** Tag to Use* in
Chapter 13](s1-rpm-inside-macros.md#s3-rpm-inside-which-patch-tag).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-patch-p-option2">The `-p<#>` Option</span>

The `-p` option is sent directly to the **patch** command. It is
followed by a number which specifies the number of leading slashes (and
the directories in between) to strip from any filenames present in the
patch file.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%patch -p2

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called ***-p `<#>`** — Strip **`<#>`** leading
slashes and directories from patch filenames* in Chapter
13](s1-rpm-inside-macros.md#s3-rpm-inside-patch-p-option).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-patch-b-option">The `-b <name>` Option</span>

When the **patch** command is used to apply a patch, unmodified copies
of the files patched are renamed to end with the extension `.orig`. The
`-b` option is used to change the extension used by **patch**.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%patch -b .fsstnd

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called ***-b `<name>`** — Set the backup file
extension to **`<name>`*** in Chapter
13](s1-rpm-inside-macros.md#s3-rpm-inside-b-option).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-patch-e-option">The **%patch -E** Option</span>

The `-E` option is sent directly to the **patch** command. It is used to
direct **patch** to remove any empty files after the patches have been
applied.

See also: [the Section called ***-E** — Remove Empty Output Files* in
Chapter 13](s1-rpm-inside-macros.md#s3-rpm-inside-e-option).

</div>

</div>

</div>

<div class="NAVFOOTER">

-----

|                                     |                           |                                        |
| :---------------------------------- | :-----------------------: | -------------------------------------: |
| [Prev](s1-rpm-specref-scripts.md) |    [Home](index.md)     | [Next](s1-rpm-specref-files-list.md) |
| Scriptlets                          | [Up](ch-rpm-specref.md) |                    The **%files** List |

</div>
