<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-commands-query-mode.html)

Appendix C. Concise RPM Command Reference

[Next](s1-rpm-commands-install-mode.html)

-----

<div class="sect1">

# <span id="s1-rpm-commands-verify-mode">Verify Mode</span>

RPM's verification mode is used to ensure that a package is still
installed properly:

Format: **rpm --verify `<options>`**

or

Format: **rpm -V `<options>`**

or

Format: **rpm -y `<options>`**

<div class="sect2">

## <span id="s2-rpm-commands-verify-options">Options To Verify Mode</span>

The following options can be used on any verify command:

  - **--nodeps** — Do not verify package dependencies.

  - **--nofiles** — Do not verify file attributes.

  - **--noscripts** — Do not execute the package's verification script.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                         |                            |                                           |
| :-------------------------------------- | :------------------------: | ----------------------------------------: |
| [Prev](s1-rpm-commands-query-mode.html) |     [Home](index.html)     | [Next](s1-rpm-commands-install-mode.html) |
| Query Mode                              | [Up](ch-rpm-commands.html) |                              Install Mode |

</div>
