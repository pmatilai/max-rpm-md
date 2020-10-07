<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-commands-erase-mode.html)

Appendix C. Concise RPM Command Reference

[Next](s1-rpm-commands-rebuild-mode.html)

-----

<div class="sect1">

# <span id="s1-rpm-commands-build-mode">Build Mode</span>

RPM's build mode is used to build packages:

Format: **rpmbuild** **-b`<stage>` `<options>` `<specfile>`**

(Note that **-vv** is the default for all build mode commands.)

<div class="sect2">

## <span id="s2-rpm-commands-build-mode-stages">Build Mode Stages</span>

One of the following stages must follow the **-b** option:

  - **p** — Prep (unpack sources and apply patches).

  - **l** — List check (do some cursory checks on **%files**).

  - **c** — Compile (prep and compile).

  - **i** — Install (prep, compile, install).

  - **b** — Binary package (prep, compile, install, package).

  - **a** — Binary/source package (prep, compile, install, package).

</div>

<div class="sect2">

## <span id="s2-rpm-commands-build-options">Options To Build Mode</span>

The following options can be used on any build command:

  - **--short-circuit** — Skip straight to specified stage (only for
    **c** and **i**).

  - **--clean** — Remove build tree when done.

  - **--sign** — Generate PGP signature.

  - **--buildroot `<s>`** — Use **`<s>`** as the build root.

  - **--buildarch `<s>`** — Use **`<s>`** as the build architecture.

  - **--buildos `<s>`** — Use **`<s>`** as the build operating system.

  - **--test** — Do not execute any stages.

  - **--timecheck `<s>`** — Set the time check to **`<s>`** seconds (0
    disables it).

</div>

</div>

<div class="NAVFOOTER">

-----

|                                         |                            |                                           |
| :-------------------------------------- | :------------------------: | ----------------------------------------: |
| [Prev](s1-rpm-commands-erase-mode.html) |     [Home](index.html)     | [Next](s1-rpm-commands-rebuild-mode.html) |
| Erase Mode                              | [Up](ch-rpm-commands.html) |                              Rebuild Mode |

</div>
