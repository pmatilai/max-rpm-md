<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-erase-watch-out.html)

[Next](s1-rpm-upgrade-upgrading-a-package.html)

-----

<div class="chapter">

# <span id="ch-rpm-upgrade"></span>Chapter 4. Using RPM to Upgrade Packages

<div class="table">

<span id="tb-rpm-upgrade-syntax"></span>

**Table 4-1. **rpm -U** Command Syntax**

**rpm -U** (or **--upgrade**)*options* `file1.rpm` … `fileN.rpm`

</div>

</div>

Parameters

`file1.rpm` … `fileN.rpm`

One or more RPM package files (URLs OK)

Upgrade-specific Options

Page

**-h** (or **--hash**)

Print hash marks ("\#") during
upgrade[<span class="footnote">\[a\]</span>](ch-rpm-upgrade.html#FTN.fn-rpm-upgrade-tablefoot)

[the Section called ***-h**: Perfect for the Impatient* in Chapter
2](s1-rpm-install-handy-options.html#s2-rpm-install-install-h)

**--oldpackage**

Permit "upgrading" to an older package

[the Section called ***--oldpackage**: Upgrade To An Older
Version*](s1-rpm-upgrade-nearly-identical.html#s2-rpm-upgrade-oldpackage-option)

**--test**

Perform upgrade tests
only[<span class="footnote">\[a\]</span>](ch-rpm-upgrade.html#FTN.fn-rpm-upgrade-tablefoot)

[the Section called ***--test**: Perform Installation Tests Only* in
Chapter
2](s1-rpm-install-additional-options.html#s2-rpm-install-test-option)

**--excludedocs**

Do not install
documentation[<span class="footnote">\[a\]</span>](ch-rpm-upgrade.html#FTN.fn-rpm-upgrade-tablefoot)

[the Section called ***--excludedocs**: Do Not Install Documentation For
This Package* in Chapter
2](s1-rpm-install-additional-options.html#s2-rpm-install-excludedocs-option)

**--includedocs**

Install
documentation[<span class="footnote">\[a\]</span>](ch-rpm-upgrade.html#FTN.fn-rpm-upgrade-tablefoot)

[the Section called ***--includedocs**: Install Documentation For This
Package* in Chapter
2](s1-rpm-install-additional-options.html#s2-rpm-install-includedocs)

**--replacepkgs**

Replace a package with a new copy of
itself[<span class="footnote">\[a\]</span>](ch-rpm-upgrade.html#FTN.fn-rpm-upgrade-tablefoot)

[the Section called ***--replacepkgs**: Install the Package Even If
Already Installed* in Chapter
2](s1-rpm-install-additional-options.html#s2-rpm-install-replacepkgs)

**--replacefiles**

Replace files owned by another
package[<span class="footnote">\[a\]</span>](ch-rpm-upgrade.html#FTN.fn-rpm-upgrade-tablefoot)

[the Section called ***--replacefiles**: Install the Package Even If It
Replaces Another Package's Files* in Chapter
2](s1-rpm-install-additional-options.html#s2-rpm-install-replacefiles-option)

**--force**

Ignore package and file conflicts

[the Section called ***--force**: The Big
Hammer*](s1-rpm-upgrade-nearly-identical.html#s2-rpm-upgrade-force-option)

**--percent**

Print percentages during
upgrade[<span class="footnote">\[a\]</span>](ch-rpm-upgrade.html#FTN.fn-rpm-upgrade-tablefoot)

[the Section called ***--percent**: Not Meant for Human Consumption* in
Chapter
2](s1-rpm-install-additional-options.html#s2-rpm-install-percent)

**--noscripts**

Do not execute pre- and post-install scripts

[the Section called ***--noscripts**: Do Not Execute Install and
Uninstall
Scripts*](s1-rpm-upgrade-nearly-identical.html#s2-rpm-upgrade-noscripts-option)

**--prefix `<path>`**

Relocate package to **`<path>`** if
possible[<span class="footnote">\[a\]</span>](ch-rpm-upgrade.html#FTN.fn-rpm-upgrade-tablefoot)

[the Section called ***--prefix `<path>`**: Relocate the package to
**`<path>`**, if possible* in Chapter
2](s1-rpm-install-additional-options.html#s2-rpm-install-prefix)

**--ignorearch**

Do not verify package
architecture[<span class="footnote">\[a\]</span>](ch-rpm-upgrade.html#FTN.fn-rpm-upgrade-tablefoot)

[the Section called ***--ignorearch**: Do Not Verify Package
Architecture* in Chapter
2](s1-rpm-install-additional-options.html#s2-rpm-install-ignorearch)

**--ignoreos**

Do not verify package operating
system[<span class="footnote">\[a\]</span>](ch-rpm-upgrade.html#FTN.fn-rpm-upgrade-tablefoot)

[the Section called ***--ignoreos**: Do Not Verify Package Operating
System* in Chapter
2](s1-rpm-install-additional-options.html#s2-rpm-install-ignoreos)

**--nodeps**

Do not check
dependencies[<span class="footnote">\[a\]</span>](ch-rpm-upgrade.html#FTN.fn-rpm-upgrade-tablefoot)

[the Section called ***--nodeps**: Do Not Check Dependencies Before
Installing Package* in Chapter
2](s1-rpm-install-additional-options.html#s2-rpm-install-nodeps)

**--ftpproxy `<host>`**

Use **`<host>`** as the FTP
proxy[<span class="footnote">\[a\]</span>](ch-rpm-upgrade.html#FTN.fn-rpm-upgrade-tablefoot)

[the Section called ***--ftpproxy `<host>`**: Use **`<host>`** As Proxy
In FTP-based Installs* in Chapter
2](s1-rpm-install-additional-options.html#s2-rpm-install-ftpproxy)

**--ftpport `<port>`**

Use **`<port>`** as the FTP
port[<span class="footnote">\[a\]</span>](ch-rpm-upgrade.html#FTN.fn-rpm-upgrade-tablefoot)

[the Section called ***--ftpport `<port>`**: Use **`<port>`** In
FTP-based Installs* in Chapter
2](s1-rpm-install-additional-options.html#s2-rpm-install-ftpport)

General Options

Page

**-v**

Display additional
information[<span class="footnote">\[a\]</span>](ch-rpm-upgrade.html#FTN.fn-rpm-upgrade-tablefoot)

[the Section called *Getting a bit more feedback with **-v*** in Chapter
2](s1-rpm-install-handy-options.html#s2-rpm-install-more-feedback)

**-vv**

Display debugging
information[<span class="footnote">\[a\]</span>](ch-rpm-upgrade.html#FTN.fn-rpm-upgrade-tablefoot)

[the Section called *Getting a *lot* more information with **-vv*** in
Chapter
2](s1-rpm-install-additional-options.html#s2-rpm-install-vv-option)

**--root `<path>`**

Set alternate root to
**`<path>`**[<span class="footnote">\[a\]</span>](ch-rpm-upgrade.html#FTN.fn-rpm-upgrade-tablefoot)

[the Section called ***--root `<path>`**: Use **`<path>`** As An
Alternate Root* in Chapter
2](s1-rpm-install-additional-options.html#s2-rpm-install-root)

**--rcfile `<rcfile>`**

Set alternate rpmrc file to
**`<rcfile>`**[<span class="footnote">\[a\]</span>](ch-rpm-upgrade.html#FTN.fn-rpm-upgrade-tablefoot)

[the Section called ***--rcfile `<rcfile>`**: Use **`<rcfile>`** As An
Alternate `rpmrc` File* in Chapter
2](s1-rpm-install-additional-options.html#s2-rpm-install-rcfile)

**--dbpath `<path>`**

Use **`<path>`** to find the RPM database
[<span class="footnote">\[a\]</span>](#FTN.fn-rpm-upgrade-tablefoot)

[the Section called ***--dbpath `<path>`**: Use **`<path>`** To Find RPM
Database* in Chapter
2](s1-rpm-install-additional-options.html#s2-rpm-install-dbpath)

Notes:  
<span id="FTN.fn-rpm-upgrade-tablefoot">a.</span> This option behaves
identically to the same option used with **rpm -i**. Please see [Chapter
2](ch-rpm-install.html) for more information on this option.  

<div class="sect1">

# <span id="s1-rpm-upgrade-what-it-does">**rpm -U** — What Does it Do?</span>

If there was one RPM command that could win over friends, it would be
RPM's upgrade command. After all, anyone who has ever tried to install a
newer version of *any* software knows what a traumatic experience it can
be. With RPM, though, this process is reduced to a single command: **rpm
-U**. The **rpm -U** command (**--upgrade** is equivalent) performs two
distinct operations:

1.  Installs the desired package.

2.  Erases all older versions of the package, if any exist.

If it sounds to you like **rpm -U** is nothing more than an **rpm -i**
command (see [Chapter 2](ch-rpm-install.html)) followed by the
appropriate number of **rpm -e** commands, (see [Chapter
3](ch-rpm-erase.html)) you'd be exactly right. In fact, we'll be
referring back to those chapters as we discuss **rpm -U**, so if you
haven't skimmed those chapters yet, you might want to do that now.

While some people might think it's a "cheap shot" to claim that RPM
performs an upgrade when in fact it's just doing the equivalent of a
couple of other commands, in fact, it's a very smart thing to do. By
carefully crafting RPM's package installation and erasure commands to do
the work required during an upgrade, it makes RPM more tolerant of
misuse by preserving important files even if an upgrade isn't being
done.

If RPM had been written with a very "smart" upgrade command, and the
install and erase commands couldn't handle upgrade situations at all,
installing a package could overwrite a modified configuration file.
Likewise, erasing a package would also mean that config files could be
erased. Not a good situation\! However, RPM's approach to upgrades makes
it possible to handle even the most tricky situation — having multiple
versions of a package install simultaneously.

<div class="sect2">

## <span id="s2-rpm-upgrade-config-file-magic">Config file magic</span>

While the **rpm -i** and **rpm -e** commands each do their part to keep
config files straight, it is with **rpm -U** that the full power of
RPM's config file handling shows through. There are no less than *six*
different scenarios that RPM takes into account when handling config
files.

In order to make the appropriate decisions, RPM needs information. The
information used to decide how to handle config files is a set of three
large numbers known as *MD5 checksums*. An MD5 checksum is produced when
a file is used as the input to a complex series of mathematical
operations. The resulting checksum has a unique property, in that *any*
change to the file's contents will result in a change to the checksum of
that file. [<span class="footnote">\[1\]</span>](#FTN.AEN2156)
Therefore, MD5 checksums are a powerful tool for quickly determining
whether two different files have the same contents or not.

In the previous paragraph, we stated that RPM uses three different MD5
checksums to determine what should be done with a config file. The three
checksums are:

1.  The MD5 checksum of the file when it was originally installed. We'll
    call this the *original file*.

2.  The MD5 checksum of the file as it exists at upgrade time. We'll
    call this the *current file*.

3.  The MD5 checksum of the corresponding file in the new package. We'll
    call this the *new file*.

Let's take a look at the various combinations of checksums, see what RPM
will do because of them, and discuss why. In the following examples,
we'll use the letters `X`, `Y`, and `Z` in place of lengthy MD5
checksums.

<div class="sect3">

### <span id="s3-rpm-upgrade-xxx">Original file = `X`, Current file = `X`, New file = `X`</span>

In this case, the file originally installed was never modified.
[<span class="footnote">\[2\]</span>](#FTN.AEN2180) The file in the new
version of the package is identical to the file on disk.

In this case, RPM installs the new file, overwriting the original. You
may be wondering why go to the trouble of installing the new file if
it's just the same as the existing one. The reason is that aspects of
the file *other* than its name and contents might have changed. The
file's ownership, for example, might be different in the new version.

</div>

<div class="sect3">

### <span id="s3-rpm-upgrade-xxy">Original file = `X`, Current file = `X`, New file = `Y`</span>

The original file has not been modified, but the file in the new package
*is* different. Perhaps the difference represents a bug-fix, or a new
feature. It makes no difference to RPM.

In this case, RPM installs the new file, overwriting the original. This
makes sense. If it didn't, RPM would never permit newer, modified
versions of software to be installed\! The original file is not saved,
since it had not been changed. A lack of changes here means that no
site-specific modifications were made to the file.

</div>

<div class="sect3">

### <span id="s3-rpm-upgrade-xyx">Original file = `X`, Current file = `Y`, New file = `X`</span>

Here we have a file that *was* changed at some point. However, the new
file is identical to the existing file *prior* to the local
modifications.

In this case, RPM takes the viewpoint that since the original file and
the new file are identical, the modifications made to the original
version must still be valid for the new version. It leaves the existing,
modified file in place.

</div>

<div class="sect3">

### <span id="s3-rpm-upgrade-xyy">Original file = `X`, Current file = `Y`, New file = `Y`</span>

At some point the original file was modified, and those modifications
happen to make the file identical to the new file. Perhaps the
modification was made to fix a security problem, and the new version of
the file has the same fix applied to it.

In this case, RPM installs the new version, overwriting the modified
original. The same philosophy used in the first scenario applies here —
although the file has not changed, perhaps some other aspect of the file
has, so the new version is installed.

</div>

<div class="sect3">

### <span id="s3-rpm-upgrade-xyz">Original file = `X`, Current file = `Y`, New file = `Z`</span>

Here the original file was modified at some point. The new file is
different from both the original *and* the modified versions of the
original file.

RPM is not able to analyze the contents of the files, and determine what
is going on. In this instance, it takes the best possible approach. The
new file is known to work properly with the rest of the software in the
new package — at least the people building the new package should have
insured that it does. The modified original file is an unknown: it might
work with the new package, it might not. So RPM installs the new file.

BUT… The existing file was definitely modified. Someone made an effort
to change the file, for some reason. Perhaps the information contained
in the file is still of use. Therefore, RPM saves the modified file,
naming it `<file>`.rpmsave, and prints a warning, so the user knows what
happened:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>warning: /etc/skel/.bashrc saved as /etc/skel/.bashrc.rpmsave

            </code></pre></td>
</tr>
</tbody>
</table>

These five scenarios cover just about every possible circumstance, save
one. The missing scenario?

</div>

<div class="sect3">

### <span id="s3-rpm-upgrade-none-huh-huh">Original file = *none*, Current file = `??`, New file = `??`</span>

While RPM doesn't use checksums in this particular case, we'll describe
it in those terms, for the sake of consistency. In this instance, RPM
had not installed the file originally, so there is no original checksum.

Because the file had not originally been installed as part of a package,
there is no way for RPM to determine if the file currently in place had
been modified. Therefore, the checksums for the current file and the new
file are irrelevant; they cannot be used to clear up the mystery.

When this happens, RPM renames the file to `<file>`.rpmorig, prints a
warning, and installs the new file. This way, any modifications
contained in the original file are saved. The system administrator can
review the differences between the original and the newly installed
files and determine what action should be taken.

As you can see, in the majority of cases RPM will automatically take the
proper course of action when performing an upgrade. It is only when
config files have been modified and are to be overwritten, that RPM
leaves any post-upgrade work for the system administrator. Even in those
cases, many times the modified files are not worth saving and can be
deleted.

</div>

</div>

</div>

### Notes

|                                                                    |                                                                                                                                                        |
| ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [<span class="footnote">\[1\]</span>](ch-rpm-upgrade.html#AEN2156) | Actually, there's a one in 2<sup>128</sup> chance a change will go undetected, but for all practical purposes, it's as close to perfect as we can get. |
| [<span class="footnote">\[2\]</span>](ch-rpm-upgrade.html#AEN2180) | Or, as some sticklers for detail may note, it may have been modified, and subsequently those modifications were undone.                                |

<div class="NAVFOOTER">

-----

|                                     |                    |                                                 |
| :---------------------------------- | :----------------: | ----------------------------------------------: |
| [Prev](s1-rpm-erase-watch-out.html) | [Home](index.html) | [Next](s1-rpm-upgrade-upgrading-a-package.html) |
| Watch Out\!                         |  [Up](p108.html)   |                             Upgrading a Package |

</div>
