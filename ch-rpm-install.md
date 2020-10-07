<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-intro-to-rpm-lets-get-started.html)

[Next](s1-rpm-install-performing-install.html)

-----

<div class="chapter">

# <span id="ch-rpm-install"></span>Chapter 2. Using RPM to Install Packages

<div class="table">

<span id="tb-rpm-install-syntax"></span>

**Table 2-1. **rpm -i** Command Syntax**

**rpm -i** (or **--install**) `options` `file1.rpm` … `fileN.rpm`

</div>

</div>

Parameters

`file1.rpm` … `fileN.rpm`

One or more RPM package files (URLs OK)

Install-specific Options

Page

**-h** (or **--hash**)

Print hash marks ("\#") during install

[the Section called ***-h**: Perfect for the
Impatient*](s1-rpm-install-handy-options.html#s2-rpm-install-install-h)

**--test**

Perform installation tests only

[the Section called ***--test**: Perform Installation Tests
Only*](s1-rpm-install-additional-options.html#s2-rpm-install-test-option)

**--percent**

Print percentages during install

[the Section called ***--percent**: Not Meant for Human
Consumption*](s1-rpm-install-additional-options.html#s2-rpm-install-percent)

**--excludedocs**

Do not install documentation

[the Section called ***--excludedocs**: Do Not Install Documentation For
This
Package*](s1-rpm-install-additional-options.html#s2-rpm-install-excludedocs-option)

**--includedocs**

Install documentation

[the Section called ***--includedocs**: Install Documentation For This
Package*](s1-rpm-install-additional-options.html#s2-rpm-install-includedocs)

**--replacepkgs**

Replace a package with a new copy of itself

[the Section called ***--replacepkgs**: Install the Package Even If
Already
Installed*](s1-rpm-install-additional-options.html#s2-rpm-install-replacepkgs)

**--replacefiles**

Replace files owned by another package

[the Section called ***--replacefiles**: Install the Package Even If It
Replaces Another Package's
Files*](s1-rpm-install-additional-options.html#s2-rpm-install-replacefiles-option)

**--force**

Ignore package and file conflicts

[the Section called ***--force**: The Big
Hammer*](s1-rpm-install-additional-options.html#s2-rpm-install-force-option)

**--noscripts**

Do not execute pre- and post-install scripts

[the Section called ***--noscripts**: Do Not Execute Pre- and
Post-install
Scripts*](s1-rpm-install-additional-options.html#s2-rpm-install-noscripts)

**--prefix `<path>`**

Relocate package to **`<path>`** if possible

[the Section called ***--prefix `<path>`**: Relocate the package to
**`<path>`**, if
possible*](s1-rpm-install-additional-options.html#s2-rpm-install-prefix)

**--ignorearch**

Do not verify package architecture

[the Section called ***--ignorearch**: Do Not Verify Package
Architecture*](s1-rpm-install-additional-options.html#s2-rpm-install-ignorearch)

**--ignoreos**

Do not verify package operating system

[the Section called ***--ignoreos**: Do Not Verify Package Operating
System*](s1-rpm-install-additional-options.html#s2-rpm-install-ignoreos)

**--nodeps**

Do not check dependencies

[the Section called ***--nodeps**: Do Not Check Dependencies Before
Installing
Package*](s1-rpm-install-additional-options.html#s2-rpm-install-nodeps)

**--ftpproxy `<host>`**

Use **`<host>`** as the FTP proxy

[the Section called ***--ftpproxy `<host>`**: Use **`<host>`** As Proxy
In FTP-based
Installs*](s1-rpm-install-additional-options.html#s2-rpm-install-ftpproxy)

**--ftpport `<port>`**

Use **`<port>`** as the FTP port

[the Section called ***--ftpport `<port>`**: Use **`<port>`** In
FTP-based
Installs*](s1-rpm-install-additional-options.html#s2-rpm-install-ftpport)

General Options

Page

**-v**

Display additional information

[the Section called *Getting a bit more feedback with
**-v***](s1-rpm-install-handy-options.html#s2-rpm-install-more-feedback)

**-vv**

Display debugging information

[the Section called *Getting a *lot* more information with
**-vv***](s1-rpm-install-additional-options.html#s2-rpm-install-vv-option)

**--root `<path>`**

Set alternate root to **`<path>`**

[the Section called ***--root `<path>`**: Use **`<path>`** As An
Alternate
Root*](s1-rpm-install-additional-options.html#s2-rpm-install-root)

**--rcfile `<rcfile>`**

Set alternate rpmrc file to **`<rcfile>`**

[the Section called ***--rcfile `<rcfile>`**: Use **`<rcfile>`** As An
Alternate `rpmrc`
File*](s1-rpm-install-additional-options.html#s2-rpm-install-rcfile)

**--dbpath `<path>`**

Use **`<path>`** to find the RPM database

[the Section called ***--dbpath `<path>`**: Use **`<path>`** To Find RPM
Database*](s1-rpm-install-additional-options.html#s2-rpm-install-dbpath)

<div class="sect1">

# <span id="s1-rpm-install-rpm-i-what-does-it-do">**rpm -i** — What does it do?</span>

Of the many things RPM can do, probably the one that people think of
first is the installation of software. As mentioned earlier, installing
new software is a complex, error-prone job. RPM turns that process into
a single command.

**rpm -i** (**--install** is equivalent) installs software that's been
packaged into an RPM package file. It does this by:

  - Performing dependency checks.

  - Checking for conflicts.

  - Performing any tasks required before the install.

  - Deciding what to do with config files.

  - Unpacking files from the package and putting them in the proper
    place.

  - Performing any tasks required after the install.

  - Keeping track of what it did.

Let's go through each of these steps in a bit more detail.

<div class="sect2">

## <span id="s2-rpm-install-dependency-checks">Performing dependency checks:</span>

Some packages will not operate properly unless some other package is
installed, too. RPM makes sure that the package being installed will
have its dependency requirements met. It will also insure that the
package's installation will not cause dependency problems for other
already-installed packages.

</div>

<div class="sect2">

## <span id="s2-rpm-install-checking-for-conflicts">Checking for conflicts:</span>

RPM performs a number of checks during this phase. These checks look for
things like attempts to install an already installed package, attempts
to install an older package over a newer version, or the possibility
that a file may be overwritten.

</div>

<div class="sect2">

## <span id="s2-rpm-install-tasks-before-install">Performing any tasks required before the install:</span>

There are cases where one or more commands must be given prior to the
actual installation of a package. RPM performs these commands exactly as
directed by the package builder, thus eliminating a common source of
problems during installations.

</div>

<div class="sect2">

## <span id="s2-rpm-install-what-to-do-with-config-files">Deciding what to do with config files:</span>

One of the features that really sets RPM apart from other package
managers, is the way it handles configuration files. Since these files
are normally changed to customize the behavior of installed software,
simply overwriting a config file would tend to make people angry — all
their customizations would be gone\! Instead, RPM analyzes the situation
and attempts to do "the right thing" with config files, even if they
weren't originally installed by RPM\!
[<span class="footnote">\[1\]</span>](#FTN.AEN691)

</div>

<div class="sect2">

## <span id="s2-rpm-install-unpacking-files">Unpacking files from the package and putting them in the proper place:</span>

This is the step most people think of when they think about installing
software. Each package file contains a list of files that are to be
installed, as well as their destination on your system. In addition,
many other file attributes, such as permissions and ownerships, are set
correctly by RPM.

</div>

<div class="sect2">

## <span id="s2-rpm-install-tasks-after-install">Performing any tasks required after the install:</span>

Very often a new package requires that one or more commands be executed
after the new files are in place. An example of this would be running
**ldconfig** to make new shared libraries accessible.

</div>

<div class="sect2">

## <span id="s2-rpm-install-keeping-track">Keeping track of what it did:</span>

Every time RPM installs a package on your system, it keeps track of the
files it installed, in its database. The database contains a wealth of
information necessary for RPM to do its job. For example, RPM uses the
database when it checks for possible conflicts during an install.

</div>

</div>

### Notes

|                                                                   |                                                                                                                                                                                              |
| ----------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](ch-rpm-install.html#AEN691) | Are you interested in what exactly "the right thing" means? [the Section called *Config file magic* in Chapter 4](ch-rpm-upgrade.html#s2-rpm-upgrade-config-file-magic) has all the details. |

<div class="NAVFOOTER">

-----

|                                               |                    |                                                |
| :-------------------------------------------- | :----------------: | ---------------------------------------------: |
| [Prev](s1-intro-to-rpm-lets-get-started.html) | [Home](index.html) | [Next](s1-rpm-install-performing-install.html) |
| Let's Get Started                             |  [Up](p108.html)   |                          Performing an Install |

</div>
