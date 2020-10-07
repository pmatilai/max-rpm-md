<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-specref-macros.html)

Appendix E. Concise Spec File Reference

[Next](s1-rpm-specref-files-list-directives.html)

-----

<div class="sect1">

# <span id="s1-rpm-specref-files-list">The **%files** List</span>

The **%files** list indicates which files on the build system are to be
packaged. The list consists of one file per line. If a directory is
specified, by default all files and subdirectories will be packaged.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%files
/etc/foo.conf
/sbin/foo
/usr/bin/foocmd

      </code></pre></td>
</tr>
</tbody>
</table>

The **%files** list can be made specific to a particular subpackage by
adding the subpackage name, and optionally, the `-n` option:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%files bar

%files -n bar

      </code></pre></td>
</tr>
</tbody>
</table>

The subpackage name and usage of the `-n` option must match those
defined with the **%package** directive.

The **%files** list can also use the contents of a file as the list of
files to be packaged. This is done by using the `-f` option, which is
then followed by a filename:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%files -f files.list

      </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%files** List* in Chapter
13](s1-rpm-inside-files-list.html).

</div>

<div class="NAVFOOTER">

-----

|                                    |                           |                                                   |
| :--------------------------------- | :-----------------------: | ------------------------------------------------: |
| [Prev](s1-rpm-specref-macros.html) |    [Home](index.html)     | [Next](s1-rpm-specref-files-list-directives.html) |
| Macros                             | [Up](ch-rpm-specref.html) |                Directives For the **%files** list |

</div>
