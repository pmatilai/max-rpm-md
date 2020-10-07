<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-queryformat-tags.md)

[Next](s1-rpm-specref-preamble.md)

-----

<div class="appendix">

# <span id="ch-rpm-specref"></span>Appendix E. Concise Spec File Reference

<div class="sect1">

# <span id="s1-rpm-specref-comments">Comments</span>

Comments are a way to make RPM ignore a line in the spec file. To create
a comment, enter an octothorp (`#`) at the start of the line. Any text
following the comment character will be ignored by RPM:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># This is the spec file for playmidi 2.3...

      </code></pre></td>
</tr>
</tbody>
</table>

Comments can be placed in any section of the spec file. Note that macros
are expanded everywhere, so that with multiline macros which would only
have the first line commented also escape the percent (**%**) character:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># %%configure

      </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *Comments: Notes Ignored by RPM* in
Chapter 13](ch-rpm-inside.md#s1-rpm-inside-comments).

</div>

</div>

<div class="NAVFOOTER">

-----

|                                      |                    |                                      |
| :----------------------------------- | :----------------: | -----------------------------------: |
| [Prev](ch-queryformat-tags.md)     | [Home](index.md) | [Next](s1-rpm-specref-preamble.md) |
| Available Tags For **--queryformat** | [Up](p14028.md)  |                         The Preamble |

</div>
