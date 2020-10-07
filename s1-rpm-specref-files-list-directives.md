<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-specref-files-list.html)

Appendix E. Concise Spec File Reference

[Next](s1-rpm-specref-package.html)

-----

<div class="sect1">

# <span id="s1-rpm-specref-files-list-directives">Directives For the **%files** list</span>

This section lists the various directives used in the **%files** lists.

<div class="sect2">

## <span id="s2-rpm-specref-file-related-directives">File-related Directives</span>

This section lists those directives that are related to files.

<div class="sect3">

### <span id="s3-rpm-specref-doc">The **%doc** Directive</span>

The **%doc** directive flags the filename(s) that follow as being
documentation.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%doc README

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%doc** Directive* in Chapter
13](s1-rpm-inside-files-list-directives.html#s3-rpm-inside-flist-doc-directive).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-config">The **%config** Directive</span>

The **%config** directive is used to flag the specified file as being a
configuration file.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%config /etc/fstab

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%config** Directive* in Chapter
13](s1-rpm-inside-files-list-directives.html#s3-rpm-inside-flist-config-directive).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-attr">The **%attr** Directive</span>

The **%attr** directive is used to permit RPM to directly control a
file's permissions and ownership. It is normally used when non-root
users build packages. The **%attr** directive has the following format:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%attr(&lt;mode&gt;, &lt;user&gt;, &lt;group&gt;) file

          </code></pre></td>
</tr>
</tbody>
</table>

The user and group identifiers must be non-numeric. Attributes that do
not need to be set by **%attr** may be replaced with a dash:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%attr(755, root, -) foo.bar

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%attr** Directive* in Chapter
13](s1-rpm-inside-files-list-directives.html#s3-rpm-inside-flist-attr-directive).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-defattr">The **%attr** Directive</span>

The **%defattr** sets default **%attr** for RPM.

The **%defattr** directive has the following format:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%attr(&lt;file mode&gt;, &lt;user&gt;, &lt;group&gt;, &lt;dir mode&gt;)

          </code></pre></td>
</tr>
</tbody>
</table>

The user and group identifiers must be non-numeric. Attributes that do
not need to be set by **%defattr** may be replaced with a dash.
Directory mode may be ommited:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%defattr(644, root, root, -)

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%defattr** Directive* in Chapter
13](s1-rpm-inside-files-list-directives.html#s3-rpm-inside-flist-defattr-directive).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-verify">The **%verify** Directive</span>

The **%verify** directive is used to control which of nine different
file attributes are to be verified by RPM. The attributes are:

1.  **owner** — The file's owner.

2.  **group** — The file's group.

3.  **mode** — The file's mode.

4.  **md5** — The file's MD5 checksum.

5.  **size** — The file's size.

6.  **maj** — The file's major number.

7.  **min** — The file's minor number.

8.  **symlink** — The file's symbolic link string.

9.  **mtime** — The file's modification time.

If the keyword **not** precedes the list, every attribute *except* those
listed will be verified.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%verify(mode md5 size maj min symlink mtime) /dev/ttyS0

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%verify** Directive* in Chapter
13](s1-rpm-inside-files-list-directives.html#s3-rpm-inside-flist-verify-directive).

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-specref-directory-related-directives">Directory-related Directives</span>

<div class="sect3">

### <span id="s3-rpm-specref-docdir">The **%docdir** Directive</span>

The **%docdir** directive is used to add the specified directory to
RPM's internal list of directories containing documentation. When a
directory is added to this list, every file packaged in this directory
(and any subdirectories) will automatically be marked as documentation.

See also: [the Section called *The **%docdir** Directive* in Chapter
13](s1-rpm-inside-files-list-directives.html#s3-rpm-inside-docdir-directive).

</div>

<div class="sect3">

### <span id="s3-rpm-specref-dir">The **%dir** Directive</span>

The **%dir** directive is used to direct RPM to package only the
directory itself, regardless of what files may reside in the directory
at the time the package is created.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%dir /usr/blather

          </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%dir** Directive* in Chapter
13](s1-rpm-inside-files-list-directives.html#s3-rpm-inside-dir-directive).

</div>

</div>

</div>

<div class="NAVFOOTER">

-----

|                                        |                           |                                     |
| :------------------------------------- | :-----------------------: | ----------------------------------: |
| [Prev](s1-rpm-specref-files-list.html) |    [Home](index.html)     | [Next](s1-rpm-specref-package.html) |
| The **%files** List                    | [Up](ch-rpm-specref.html) |              **%package** Directive |

</div>
