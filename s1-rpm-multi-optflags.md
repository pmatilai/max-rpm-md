<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-multi-build-install-detection.md)

Chapter 19. Building Packages for Multiple Architectures and Operating
Systems

[Next](s1-rpm-multi-platform-dependent-tags.md)

-----

<div class="sect1">

# <span id="s1-rpm-multi-optflags">**optflags** — The Other `rpmrc` File Entry</span>

While the **optflags** entry doesn't play a part in determining the
build or install platform, it *does* play a role in multi-platform
package building. The **optflags** entry is used to define a standard
set of options that can be used during the build process, specifically
during compilation.

The **optflags** entry looks like this:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>optflags: &lt;architecture&gt; &lt;value&gt;
        </code></pre></td>
</tr>
</tbody>
</table>

For example, assume the following **optflags** entries were placed in an
`rpmrc` file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>optflags: i386 -O2 -m486 -fno-strength-reduce
optflags: sparc -O2

        </code></pre></td>
</tr>
</tbody>
</table>

If RPM was running on an Intel 80386-compatible architecture, the
**optflags** value would be set to **-O2 -m486 -fno-strength-reduce**.
If, however, RPM was running on a Sun SPARC-based system, **optflags**
would be set to **-O2**.

This entry sets the `RPM_OPT_FLAGS` environment variable, which can be
used in the **%prep**, **%build**, and **%install** scripts.

</div>

<div class="NAVFOOTER">

-----

|                                                   |                         |                                                   |
| :------------------------------------------------ | :---------------------: | ------------------------------------------------: |
| [Prev](s1-rpm-multi-build-install-detection.md) |   [Home](index.md)    | [Next](s1-rpm-multi-platform-dependent-tags.md) |
| Build and Install Platform Detection              | [Up](ch-rpm-multi.md) |                           Platform-Dependent Tags |

</div>
