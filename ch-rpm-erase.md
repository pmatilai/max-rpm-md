<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-install-additional-options.html)

[Next](s1-rpm-erase-erasing-package.html)

-----

<div class="chapter">

# <span id="ch-rpm-erase"></span>Chapter 3. Using RPM to Erase Packages

<div class="table">

<span id="tb-rpm-erase-syntax"></span>

**Table 3-1. **rpm -e** Command Syntax**

**rpm -e** (or **--erase**) `options` `pkg1` … `pkgN`

</div>

</div>

Parameters

`pkg1` … `pkgN`

One or more installed packages

Erase-specific Options

Page

**--test**

Perform erase tests only

[the Section called ***--test** — Go Through the Process of Erasing the
Package, But Do Not Erase
It*](s1-rpm-erase-additional-options.html#s2-rpm-erase-test-option)

**--noscripts**

Do not execute pre- and post-uninstall scripts

[the Section called ***--noscripts** — Do *Not* Execute Pre- and
Post-uninstall
Scripts*](s1-rpm-erase-additional-options.html#s2-rpm-erase-noscripts-option)

**--nodeps**

Do not check dependencies

[the Section called ***--nodeps**: Do Not Check Dependencies Before
Erasing
Package*](s1-rpm-erase-additional-options.html#s2-rpm-erase-nodeps-option)

General Options

Page

**-vv**

Display debugging information

[the Section called *Getting More Information With
**-vv***](s1-rpm-erase-erasing-package.html#s2-rpm-erase-vv-option)

**--root `<path>`**

Set alternate root to **`<path>`**

[the Section called ***--root `<path>`** — Use **`<path>`** As the
Root*](s1-rpm-erase-additional-options.html#s2-rpm-erase-root-option)

**--rcfile `<rcfile>`**

Set alternate rpmrc file to **`<rcfile>`**

[the Section called ***--rcfile `<rcfile>`** — Read **`<rcfile>`** For
RPM
Defaults*](s1-rpm-erase-additional-options.html#s2-rpm-erase-rcfile-option)

**--dbpath `<path>`**

Use **`<path>`** to find the RPM database

[the Section called ***--dbpath `<path>`**: Use **`<path>`** To Find RPM
Database*](s1-rpm-erase-additional-options.html#s2-rpm-erase-dbpath-option)

<div class="sect1">

# <span id="s1-rpm-erase-what-does-it-do">**rpm -e** — What Does it Do?</span>

The **rpm -e** command (**--erase** is equivalent) removes, or erases,
one or more packages from the system. RPM performs a series of steps
whenever it erases a package:

  - It checks the RPM database to make sure that no other packages
    depend on the package being erased.

  - It executes a pre-uninstall script (if one exists).

  - It checks to see if any of the package's config files have been
    modified. If so, it saves copies of them.

  - It reviews the RPM database to find every file listed as being part
    of the package, and if they do not belong to another package,
    deletes them.

  - It executes a post-uninstall script (if one exists).

  - It removes all traces of the package (and the files belonging to it)
    from the RPM database.

That's quite a bit of activity for a single command. No wonder RPM can
be such a time-saver\!

</div>

<div class="NAVFOOTER">

-----

|                                                |                    |                                           |
| :--------------------------------------------- | :----------------: | ----------------------------------------: |
| [Prev](s1-rpm-install-additional-options.html) | [Home](index.html) | [Next](s1-rpm-erase-erasing-package.html) |
| Additional options to **rpm -i**               |  [Up](p108.html)   |                         Erasing a Package |

</div>
