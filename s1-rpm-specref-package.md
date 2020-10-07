<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-specref-files-list-directives.html)

Appendix E. Concise Spec File Reference

[Next](s1-rpm-specref-conditionals.html)

-----

<div class="sect1">

# <span id="s1-rpm-specref-package">**%package** Directive</span>

The **%package** directive is used to control the creation of
subpackages. The subpackage name is derived from the first **Name:** tag
in the spec file, followed by the name specified after the **%package**
directive. Therefore, if the first **Name:** tag is:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Name: foo

      </code></pre></td>
</tr>
</tbody>
</table>

and a subpackage is defined with the following **%package** directive:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%package bar

      </code></pre></td>
</tr>
</tbody>
</table>

the subpackage name will be `foo-bar`.

See also: [the Section called *The Lone Directive: **%package*** in
Chapter 13](s1-rpm-inside-package-directive.html).

<div class="sect2">

## <span id="s2-rpm-specref-package-n-option">The **%package -n** Option</span>

The `-n` option is used to change how RPM derives the subpackage name.
When the `-n` option is used, the name following the **%package**
directive becomes the complete subpackage name. Therefore, if a
subpackage is defined with the following **%package** directive:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%package -n bar

        </code></pre></td>
</tr>
</tbody>
</table>

the subpackage name will be `bar`.

See also: [the Section called ***-n `<string>`** — Use **`<string>`** As
the Entire Subpackage Name* in Chapter
13](s1-rpm-inside-package-directive.html#s2-rpm-inside-package-n-option).

</div>

</div>

<div class="NAVFOOTER">

-----

|                                                   |                           |                                          |
| :------------------------------------------------ | :-----------------------: | ---------------------------------------: |
| [Prev](s1-rpm-specref-files-list-directives.html) |    [Home](index.html)     | [Next](s1-rpm-specref-conditionals.html) |
| Directives For the **%files** list                | [Up](ch-rpm-specref.html) |                             Conditionals |

</div>
