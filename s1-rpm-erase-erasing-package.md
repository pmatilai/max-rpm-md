<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-erase.md)

Chapter 3. Using RPM to Erase Packages

[Next](s1-rpm-erase-additional-options.md)

-----

<div class="sect1">

# <span id="s1-rpm-erase-erasing-package">Erasing a Package</span>

The most basic erase command is:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -e eject
#
        </code></pre></td>
</tr>
</tbody>
</table>

In this case, the `eject` package was erased. There isn't much in the
way of feedback, is there? Could we get more if we add **-v**?

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -ev eject
#
        </code></pre></td>
</tr>
</tbody>
</table>

Still nothing. However, there's another option that can be counted on to
give a wealth of information. Let's give it a try:

<div class="sect2">

## <span id="s2-rpm-erase-vv-option">Getting More Information With **-vv**</span>

By adding **-vv** to the command line, we can often get a better feel
for what's going on inside RPM. The **-vv** option was really meant for
the RPM developers, and its output may change, but it is a great way to
gain insight into RPM's inner workings. Let's try it with **rpm -e**:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -evv eject
D: uninstalling record number 286040
D: running preuninstall script (if any)
D: removing files test = 0
D: /usr/man/man1/eject.1 - removing
D: /usr/bin/eject - removing
D: running postuninstall script (if any)
D: removing database entry
D: removing name index
D: removing group index
D: removing file index for /usr/bin/eject
D: removing file index for /usr/man/man1/eject.1

#
          </code></pre></td>
</tr>
</tbody>
</table>

Although **-v** had no effect on RPM's output, **-vv** gave us a torrent
of output. But what does it tell us?

First, RPM displays the package's record number. The number is normally
of use only to people that work on RPM's database code.

Next, RPM executes a "pre-uninstall" script, if one exists. This script
can execute any commands required to remove the package before any files
are actually deleted.

The "files test = 0" line indicates that RPM is to actually erase the
package. If the number had been non-zero, RPM would only be performing a
test of the package erasure. This happens when the **--test** option is
used. Refer to [the Section called ***--test** — Go Through the Process
of Erasing the Package, But Do Not Erase
It*](s1-rpm-erase-additional-options.md#s2-rpm-erase-test-option) for
more information on the use of the **--test** option with **rpm -e**.

The next two lines log the actual removal of the files comprising the
package. Packages with many files can result in a lot of output when
using **-vv**\!

Next, RPM executes a "post-uninstall" script, if one exists. Like the
pre-uninstall script, this script is used to perform any processing
required to cleanly erase the package. Unlike the pre-uninstall script,
however, the post-uninstall script runs *after* all the package's files
have been removed.

Finally, the last five lines show the process RPM uses to remove every
trace of the package from its database. From the messages, we can see
that the database contains some per-package data, followed by
information on every file installed by the package.

</div>

</div>

<div class="NAVFOOTER">

-----

|                             |                         |                                              |
| :-------------------------- | :---------------------: | -------------------------------------------: |
| [Prev](ch-rpm-erase.md)   |   [Home](index.md)    | [Next](s1-rpm-erase-additional-options.md) |
| Using RPM to Erase Packages | [Up](ch-rpm-erase.md) |                           Additional Options |

</div>
