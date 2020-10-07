<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-depend.md)

Chapter 14. Adding Dependency Information to a Package

[Next](s1-rpm-depend-manual-dependencies.md)

-----

<div class="sect1">

# <span id="s1-rpm-depend-auto-depend">Automatic Dependencies</span>

When a package is built by RPM, if any file in the package's **%files**
list is a shared library, the library's *soname* is automatically added
to the list of capabilities the package provides. The soname is the name
used to determine compatibility between different versions of a library.

Note that this is *not* a filename. In fact, no aspect of RPM's
dependency processing is based on filenames. Many people new to RPM
often make the assumption that a failed dependency represents a missing
file. This is not the case.

Remember that RPM's dependency processing is based on knowing what
capabilities are provided by a package and what capabilities a package
requires. We've seen how RPM automatically determines what shared
library resources a package provides. But does it automatically
determine what shared libraries a package *requires*?

Yes\! RPM does this by running **ldd** on every executable program in a
package's **%files** list. Since **ldd** provides a list of the shared
libraries each program requires, both halves of the equation are
complete — that is, the packages that make shared libraries available,
*and* the packages that require those shared libraries, are tracked by
RPM. RPM can then take that information into account when packages are
installed, upgraded, or erased.

<div class="sect2">

## <span id="s2-rpm-depend-auto-depend-scripts">The Automatic Dependency Scripts</span>

RPM uses two scripts to handle automatic dependency processing. They
reside in `/usr/bin` and are called `find-requires`, and
`find-provides`. We'll take a look at them in a minute, but first let's
look at why there are scripts to do this sort of thing. Wouldn't it be
better to have this built into RPM itself?

Actually, creating scripts for this sort of thing *is* a better idea.
The reason? RPM has already been ported to a variety of different
operating systems. Determining what shared libraries an executable
requires, and the soname of shared libraries, is simple, but the exact
steps required vary widely from one operating system to another. Putting
this part of RPM into a script makes it easier to port RPM.

Let's take a look at the scripts that are used by RPM under the Linux
operating system.

<div class="sect3">

### <span id="s3-rpm-depend-find-requires">`find-requires` — Automatically Determine Shared Library Requirements</span>

The `find-requires` script for Linux is quite simple:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#!/bin/sh

# note this works for both a.out and ELF executables

ulimit -c 0

filelist=`xargs -r file | fgrep executable | cut -d: -f1 `

for f in $filelist; do
    ldd $f | awk &#39;/=&gt;/ { print $1 }&#39;
done | sort -u | xargs -r -n 1 basename | sort -u

            </code></pre></td>
</tr>
</tbody>
</table>

This script first creates a list of executable files. Then, for each
file in the list, **ldd** determines the file's shared library
requirements, producing a list of sonames. Finally, the list of sonames
is sanitized by removing duplicates, and removing any paths.

</div>

<div class="sect3">

### <span id="s3-rpm-depend-find-provides">`find-provides` — Automatically Determine Shared Library Sonames</span>

The `find-provides` script for Linux is a bit more complex, but still
pretty straightforward:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#!/bin/bash

# This script reads filenames from STDIN and outputs any relevant
# provides information that needs to be included in the package.

filelist=$(grep &quot;.so&quot; | grep -v &quot;^/lib/ld.so&quot; | 
xargs file -L 2&gt;/dev/null | grep &quot;ELF.*shared object&quot; | cut -d: -f1)

for f in $filelist; do
    soname=$(objdump -p $f | awk &#39;/SONAME/ {print $2}&#39;)

    if [ &quot;$soname&quot; != &quot;&quot; ]; then
        if [ ! -L $f ]; then
            echo $soname
        fi
    else
        echo ${f##*/}
    fi
done | sort -u

            </code></pre></td>
</tr>
</tbody>
</table>

First, a list of shared libraries is created. Then, for each file on the
list, the soname is extracted, cleaned up, and duplicates removed.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-depend-auto-depend-example">Automatic Dependencies: An Example</span>

Let's take a widely used program, **ls**, the directory lister, as an
example. On a Red Hat Linux system, **ls** is part of the `fileutils`
package and is installed in `/bin`. Let's play the part of RPM during
`fileutils`' package build and run `find-requires` on `/bin/ls`. Here's
what we'll see:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># find-requires
/bin/ls
&lt;ctrl-d&gt;
libc.so.5

#
          </code></pre></td>
</tr>
</tbody>
</table>

The `find-requires` script returned `libc.so.5`. Therefore, RPM should
add a requirement for `libc.so.5` when the `fileutils` package is built.
We can verify that RPM did add **ls**' requirement for `libc.so.5` by
using RPM's **--requires** option to display `fileutils`' requirements:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --requires fileutils
libc.so.5

#
          </code></pre></td>
</tr>
</tbody>
</table>

OK, that's the first half of the equation — RPM automatically detecting
a package's shared library requirements. Now let's look at the second
half of the equation -- RPM detecting packages that provide shared
libraries. Since the `libc` package includes, among others, the shared
library `/lib/libc.so.5.3.12`, RPM would obtain its soname. We can
simulate this by using `find-provides` to print out the library's
soname:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># find-provides
/lib/libc.so.5.3.12
Ctrl-D
libc.so.5

#
          </code></pre></td>
</tr>
</tbody>
</table>

OK, so `/lib/libc.so.5.3.12`'s soname is `libc.so.5`. Let's see if the
`libc` package really does "provide" the `libc.so.5` soname:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --provides libc
libm.so.5
libc.so.5

#
          </code></pre></td>
</tr>
</tbody>
</table>

Yes, there it is, along with the soname of another library contained in
the package. In this way, RPM can ensure that any package requiring
`libc.so.5` will have a compatible library available as long as the
`libc` package, which provides `libc.so.5`, is installed.

In most cases, automatic dependencies are enough to fill the bill.
However, there are circumstances when the package builder has to
manually add dependency information to a package. Fortunately, RPM's
approach to manual dependencies is both simple and flexible.

</div>

<div class="sect2">

## <span id="s2-rpm-depend-autoreqprov">The **autoreqprov**, **autoreq**, and **autoprov** Tags — Disable Automatic Dependency Processing</span>

There may be times when RPM's automatic dependency processing is not
desired. In these cases, the **autoreqprov**, **autoreq**, and
**autoprov** tags may be used to disable it. This tag takes a **yes/no**
or **0/1** value. For example, to disable automatic dependency
processing, the following line may be used:

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

The **autoreq** and **autoprov** tags can be used to disable automatic
processing of requirements or "provides" only, respectively.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                            |                          |                                                |
| :----------------------------------------- | :----------------------: | ---------------------------------------------: |
| [Prev](ch-rpm-depend.md)                 |    [Home](index.md)    | [Next](s1-rpm-depend-manual-dependencies.md) |
| Adding Dependency Information to a Package | [Up](ch-rpm-depend.md) |                            Manual Dependencies |

</div>
