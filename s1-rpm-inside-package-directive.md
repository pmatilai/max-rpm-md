<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-inside-files-list-directives.html)

Chapter 13. Inside the Spec File

[Next](s1-rpm-inside-conditionals.html)

-----

<div class="sect1">

# <span id="s1-rpm-inside-package-directive">The Lone Directive: **%package**</span>

While every directive we've seen so far is used in the **%files** list,
the **%package** directive is different. It is used to permit the
creation of more than one package per spec file and can appear at any
point in the spec file. These additional packages are known as
subpackages. Subpackages are named according to the contents of the line
containing the **%package** directive. The format of the package
directive is:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%package: &lt;string&gt;

        </code></pre></td>
</tr>
</tbody>
</table>

The **\<string\>** should be a name that describes the subpackage. This
string is appended to the base package name to produce the subpackage's
name. For example, if a spec file contains a **name** tag value of
"**foonly**", and a "**%package doc**" line, then the subpackage name
will be `foonly-doc`.

<div class="sect2">

## <span id="s2-rpm-inside-package-n-option">**-n `<string>`** — Use **`<string>`** As the Entire Subpackage Name</span>

As we mentioned above, the name of a subpackage normally includes the
main package name. When the **-n** option is added to the **%package**
directive, it directs RPM to use the name specified on the **%package**
line as the entire package name. In the example above, the following
**%package** line would create a subpackage named `foonly-doc`:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%package doc

          </code></pre></td>
</tr>
</tbody>
</table>

The following **%package** line would create a subpackage named `doc`:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%package -n doc

          </code></pre></td>
</tr>
</tbody>
</table>

The **%package** directive plays another role in subpackage building.
That role is to act as a place to collect tags that are specific to a
given subpackage. Any tag placed after a **%package** directive will
only apply to that subpackage.

Finally, the name string specified by the **%package** directive is also
used to denote which parts of the spec file are a part of that
subpackage. This is done by including the string (along with the **-n**
option, if present on the **%package** line) on the starting line of the
section that is to be subpackage-specific. Here's an example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>…
%package -n bar
…
%post -n bar
…

          </code></pre></td>
</tr>
</tbody>
</table>

In this heavily edited spec file segment, a subpackage called `bar` has
been defined. Later in the file is a post-install script. Because it has
subpackage `bar`'s name on the **%post** line, the post-install script
will be part of the `bar` subpackage only.

For more information on building subpackages, please see [Chapter
18](ch-rpm-subpack.html).

</div>

</div>

<div class="NAVFOOTER">

-----

|                                                  |                          |                                         |
| :----------------------------------------------- | :----------------------: | --------------------------------------: |
| [Prev](s1-rpm-inside-files-list-directives.html) |    [Home](index.html)    | [Next](s1-rpm-inside-conditionals.html) |
| Directives For the **%files** list               | [Up](ch-rpm-inside.html) |                            Conditionals |

</div>
