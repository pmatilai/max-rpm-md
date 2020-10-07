<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-upgrade-nearly-identical.html)

[Next](s1-rpm-query-parts.html)

-----

<div class="chapter">

# <span id="ch-rpm-query"></span>Chapter 5. Getting Information About Packages

<div class="table">

<span id="tb-rpm-query-q-syntax"></span>

**Table 5-1. **rpm -q** Command Syntax**

**rpm -q** (or **--query**) *options*

</div>

</div>

Package Selection Options

Page

`pkg1` … `pkgN`

Query installed package(s)

[the Section called *The Package
Label*](s1-rpm-query-parts.html#s3-rpm-query-package-label)

**-p `<file>`**(or "**-**")

Query package file **`<file>`** (URLs OK)

[the Section called ***-p `<file>`** — Query a Specific RPM Package
File*](s1-rpm-query-parts.html#s3-rpm-query-p-option)

**-f `<file>`**

Query package owning **`<file>`**

[the Section called ***-f `<file>`** — Query the Package Owning
**`<file>`***](s1-rpm-query-parts.html#s3-rpm-query-by-file)

**-a**

Query all installed packages

[the Section called ***-a** — Query All Installed
Packages*](s1-rpm-query-parts.html#s3-rpm-query-a-option)

**--whatprovides `<x>`**

Query packages providing capability **`<x>`**

[the Section called ***--whatprovides `<x>`**: Query the Packages That
Provide Capability
**`<x>`***](s1-rpm-query-parts.html#s3-rpm-query-whatprovides-option)

**-g `<group>`**

Query packages belonging to group **`<group>`**

[the Section called ***-g `<group>`**: Query Packages Belonging To Group
**`<group>`***](s1-rpm-query-parts.html#s3-rpm-query-g-option)

**--whatrequires `<x>`**

Query packages requiring capability **`<x>`**

[the Section called ***--whatrequires `<x>`**: Query the Packages That
Require Capability
**`<x>`***](s1-rpm-query-parts.html#s3-rpm-query-whatrequires)

Information Selection Options

Page

**`<null>`**

Display full package label

[the Section called *The Package
Label*](s1-rpm-query-parts.html#s3-rpm-query-package-label)

**-i**

Display summary package information

[the Section called ***-i** — Display Package
Information*](s1-rpm-query-parts.html#s3-rpm-query-i-option)

**-l**

Display list of files in package

[the Section called ***-l** — Display the Package's File
List*](s1-rpm-query-parts.html#s3-rpm-query-l-option)

**-c**

Display list of configuration files

[the Section called ***-c** — Display the Package's List of
Configuration Files*](s1-rpm-query-parts.html#s3-rpm-query-c-option)

**-d**

Display list of documentation files

[the Section called ***-d** — Display a List of the Package's
Documentation*](s1-rpm-query-parts.html#s3-rpm-query-d-option)

**-s**

Display list of files in package, with state

[the Section called ***-s** — Display the State of Each File in the
Package*](s1-rpm-query-parts.html#s3-rpm-query-s-option)

**--scripts**

Display install, uninstall, verify scripts

[the Section called ***--scripts** — Show Scripts Associated With a
Package*](s1-rpm-query-parts.html#s3-rpm-query-scripts-option)

**--queryformat** (or **--qf**)

Display queried data in custom format

[the Section called ***--queryformat** — Construct a Custom Query
Response*](s1-rpm-query-parts.html#s3-rpm-query-queryformat-option)

**--dump**

Display all verifiable information for each file

[the Section called ***--dump**: Display All Verifiable Information for
Each File*](s1-rpm-query-parts.html#s3-rpm-query-dump-option)

**--provides**

Display capabilities package provides

[the Section called ***--provides**: Display Capabilities Provided by
the Package*](s1-rpm-query-parts.html#s3-rpm-query-provides-option)

**--requires** (or **-R**)

Display capabilities package requires

[the Section called ***--requires**: Display Capabilities Required by
the Package*](s1-rpm-query-parts.html#s3-rpm-query-requires-option)

General Options

Page

**-v**

Display additional information

[the Section called ***-v** — Display Additional
Information*](s1-rpm-query-parts.html#s4-rpm-query-v-option)

**-vv**

Display debugging information

[the Section called *Getting a *lot* more information with
**-vv***](s1-rpm-query-parts.html#s2-rpm-query-vv-option)

**--root `<path>`**

Set alternate root to **`<path>`**

[the Section called ***--root `<path>`**: Use **`<path>`** As An
Alternate Root*](s1-rpm-query-parts.html#s2-rpm-query-root-option)

**--rcfile `<rcfile>`**

Set alternate `rpmrc` file to **`<rcfile>`**

[the Section called ***--rcfile `<rcfile>`**: Use **`<rcfile>`** As An
Alternate `rpmrc`
File*](s1-rpm-query-parts.html#s2-rpm-query-rcfile-option)

**--dbpath `<path>`**

Use **`<path>`** to find the RPM database

[the Section called ***--dbpath `<path>`**: Use **`<path>`** To Find RPM
Database*](s1-rpm-query-parts.html#s2-rpm-query-dbpath-option)

<div class="sect1">

# <span id="s1-rpm-query-what-it-does">**rpm -q** — What does it do?</span>

One of the nice things about using RPM is that the packages you manage
don't end up going into some kind of black hole. Nothing would be worse
than to install, upgrade, and erase several different packages and not
have a clue as to what's on your system. In fact, RPM's query function
can help you get out of sticky situations like:

  - You're poking around your system, and you come across a file that
    you just can't identify. Where did it come from?

  - Your friend sends you a package file, and you have no idea what the
    package does, what it installs, or where it originally came from.

  - You know that you installed XFree86 a couple months ago, but you
    don't know what version, and you can't find any documentation on it.

The list could go on, but you get the idea. The **rpm -q** command is
what you need. If you're the kind of person that doesn't like to have
more options than you know what to do with, **rpm -q** might look
imposing. But fear not. Once you have a handle on the basic structure of
an RPM query, it'll be a piece of cake.

</div>

<div class="NAVFOOTER">

-----

|                                              |                    |                                 |
| :------------------------------------------- | :----------------: | ------------------------------: |
| [Prev](s1-rpm-upgrade-nearly-identical.html) | [Home](index.html) | [Next](s1-rpm-query-parts.html) |
| They're Nearly Identical…                    |  [Up](p108.html)   |       The Parts of an RPM Query |

</div>
