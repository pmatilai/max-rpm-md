<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-multi-platform-dependent-tags.html)

Chapter 19. Building Packages for Multiple Architectures and Operating
Systems

[Next](s1-rpm-multi-hints-and-kinks.html)

-----

<div class="sect1">

# <span id="s1-rpm-multi-platform-dependent-conditional">Platform-Dependent Conditionals</span>

Of course, the control exerted by the **exclude`xxx`** and
**exclusive`xxx`** tags over package building is often too coarse. There
may be packages, for example, that would build just fine on another
platform, if only you could substitute a platform-specific patch file or
change some paths in the **%files** list.

The key to exerting this kind of platform-specific control in the spec
file is to use RPM's conditionals. The conditionals provide a
general-purpose means of constructing a platform-specific version of the
spec file during the actual build process.

<div class="sect2">

## <span id="s2-rpm-multi-conditionals-common-features">Common Features of All Conditionals</span>

There are a few things that are common to each conditional, so let's
discuss them first. The first thing is that conditionals are
block-structured. The second is that conditionals can be nested.
Finally, conditionals can span any part of the spec file.

<div class="sect3">

### <span id="s3-rpm-multi-conditionals-block-structured">Conditionals Are Block Structured</span>

Every conditional is block-structured — in other words, the conditional
begins at a certain point within the spec file and continues some number
of lines until it is ended. This forms a block that will be used or
ignored, depending on the platform the conditional is checking for, as
well as the build platform itself.

Every conditional starts with a line beginning with the characters
**%if** and is followed by one of four platform-related conditions.
Every conditional ends with a line containing the characters **%endif**.

Ignoring the platform-related conditions for a moment, here's an example
of a conditional block:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%ifos Linux
Summary: This is a package for the Linux operating system
%endif

            </code></pre></td>
</tr>
</tbody>
</table>

It's a one-line block, but a block nonetheless.

There's also another style of conditional block. As before, it starts
with a **%if**, and ends with a **%endif**. But there's something new in
the middle:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%ifos Linux
Summary: This is a package for the Linux operating system
%else
Summary: This is a package for some other operating system
%endif

            </code></pre></td>
</tr>
</tbody>
</table>

Here we've replaced one **summary** tag with another.

</div>

<div class="sect3">

### <span id="s3-rpm-multi-nested-conditionals">Conditionals Can Be Nested</span>

Conditionals can be nested — That is, the block formed by one
conditional can enclose another conditional. Here's an example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%ifarch i386

echo &quot;This is an i386&quot;

%ifos Linux
echo &quot;This is a Linux system&quot;
%else
echo &quot;This is not a Linux system&quot;
%endif

%else

echo &quot;This is not an i386&quot;

%endif

            </code></pre></td>
</tr>
</tbody>
</table>

In this example, the first conditional block formed by the **%ifarch
i386** line contains a complete **%ifos — %else — %endif** conditional.
Therefore, if the build system was Intel-based, the **%ifos**
conditional would be tested. If the build system was *not* Intel-based,
the **%ifos** conditional would not be tested.

</div>

<div class="sect3">

### <span id="s3-rpm-multi-conditionals-cross">Conditionals Can Cross Spec File Sections</span>

The next thing each conditional has in common is that there is no limit
to the number of lines a conditional block can contain. You could
enclose the entire spec file within a conditional, if you like. But it's
much better to use conditionals to insert only the appropriate
platform-specific contents.

Now that we have the basics out of the way, let's take a look at each of
the conditionals and see how they work.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-multi-ifxxx-conditional">**%if`xxx`**</span>

The **%if`xxx`** conditionals are used to control the inclusion of a
block, as long as the platform-dependent information is true. Here are
two examples:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%ifarch i386 alpha

          </code></pre></td>
</tr>
</tbody>
</table>

In this case, the block following the conditional would be included only
if the build architecture was **i386** or **alpha**.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%ifos Linux

          </code></pre></td>
</tr>
</tbody>
</table>

This example would include the block following the conditional only if
the operating system was **Linux**.

</div>

<div class="sect2">

## <span id="s2-rpm-multi-ifnxxx-conditional">**%ifn`xxx`**</span>

The **%ifn`xxx`** conditionals are used to control the inclusion of a
block, as long as the platform-dependent information is *not* true. Here
are two examples:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%ifnarch i386 alpha

          </code></pre></td>
</tr>
</tbody>
</table>

In this case, the block following the conditional would be included only
if the build architecture was *not* **i386** or **alpha**.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%ifnos Linux

          </code></pre></td>
</tr>
</tbody>
</table>

This example would include the block following the conditional only if
the operating system was *not* **Linux**.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                                   |                         |                                           |
| :------------------------------------------------ | :---------------------: | ----------------------------------------: |
| [Prev](s1-rpm-multi-platform-dependent-tags.html) |   [Home](index.html)    | [Next](s1-rpm-multi-hints-and-kinks.html) |
| Platform-Dependent Tags                           | [Up](ch-rpm-multi.html) |                           Hints and Kinks |

</div>
