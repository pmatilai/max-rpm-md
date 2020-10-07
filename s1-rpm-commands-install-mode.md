<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-commands-verify-mode.md)

Appendix C. Concise RPM Command Reference

[Next](s1-rpm-commands-upgrade-mode.md)

-----

<div class="sect1">

# <span id="s1-rpm-commands-install-mode">Install Mode</span>

RPM's installation mode is used to install packages:

Format: **rpm --install `<packagefile>`**

or

Format: **rpm -i `<packagefile>`**

<div class="sect2">

## <span id="s2-rpm-commands-install-options">Options To Install Mode</span>

The following options can be used on any install command:

  - **-h**, **--hash** — Print hash marks as package installs (good with
    **-v**).

  - **--prefix `<dir>`** — Relocate the package to **`<dir>`**, if
    relocatable.

  - **--excludedocs** — Do not install documentation.

  - **--force** — Shorthand for **--replacepkgs** and
    **--replacefiles**.

  - **--ignorearch** — Do not verify package architecture.

  - **--ignoreos** — Do not verify package operating system.

  - **--includedocs** — Install documentation.

  - **--nodeps** — Do not check package dependencies.

  - **--noscripts** — Do not execute any installation scripts.

  - **--percent** — Print percentages as package installs.

  - **--replacefiles** — Install even if the package replaces installed
    files.

  - **--replacepkgs** — Reinstall if the package is already present.

  - **--test** — Do not install, but tell if it would work or not.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                          |                            |                                           |
| :--------------------------------------- | :------------------------: | ----------------------------------------: |
| [Prev](s1-rpm-commands-verify-mode.md) |     [Home](index.md)     | [Next](s1-rpm-commands-upgrade-mode.md) |
| Verify Mode                              | [Up](ch-rpm-commands.md) |                              Upgrade Mode |

</div>
