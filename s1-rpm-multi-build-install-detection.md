<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-multi-multi-platform-easier.md)

Chapter 19. Building Packages for Multiple Architectures and Operating
Systems

[Next](s1-rpm-multi-optflags.md)

-----

<div class="sect1">

# <span id="s1-rpm-multi-build-install-detection">Build and Install Platform Detection</span>

As we mentioned above, the first step to multi-platform package building
is to identify the build platform. This is done by matching information
from the build system's **uname** output against a number of `rpmrc`
file entries.

Normally, it's not necessary to worry too much about the following
`rpmrc` file entries, as RPM comes with a set of entries that support
all platforms that currently run RPM. However, when adding support for
new platforms, it will be necessary to use the following entries to add
support for the new build platform.

<div class="sect2">

## <span id="s2-rpm-multi-platform-rpmrc-entries">Platform-Specific `rpmrc` Entries</span>

Normally, the file `/usr/lib/rpmrc` contains the following `rpmrc` file
entries. They can be overridden by entries in `/etc/rpmrc` or
`~/.rpmrc`. This is discussed more completely in [Appendix
B](ch-rpmrc-file.md).

Because each entry type is available in both architecture and operating
system flavors, we'll just use **`xxx`** in place of **arch** and **os**
in the following descriptions.

<div class="sect3">

### <span id="s3-rpm-multi-xxx-canon">**`xxx`\_canon** — Define Canonical Platform Name and Number</span>

The **`xxx`\_canon** entry is used to convert information obtained from
the system running RPM into a canonical name and number that RPM will
use internally. Here's the format:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>xxx_canon: &lt;label&gt;: &lt;string&gt; &lt;value&gt;

            </code></pre></td>
</tr>
</tbody>
</table>

The `<label>` is compared against information from **uname(2)**. If a
match is found, then `<string>` is used by RPM as the canonical name,
and `<value>` is used as a unique numeric value. Here are two examples:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>arch_canon: sun4:  sparc  3
os_canon:  Linux:  Linux  1

            </code></pre></td>
</tr>
</tbody>
</table>

The **arch\_canon** tag above is used to define the canonical
architecture information for Sun Microsystems' SPARC architecture. In
this case, the output from **uname** is compared against **sun4**. If
there's a match, the canonical architecture name is set to **sparc** and
the architecture number is set to **3**.

The **os\_canon** tag above is used to define the canonical operating
system information for the Linux operating system. In this case, the
output from **uname** is compared against **Linux**. If there's a match,
the canonical operating system name is set to **Linux**, and the
operating system number is set to **1**.

The description above is not 100% complete — There is an additional step
performed during the time RPM gets the system information from
**uname**, and compares it against a canonical name. Next, let's look at
the `rpmrc` file entry that comes into play during this intermediate
step.

</div>

<div class="sect3">

### <span id="s3-rpm-multi-buildxxxtranslate">**build`xxx`translate** — Define Build Platform</span>

The **build`xxx`translate** entry is used to define the build platform
information. Specifically, these entries are used to create a table that
maps information from **uname** to the appropriate
architecture/operating system name.

The **build`xxx`translate** entry looks like this:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>buildxxxtranslate: &lt;label&gt;: &lt;string&gt;
            </code></pre></td>
</tr>
</tbody>
</table>

The `<label>` is compared against information from **uname(2)**. If a
match is found, then `<string>` is used by RPM as the build platform
information, after it has been canonicalized by **`xxx`\_canon**. Here
are two examples:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>buildarchtranslate: i586: i386
buildostranslate: Linux: Linux

            </code></pre></td>
</tr>
</tbody>
</table>

The **buildarchtranslate** tag shown above is used to define the build
architecture for an Intel Pentium (or **i586** as it's shown here)
processor. Any Pentium-based system will, by default, build packages for
the Intel 80386 (or **i386**) architecture.

The **buildostranslate** tag shown above is used to define the build
operating system for systems running the Linux operating system. In this
case, the build operating system remains unchanged.

</div>

<div class="sect3">

### <span id="s3-rpm-multi-xxx-compat">**`xxx`\_compat** — Define Compatible Architectures</span>

The **`xxx`\_compat** entry is used to define which architectures and
operating systems are compatible with one another. It is used at
install-time only. The format of the entry is:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>xxx_compat: &lt;label&gt;: &lt;list&gt;
            </code></pre></td>
</tr>
</tbody>
</table>

The `<label>` is a name string as defined by an **`xxx`\_canon** entry.
The `<list>` following it consists of one or more names, also defined by
**arch\_canon**. If there is more than one name in the list, they should
be separated by a space.

The names in the list are considered compatible to the name specified in
the label.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>arch_compat: i586: i486
arch_compat: i486: i386
os_compat: Linux: AIX

            </code></pre></td>
</tr>
</tbody>
</table>

The **arch\_compat** lines shown above illustrate how a family of
upwardly compatible architectures may be represented. For example, if
the build architecture was defined as an **i586**, the compatible
architectures would be **i486**, and **i386**. However, if the build
system was an **i486**, the only compatible architecture would be an
**i386**.

While the **os\_compat** line shown above is entirely fictional, its
purpose would be to declare **AIX** compatible with **Linux**. If it
were only that simple…

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-multi-platform-override">Overriding Platform Information At Build-Time</span>

By using the `rpmrc` file entries discussed above, RPM usually makes the
right decisions in selecting the build and install platforms. However,
there might be times when RPM's selections aren't the best. Normally the
circumstances are unusual, as in the case of cross-compiling software.
In these cases, it is nice to have an easy way of overriding the
build-time architecture and operating system.

The **--buildarch** and **--buildos** options can be used to set the
build-time architecture and operating system rather than relying on
RPM's automatic detection capabilities. These options are added to a
normal RPM build command. One important point to remember is that,
although RPM does try to find the specified architecture name, it does
no checking as to the sanity of the entered architecture or operating
system. For example, if you enter an entirely fictional operating
system, RPM will issue a warning message, and then happily build a
package for it.

Why? Wouldn't it make more sense for RPM to perform some sort of sanity
check? In a word, no. One of RPM's main design goals was to never get in
the way of the package builder. If someone has a need to override their
build platform information, they should know what they're doing, and
what the full implications of their actions are.

Bottom line: Unless you *know* why you need to use **--buildarch** or
**--buildos**, you probably don't need to use them.

</div>

<div class="sect2">

## <span id="s2-rpm-multi-platform-override-install-time">Overriding Platform Information At Install-Time</span>

It's also possible to direct RPM to ignore platform information while a
package is being installed. The **--ignorearch** and **--ignoreos**
options, when added to any install or upgrade command, will direct RPM
to proceed with the install or upgrade, even if the package's platform
doesn't match the install platform.

Dangerous? Yes. But it can be indispensable in certain circumstances.
Like the ability to override platform information at build-time, unless
you know why you need to use **--ignorearch** or **--ignoreos**, you
probably don't need to use them.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                                           |                         |                                             |
| :-------------------------------------------------------- | :---------------------: | ------------------------------------------: |
| [Prev](s1-rpm-multi-multi-platform-easier.md)           |   [Home](index.md)    |          [Next](s1-rpm-multi-optflags.md) |
| What Does RPM Do To Make Multi-Platform Packaging Easier? | [Up](ch-rpm-multi.md) | **optflags** — The Other `rpmrc` File Entry |

</div>
