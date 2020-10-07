<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-commands-information-options.html)

Appendix C. Concise RPM Command Reference

[Next](s1-rpm-commands-verify-mode.html)

-----

<div class="sect1">

# <span id="s1-rpm-commands-query-mode">Query Mode</span>

RPM's query mode is used to display information about packages:

Format: **rpm --query `<options>`**

or

Format: **rpm -q `<options>`**

<div class="sect2">

## <span id="s2-rpm-commands-package-specification">Package Specification Options To Query Mode</span>

No more than one of the following options may be present in every query
command. They are used to select the source of the information to be
displayed.

  - **`<packagename>`** — Query the named package.

  - **-a** — Query all packages.

  - **-f `<file>`+** — Query package owning **`<file>`**.

  - **-g `<group>`+** — Query packages with group **`<group>`**.

  - **-p `<packagefile>`+** — Query (uninstalled) package
    **`<packagefile>`**.

  - **--whatprovides `<i>`** — Query packages that provide **`<i>`**
    capability.

  - **--whatrequires `<i>`** — Query packages that require **`<i>`**
    capability.

</div>

<div class="sect2">

## <span id="s2-rpm-commands-information-selection">Information Selection Options To Query Mode</span>

One or more of the following options may be added to any query command.
They are used to select what information RPM will display. If no
information selection option is present on the command line, RPM will
simply display the applicable package label(s):

  - **-i** — Display package information.

  - **-l** — Display package file list.

  - **-s** — Show file states (implies **-l**).

  - **-d** — List only documentation files (implies **-l**).

  - **-c** — List only configuration files (implies **-l**).

  - **--dump** — Show all available information for each file (must be
    used with **-l**, **-c**, or **-d**).

  - **--provides** — List capabilities that the package provides.

  - **--requires**, **-R** — List capabilities that the package
    requires.

  - **--scripts** — Print the various \[un\]install, verification
    scripts.

  - **--queryformat `<s>`** — Use **`<s>`** as the header format
    (implies **-i**).

  - **--qf `<s>`** — Shorthand for **--queryformat**.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                                  |                            |                                          |
| :----------------------------------------------- | :------------------------: | ---------------------------------------: |
| [Prev](s1-rpm-commands-information-options.html) |     [Home](index.html)     | [Next](s1-rpm-commands-verify-mode.html) |
| Informational Options                            | [Up](ch-rpm-commands.html) |                              Verify Mode |

</div>
