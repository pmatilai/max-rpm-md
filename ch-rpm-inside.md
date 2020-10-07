<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-b-command-other-build-commands.html)

[Next](s1-rpm-inside-tags.html)

-----

<div class="chapter">

# <span id="ch-rpm-inside"></span>Chapter 13. Inside the Spec File

In this chapter, we're going to cover the spec file in detail. There are
a number of different types of entries that comprise a spec file, and
every one will be documented here. The different types of entries are:

  - Comments — Human-readable notes ignored by RPM.

  - Tags — Define data.

  - Scripts — Contain commands to be executed at specific times.

  - Macros — A method of executing multiple commands easily.

  - The **%files** list — A list of files to be included in the package.

  - Directives — Used in the **%files** list to direct RPM to handle
    certain files in a specific way.

  - Conditionals — Permit operating system- or architecture-specific
    preprocessing of the spec file.

Let's start by looking at comments.

<div class="sect1">

# <span id="s1-rpm-inside-comments">Comments: Notes Ignored by RPM</span>

Comments are a way to make RPM ignore a line in the spec file. The
contents of a comment line are entirely up to the person writing the
spec file.

To create a comment, enter an octothorp (**\#**) at the start of the
line. Any text following the comment character will be ignored by RPM.
Here's an example comment:

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
are expanded everywhere, so with multiline macros which would only have
the first line commented also escape the percent (**%**) character:

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

</div>

</div>

<div class="NAVFOOTER">

-----

|                                                    |                    |                                 |
| :------------------------------------------------- | :----------------: | ------------------------------: |
| [Prev](s1-rpm-b-command-other-build-commands.html) | [Home](index.html) | [Next](s1-rpm-inside-tags.html) |
| Other Build-related Commands                       |  [Up](p5206.html)  |          Tags: Data Definitions |

</div>
