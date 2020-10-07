<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-upgrade.html)

Chapter 4. Using RPM to Upgrade Packages

[Next](s1-rpm-upgrade-nearly-identical.html)

-----

<div class="sect1">

# <span id="s1-rpm-upgrade-upgrading-a-package">Upgrading a Package</span>

The most basic version of the **rpm -U** command is simply "**rpm -U**",
followed by the name of a `.rpm` package file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -U eject-1.2-2.i386.rpm
#
        </code></pre></td>
</tr>
</tbody>
</table>

Here, RPM performed all the steps necessary to upgrade the `eject-1.2-2`
package, faster than could have been done by hand. As in RPM's install
command, Uniform Resource Locators, or URLs, can also be used to specify
the package file. [<span class="footnote">\[1\]</span>](#FTN.AEN2248)

<div class="sect2">

## <span id="s2-rpm-upgrade-secret">**rpm -U**'s Dirty Little Secret</span>

Well, in the example above, we didn't tell the whole story. There was no
older version of the `eject` package installed. Yes, it's true — **rpm
-U** works just fine as a replacement for the normal install command
**rpm -i**.

This is another, more concrete example of the strength of RPM's method
of performing upgrades. Since RPM's install command is smart enough to
handle upgrades, RPM's upgrade command is really just another way to
specify an install. Some people never even bother to use RPM's install
command; they always use **rpm -U**. Maybe the "**-U**" should stand
for, "Uh, do the right thing"…

</div>

</div>

### Notes

|                                                                                        |                                                                                                                                                                                                    |
| -------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-upgrade-upgrading-a-package.html#AEN2248) | For more information on RPM's use of URLs, please see [the Section called *URLs — Another Way to Specify Package Files* in Chapter 2](s1-rpm-install-performing-install.html#s2-rpm-install-urls). |

<div class="NAVFOOTER">

-----

|                               |                           |                                              |
| :---------------------------- | :-----------------------: | -------------------------------------------: |
| [Prev](ch-rpm-upgrade.html)   |    [Home](index.html)     | [Next](s1-rpm-upgrade-nearly-identical.html) |
| Using RPM to Upgrade Packages | [Up](ch-rpm-upgrade.html) |                    They're Nearly Identical… |

</div>
