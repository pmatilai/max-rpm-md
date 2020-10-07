<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-specref-package.md)

Appendix E. Concise Spec File Reference

[Next](ch-rpm-resources.md)

-----

<div class="sect1">

# <span id="s1-rpm-specref-conditionals">Conditionals</span>

The **%if`xxx`** conditionals are used to begin a section of the spec
file that is specific to a particular architecture or operating system.
They are followed by one or more architecture or operating system
specifiers, each separated by commas or whitespace.

Conditionals may be nested within other conditionals, provided that the
inner conditional is completely enclosed by the outer conditional.

<div class="sect2">

## <span id="s2-rpm-specref-ifarch">The **%ifarch** Conditional</span>

If the build system's architecture is specified, the part of the spec
file following the **%ifarch**, but before a **%else** or **%endif**
will be used during the build.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%ifarch i386 sparc

        </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%ifarch** Conditional* in Chapter
13](s1-rpm-inside-conditionals.md#s2-rpm-inside-ifarch-conditional).

</div>

<div class="sect2">

## <span id="s2-rpm-specref-ifnarch">The **%ifnarch** Conditional</span>

If the build system's architecture is specified, the part of the spec
file following the **%ifarch** but before a **%else** or **%endif** will
*not* be used during the build.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%ifnarch i386 sparc

        </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%ifnarch** Conditional* in Chapter
13](s1-rpm-inside-conditionals.md#s2-rpm-inside-ifnarch-conditional).

</div>

<div class="sect2">

## <span id="s2-rpm-specref-ifos">The **%ifos** Conditional</span>

If the build system is running one of the specified operating systems,
the part of the spec file following the **%ifos** but before a **%else**
or **%endif** will be used during the build.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%ifos linux

        </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%ifos** Conditional* in Chapter
13](s1-rpm-inside-conditionals.md#s2-rpm-inside-ifos-conditional).

</div>

<div class="sect2">

## <span id="s2-rpm-specref-ifnos">The **%ifnos** Conditional</span>

If the build system is running one of the specified operating systems,
the part of the spec file following the **%ifnos** but before a
**%else** or **%endif** will *not* be used during the build.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%ifnos linux

        </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%ifnos** Conditional* in Chapter
13](s1-rpm-inside-conditionals.md#s2-rpm-inside-ifnos-conditional).

</div>

<div class="sect2">

## <span id="s2-rpm-specref-else">The **%else** Conditional</span>

The **%else** conditional is placed between a **%if** conditional of
some persuasion, and an **%endif**. It is used to create two blocks of
spec file statements, only one of which will be used in any given case.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%ifarch alpha
make RPM_OPT_FLAGS=&quot;$RPM_OPT_FLAGS -I .&quot;
%else
make RPM_OPT_FLAGS=&quot;$RPM_OPT_FLAGS&quot;
%endif

        </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%else** Conditional* in Chapter
13](s1-rpm-inside-conditionals.md#s2-rpm-inside-else-conditional).

</div>

<div class="sect2">

## <span id="s2-rpm-specref-endif">The **%endif** Conditional</span>

An **%endif** is used to end a conditional block of spec file
statements. The **%endif** is always needed after a conditional,
otherwise, the build will fail.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%ifarch i386
make INTELFLAG=-DINTEL
%endif

        </code></pre></td>
</tr>
</tbody>
</table>

See also: [the Section called *The **%endif** Conditional* in Chapter
13](s1-rpm-inside-conditionals.md#s2-rpm-inside-endif-conditional).

</div>

</div>

<div class="NAVFOOTER">

-----

|                                     |                           |                               |
| :---------------------------------- | :-----------------------: | ----------------------------: |
| [Prev](s1-rpm-specref-package.md) |    [Home](index.md)     | [Next](ch-rpm-resources.md) |
| **%package** Directive              | [Up](ch-rpm-specref.md) |         RPM-related Resources |

</div>
