<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-commands-build-mode.html)

Appendix C. Concise RPM Command Reference

[Next](s1-rpm-commands-recompile-mode.html)

-----

<div class="sect1">

# <span id="s1-rpm-commands-rebuild-mode">Rebuild Mode</span>

RPM's rebuild mode is used to rebuild packages from a source package
file. The source archives, patches, and icons that comprise the source
package are removed after the binary package is built. Rebuild mode
implies **--clean**.

Format: **rpm --rebuild `<options>` `<source-package>`**

(Note that **-vv** is the default for all rebuild mode commands.)

<div class="sect2">

## <span id="s2-rpm-commands-rebuild-options">Options To Rebuild Mode</span>

Only the global options may be used.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                         |                            |                                             |
| :-------------------------------------- | :------------------------: | ------------------------------------------: |
| [Prev](s1-rpm-commands-build-mode.html) |     [Home](index.html)     | [Next](s1-rpm-commands-recompile-mode.html) |
| Build Mode                              | [Up](ch-rpm-commands.html) |                              Recompile Mode |

</div>
