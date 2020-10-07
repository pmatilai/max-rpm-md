<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](p14028.md)

[Next](s1-rpm-file-format-rpm-file-format.md)

-----

<div class="appendix">

# <span id="ch-rpm-file-format"></span>Appendix A. Format of the RPM File

<div class="sect1">

# <span id="s1-rpm-file-format-file-naming-convention">RPM File Naming Convention</span>

While RPM will run just as well if a package file has been renamed, when
the packages are created during RPM's build process, they follow a
specific naming convention. The convention is:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>name-version-release.architecture.rpm

      </code></pre></td>
</tr>
</tbody>
</table>

where:

  - <span class="symbol">`name`</span> is a name describing the packaged
    software.

  - <span class="symbol">`version`</span> is the version of the packaged
    software.

  - <span class="symbol">`release`</span> is the number of times this
    version of the software has been packaged.

  - <span class="symbol">`architecture`</span> is a shorthand name
    describing the type of computer hardware the packaged software is
    meant to run on. It may also be the string `src`, or `nosrc`. Both
    of these strings indicate the file is an RPM source package. The
    `nosrc` string means that the file contains only package building
    files, while the `src` string means the file contains the necessary
    package building files *and* the software's source code.

A few notes are in order. Normally, the package name is taken verbatim
from the packaged software's name. Occasionally, this approach won't
work — usually this occurs when the software is split into multiple
"subpackages," each supporting a different set of functions. An example
of this situation would be the way `ncurses` was packaged on Red Hat
Linux Linux. The package incorporating `ncurses` basic functionality was
called `ncurses`, while the package incorporating those parts of
`ncurses`' program development functionality was named `ncurses-devel`.

The version number is normally taken verbatim from the package's
version. The only restriction placed on the version is that it cannot
contain a dash "`-`".

The release can be thought of as the *package's* version. Traditionally
it is a number, starting at 1, that shows how many times the packaged
software, at a given version, has been built. This is tradition and not
a restriction, however. Like the version number, the only restriction is
that dashes are not allowed.

The architecture specifier is a string that indicates what hardware the
package has been built for. There are a number of architectures defined:

  - `i386` — The Intel x86 family of microprocessors, starting with the
    80386.

  - `alpha` — The Digital Alpha/AXP series of microprocessors.

  - `sparc` — Sun Microsystems' SPARC series of chips.

  - `mips` — MIPS Technologies' processors.

  - `ppc` — The Power PC microprocessor family.

  - `m68k` — Motorola's 68000 series of CISC microprocessors.

  - `SGI` — Equivalent to "MIPS".

This list will almost certainly change. For the most up-to-date list,
please refer to the file `/usr/lib/rpmrc`. It contains information used
internally by RPM, including a list of architectures and equivalent code
numbers.

</div>

</div>

<div class="NAVFOOTER">

-----

|                     |                    |                                                 |
| :------------------ | :----------------: | ----------------------------------------------: |
| [Prev](p14028.md) | [Home](index.md) | [Next](s1-rpm-file-format-rpm-file-format.md) |
| Appendixes          | [Up](p14028.md)  |                                 RPM File Format |

</div>
