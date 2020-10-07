<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-depend-summary.html)

[Next](s1-rpm-reloc-prefix-tag.html)

-----

<div class="chapter">

# <span id="ch-rpm-reloc"></span>Chapter 15. Making a Relocatable Package

RPM has the ability to give users some latitude in deciding where
packages are to be installed on their systems. However, package builders
must first design their packages to give users this freedom.

That's all well and good, but why would the ability to "relocate" a
package be all that important?

<div class="sect1">

# <span id="s1-rpm-reloc-why">Why relocatable packages?</span>

One of the many problems that plague a system administrator's life is
disk space. Usually, there's not enough of it, and if there *is* enough,
chances are it's in the wrong place. Here's a hypothetical example:

  - Some new software comes out and is desired greatly by the user
    community.

  - The system administrator carefully reviews the software's
    installation documentation prior to doing to the installation.
    [<span class="footnote">\[1\]</span>](#FTN.AEN9725) She notes that
    the software, all 150MB of it, installs into `/opt`.

  - Frowning, the sysadmin fires off a quick **df** command:
    
    <table>
    <colgroup>
    <col style="width: 100%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td><pre class="screen"><code># df
    Filesystem         1024-blocks  Used Available Capacity Mounted on
    /dev/sda0             100118   28434    66514     30%   /
    /dev/sda6             991995  365527   575218     39%   /usr
    
    # 
                  </code></pre></td>
    </tr>
    </tbody>
    </table>
    
    Bottom line: There's no way 150MB of new software is going to fit on
    the root filesystem.
    [<span class="footnote">\[2\]</span>](#FTN.AEN9737)

  - Sighing heavily, the sysadmin ponders what to do next. If only there
    were some way to install the software somewhere on the `/usr`
    filesystem…

It doesn't have to be this way. RPM has the ability to make packages
that can be installed with a user-specified prefix that dictates where
the software will actually be placed. By making packages relocatable,
the package builder can make life easier for sysadmins everywhere. But
what exactly *is* a relocatable package?

A relocatable package is a package that is standard in every way, save
one. The difference lies in the **prefix** tag. When this tag is added
to a spec file, RPM will attempt to build a relocatable package.

Note the word "attempt". There are a few conditions that must be met
before a relocatable package can be built successfully, and this chapter
will cover them all. But first, let's look at exactly how RPM can
relocate a package. And the one component at the heart of package
relocation is the **prefix** tag.

</div>

</div>

### Notes

|                                                                  |                                                                                                                                                                                                                                                |
| ---------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](ch-rpm-reloc.html#AEN9725) | Hey, we said it was hypothetical\!                                                                                                                                                                                                             |
| [<span class="footnote">\[2\]</span>](ch-rpm-reloc.html#AEN9737) | Of course, it would be possible to get around this lack of space by symlinking `/opt` to, for instance, `/usr/opt`. However, since the point of this chapter is to explore RPM's relocatability features, we won't explore this approach here. |

<div class="NAVFOOTER">

-----

|                                    |                    |                                        |
| :--------------------------------- | :----------------: | -------------------------------------: |
| [Prev](s1-rpm-depend-summary.html) | [Home](index.html) |   [Next](s1-rpm-reloc-prefix-tag.html) |
| To Summarize…                      |  [Up](p5206.html)  | The **prefix** tag: Relocation Central |

</div>
