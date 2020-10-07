<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-multi-optflags.md)

Chapter 19. Building Packages for Multiple Architectures and Operating
Systems

[Next](s1-rpm-multi-platform-dependent-conditional.md)

-----

<div class="sect1">

# <span id="s1-rpm-multi-platform-dependent-tags">Platform-Dependent Tags</span>

Once RPM has determined the build platform's information, that
information can be used in the build process. The first way this
information can be used is to determine whether a given package should
be built on a given platform. This is done through the use of four tags
that can be added to a spec file.

There can be many reasons to do this. For example, the software may not
build correctly on a given platform. Or the software may be
platform-specific, such that packaging the software on any other
platform, while technologically possible, would really make no sense.

The real world is not always so clear-cut, so there might even be cases
where a package should be built on, say, three different platforms, but
no others. By carefully using the following tags, any conceivable
situation can be covered.

Like the `rpmrc` file entries we've already discussed, there are
identical tags for architecture and operating system, so we'll discuss
them together.

<div class="sect2">

## <span id="s2-rpm-multi-excludexxx-tag">The **exclude`xxx`** Tag</span>

The **exclude`xxx`** tags are used to direct RPM to insure that the
package does *not* attempt to build on the excluded platforms. One or
more platforms may be specified after the **exclude`xxx`** tags,
separated by either spaces or commas. Here are two examples:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>ExcludeArch: sparc alpha
ExcludeOS: Irix

          </code></pre></td>
</tr>
</tbody>
</table>

The first line prevents systems based on the Sun SPARC and Digital
Alpha/AXP architectures from attempting to build the package. The second
line insures that the package will not be built for the Silicon Graphics
operating system, Irix.

If a build is attempted on an excluded architecture or operating system,
the following message will be displayed, and the build will fail:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba cdplayer-1.0.spec
Arch mismatch!
cdplayer-1.0.spec doesn&#39;t build on this architecture

# 
          </code></pre></td>
</tr>
</tbody>
</table>

The **exclude`xxx`** tags are meant to explicitly prevent a finite set
of architectures or operating systems from building a package. If your
goal is to insure that a package will only build on *one* architecture,
then you should use the **exclusive`xxx`** tags.

</div>

<div class="sect2">

## <span id="s2-rpm-multi-exclusivexxx-tag">The **exclusive`xxx`** Tag</span>

The **exclusive`xxx`** tags are used to direct RPM to only build the
package on the specified platforms. These tags insure that, in the
future, no brand-new platform will mistakenly attempt to build the
package. RPM will build the package on the specified platforms only.

The syntax of the **exclusive`xxx`** tags is identical to
**exclude`xxx`**:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>ExclusiveArch: sparc alpha
ExclusiveOS: Irix

          </code></pre></td>
</tr>
</tbody>
</table>

In the first line, the package will only build on a Sun SPARC or Digital
Alpha/AXP system. In the second, the package will only be built on the
Irix operating system.

The **exclusive`xxx`** tags are meant to explicitly allow a finite set
of architectures or operating systems to build a package. If your goal
is to insure that a package will *not* build on a specific platform,
then you should use the **exclude`xxx`** tag.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                             |                         |                                                          |
| :------------------------------------------ | :---------------------: | -------------------------------------------------------: |
| [Prev](s1-rpm-multi-optflags.md)          |   [Home](index.md)    | [Next](s1-rpm-multi-platform-dependent-conditional.md) |
| **optflags** — The Other `rpmrc` File Entry | [Up](ch-rpm-multi.md) |                          Platform-Dependent Conditionals |

</div>
