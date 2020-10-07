<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-commands-install-mode.html)

Appendix C. Concise RPM Command Reference

[Next](s1-rpm-commands-erase-mode.html)

-----

<div class="sect1">

# <span id="s1-rpm-commands-upgrade-mode">Upgrade Mode</span>

RPM's upgrade mode is used to upgrade packages:

Format: **rpm --upgrade `<packagefile>`**

or

Format: **rpm -U `<packagefile>`**

<div class="sect2">

## <span id="s2-rpm-commands-upgrade-options">Options To Upgrade Mode</span>

The following options can be used on any upgrade command:

  - **-h**, **--hash** — Print hash marks as package installs (good with
    **-v**).

  - **--prefix `<dir>`** — Relocate the package to **`<dir>`**, if
    relocatable.

  - **--excludedocs** — Do not install documentation.

  - **--force** — Shorthand for **--replacepkgs**, **--replacefiles**,
    and **--oldpackage**.

  - **--ignorearch** — Do not verify package architecture.

  - **--ignoreos** — Do not verify package operating system.

  - **--includedocs** — Install documentation.

  - **--nodeps** — Do not verify package dependencies.

  - **--noscripts** — Do not execute any installation scripts.

  - **--percent** — Print percentages as package installs.

  - **--replacefiles** — Install even if the package replaces installed
    files.

  - **--replacepkgs** — Reinstall if the package is already present.

  - **--test** — Do not install, but tell if it would work or not.

  - **--oldpackage** — Upgrade to an old version of the package
    (**--force** on upgrades does this automatically).

</div>

</div>

<div class="NAVFOOTER">

-----

|                                           |                            |                                         |
| :---------------------------------------- | :------------------------: | --------------------------------------: |
| [Prev](s1-rpm-commands-install-mode.html) |     [Home](index.html)     | [Next](s1-rpm-commands-erase-mode.html) |
| Install Mode                              | [Up](ch-rpm-commands.html) |                              Erase Mode |

</div>
