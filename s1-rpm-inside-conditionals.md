<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-inside-package-directive.md)

Chapter 13. Inside the Spec File

[Next](ch-rpm-depend.md)

-----

<div class="sect1">

# <span id="s1-rpm-inside-conditionals">Conditionals</span>

While the "exclude" and "exclusive" tags (**excludearch**,
**exclusivearch**, **excludeos**, and **exclusiveos**) provide some
control over whether a package will be built on a given architecture
and/or operating system, that control is still rather coarse.

For example, what should be done if a package will build under multiple
architectures, but requires slightly different **%build** scripts? Or
what if a package requires a certain set of files under one operating
system, and an entirely different set under another operating system?
The architecture and operating system-specific tags we've discussed
earlier in the chapter do nothing to help in such situations. What can
be done?

One approach would be to simply create different spec files for each
architecture or operating system. While it would certainly work, this
approach has some problems:

  - More work. The existence of multiple spec files for a given package
    means that the effort required to make any changes to the package is
    multiplied by however many different spec files there are.

  - More chance for mistakes. If any work needs to be done to the spec
    files, the fact they are separate means it is that much easier to
    forget to make the necessary changes to each one. There is also the
    chance of introducing mistakes each time changes are made.

The other approach is to somehow permit the conditional inclusion of
architecture- or operating system-specific sections of the spec file.
Fortunately, the RPM designers chose this approach, and it makes
multi-platform package building easier and less prone to mistakes.

We discuss multi-platform package building in depth in [Chapter
19](ch-rpm-multi.md). For now, let's take a quick look at RPM's
conditionals.

<div class="sect2">

## <span id="s2-rpm-inside-ifarch-conditional">The **%ifarch** Conditional</span>

The **%ifarch** conditional is used to begin a section of the spec file
that is architecture-specific. It is followed by one or more
architecture specifiers, each separated by commas or whitespace. Here is
an example:

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

The contents of the spec file following this line would be processed
only by Intel x86 or Sun SPARC-based systems. However, if only this line
were placed in a spec file, this is what would happen if a build was
attempted:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba cdplayer-1.0.spec
Unclosed %if
Build failed.

#
          </code></pre></td>
</tr>
</tbody>
</table>

The problem that surfaced here is that any conditional must be "closed"
by using either **%else** or **%endif**. We'll be covering them a bit
later in the chapter.

</div>

<div class="sect2">

## <span id="s2-rpm-inside-ifnarch-conditional">The **%ifnarch** Conditional</span>

The **%ifnarch** conditional is used in a similar fashion to
**%ifarch**, except that the logic is reversed. If a spec file contains
a conditional block starting with **%ifarch alpha**, that block would be
processed only if the build was being done on a Digital Alpha/AXP-based
system. However, if the conditional block started with **%ifnarch
alpha**, then that block would be processed only if the build were *not*
being done on an Alpha.

Like **%ifarch**, **%ifnarch** can be followed by one or more
architectures and must be closed by a **%else** or **%endif**.

</div>

<div class="sect2">

## <span id="s2-rpm-inside-ifos-conditional">The **%ifos** Conditional</span>

The **%ifos** conditional is used to control RPM's spec file processing
based on the build system's operating system. It is followed by one or
more operating system names. A conditional block started with **%ifos**
must be closed by a **%else** or **%endif**. Here's an example:

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

The contents of the spec file following this line would be processed
only if the build was done on a linux system.

</div>

<div class="sect2">

## <span id="s2-rpm-inside-ifnos-conditional">The **%ifnos** Conditional</span>

The **%ifnos** conditional is the logical complement to **%ifos**: that
is, if a conditional starting with the line **%ifnos irix** is present
in a spec file, then the file contents after the **%ifnos** will not be
processed if the build system is running Irix. As always, a conditional
block starting with **%ifnos** must be closed by a **%else** or
**%endif**.

</div>

<div class="sect2">

## <span id="s2-rpm-inside-else-conditional">The **%else** Conditional</span>

The **%else** conditional is placed between a **%if** conditional of
some persuasion, and a **%endif**. It is used to create two blocks of
spec file statements, only one of which will be used in any given case.
Here's an example:

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

When a build is performed on a Digital Alpha/AXP, some additional flags
are added to the **make** command. On all other systems, these flags are
not added.

</div>

<div class="sect2">

## <span id="s2-rpm-inside-endif-conditional">The **%endif** Conditional</span>

A **%endif** is used to end a conditional block of spec file statements.
It can follow one of the **%if** conditionals, or the **%else**. The
**%endif** is always needed after a conditional, otherwise the build
will fail. Here's short conditional block, ending with a **%endif**:

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

In this example, we see the conditional block started with a **%ifarch**
and ended with a **%endif**.

Now that we have some more in-depth knowledge of the spec file, let's
take a look at some of RPM's additional features. In the next chapter,
we'll explore how to add dependency information to a package.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                              |                          |                                            |
| :------------------------------------------- | :----------------------: | -----------------------------------------: |
| [Prev](s1-rpm-inside-package-directive.md) |    [Home](index.md)    |                 [Next](ch-rpm-depend.md) |
| The Lone Directive: **%package**             | [Up](ch-rpm-inside.md) | Adding Dependency Information to a Package |

</div>
