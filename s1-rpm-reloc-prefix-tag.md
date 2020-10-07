<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-reloc.html)

Chapter 15. Making a Relocatable Package

[Next](s1-rpm-reloc-wrinkles.html)

-----

<div class="sect1">

# <span id="s1-rpm-reloc-prefix-tag">The **prefix** tag: Relocation Central</span>

The best way to explain how the **prefix** tag is used is to step
through an example. Here's a sample **prefix** tag:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Prefix: /opt

        </code></pre></td>
</tr>
</tbody>
</table>

In this example, the prefix path is defined as `/opt`. This means that,
by default, the package will install its files under `/opt`. Let's
assume the spec file contains the following line in its **%files** list:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>/opt/bin/baz

        </code></pre></td>
</tr>
</tbody>
</table>

If the package is installed without any relocation, this file will be
installed in `/opt/bin`. This is identical to how a non-relocatable
package is installed.

However, if the package *is* to be relocated on installation, the path
of every file in the **%files** list is modified according to the
following steps:

1.  The part of the file's path that corresponds to the path specified
    on the **prefix** tag line is removed.

2.  The user-specified relocation prefix is prepended to the file's
    path.

Using our `/opt/bin/baz` file as an example, let's assume that the user
installing the package wishes to override the default prefix (`/opt`),
with a new prefix, say, `/usr/local/opt`. Following the steps above, we
first remove the original prefix from the file's path:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>/opt/bin/baz

        </code></pre></td>
</tr>
</tbody>
</table>

becomes:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>/bin/baz

        </code></pre></td>
</tr>
</tbody>
</table>

Next, we add the user-specified prefix to the front of the remaining
part of the filename:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>/usr/local/opt + /bin/baz = /usr/local/opt/bin/baz

        </code></pre></td>
</tr>
</tbody>
</table>

Now that the file's new path has been created, RPM installs the file
normally. This part of it seems simple enough, and it is. But as we
mentioned above, there are a few things the package builder needs to
consider before getting on the relocatable package bandwagon.

</div>

<div class="NAVFOOTER">

-----

|                              |                         |                                          |
| :--------------------------- | :---------------------: | ---------------------------------------: |
| [Prev](ch-rpm-reloc.html)    |   [Home](index.html)    |       [Next](s1-rpm-reloc-wrinkles.html) |
| Making a Relocatable Package | [Up](ch-rpm-reloc.html) | Relocatable Wrinkles: Things to Consider |

</div>
