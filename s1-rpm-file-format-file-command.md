<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-file-format-rpm-tools.html)

Appendix A. Format of the RPM File

[Next](ch-rpmrc-file.html)

-----

<div class="sect1">

# <span id="s1-rpm-file-format-file-command">Identifying RPM files with the **file(1)** command</span>

The `magic` file on most UNIX-like systems today should have the
necessary information to identify RPM files. But in case your system
doesn't, the following information can be added to the file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#-------------------------------------------------------------------
#
# RPM: file(1) magic for Red Hat Packages
#
0       beshort         0xedab          
&gt;2      beshort         0xeedb          RPM
&gt;&gt;4     byte            x               v%d
&gt;&gt;6     beshort         0               bin
&gt;&gt;6     beshort         1               src
&gt;&gt;8     beshort         1               i386
&gt;&gt;8     beshort         2               Alpha
&gt;&gt;8     beshort         3               Sparc
&gt;&gt;8     beshort         4               MIPS
&gt;&gt;8     beshort         5               PowerPC
&gt;&gt;8     beshort         6               68000
&gt;&gt;8     beshort         7               SGI
&gt;&gt;10    string          x               %s

      </code></pre></td>
</tr>
</tbody>
</table>

The output of the **file** command is succinct:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># file baz
baz: RPM v3 bin i386 vlock-1.0-2

#
      </code></pre></td>
</tr>
</tbody>
</table>

In this case, the file called `baz` is a version 3 format RPM file
containing release 2 of version 1.0 of the `vlock` package, which has
been built for the Intel x86 architecture.

</div>

<div class="NAVFOOTER">

-----

|                                           |                               |                            |
| :---------------------------------------- | :---------------------------: | -------------------------: |
| [Prev](s1-rpm-file-format-rpm-tools.html) |      [Home](index.html)       | [Next](ch-rpmrc-file.html) |
| Tools For Studying RPM Files              | [Up](ch-rpm-file-format.html) |           The `rpmrc` File |

</div>
