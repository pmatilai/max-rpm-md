<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-query-handy-queries.html)

[Next](s1-rpm-verify-output.html)

-----

<div class="chapter">

# <span id="ch-rpm-verify"></span>Chapter 6. Using RPM to Verify Installed Packages

<div class="table">

<span id="tb-rpm-verify-rpm-v-syntax"></span>

**Table 6-1. **rpm -V** Command Syntax**

**rpm -V** or (**--verify**, or **-y**) *options*

</div>

</div>

Package Selection Options

Page

`pkg1` … `pkgN`

Verify named package(s)

[the Section called *The Package Label — Verify an Installed Package
Against the RPM
Database*](s1-rpm-verify-what-to-verify.html#s2-rpm-verify-package-label)

**-p `<file>`**

Verify against package file **`<file>`**

[the Section called ***-p `<file>`** — Verify Against a Specific Package
File*](s1-rpm-verify-what-to-verify.html#s2-rpm-verify-p-option)

**-f `<file>`**

Verify package owning **`<file>`**

[the Section called ***-f `<file>`** — Verify the Package Owning
**`<file>`** Against the RPM
Database*](s1-rpm-verify-what-to-verify.html#s2-rpm-verify-f-option)

**-a**

Verify all installed packages

[the Section called ***-a** — Verify All Installed Packages Against the
RPM Database*](s1-rpm-verify-what-to-verify.html#s2-rpm-verify-a-option)

**-g `<group>`**

Verify packages belonging to group **`<group>`**

[the Section called ***-g `<group>`** — Verify Packages Belonging To
**`<group>`***](s1-rpm-verify-what-to-verify.html#s2-rpm-verify-g-option)

Verify-specific Options

Page

**--noscripts**

Do not execute verification script

[the Section called ***--noscripts**: Do Not Execute Verification
Script*](s1-rpm-verify-what-to-verify.html#s2-rpm-verify-noscripts-option)

**--nodeps**

Do not verify dependencies

[the Section called ***--nodeps**: Do Not Check Dependencies During
Verification*](s1-rpm-verify-what-to-verify.html#s2-rpm-verify-nodeps-option)

**--nofiles**

Do not verify file attributes

[the Section called ***--nofiles**: Do Not Verify File
Attributes*](s1-rpm-verify-what-to-verify.html#s2-rpm-verify-nofiles-option)

General Options

Page

**-v**

Display additional information

[the Section called ***-v** — Display Additional
Information*](s1-rpm-verify-what-to-verify.html#s2-rpm-verify-v-option)

**-vv**

Display debugging information

[the Section called ***-vv** — Display Debugging
Information*](s1-rpm-verify-what-to-verify.html#s2-rpm-verify-vv-option)

**--root `<path>`**

Set alternate root to **`<path>`**

[the Section called ***--root `<path>`**: Set Alternate Root to
**`<path>`***](s1-rpm-verify-what-to-verify.html#s2-rpm-verify-root-option)

**--rcfile `<rcfile>`**

Set alternate rpmrc file to **`<rcfile>`**

[the Section called ***--rcfile `<rcfile>`**: Set Alternate `rpmrc` file
to
**`<rcfile>`***](s1-rpm-verify-what-to-verify.html#s2-rpm-verify-rcfile-option)

**--dbpath `<path>`**

Use **`<path>`** to find the RPM database

[the Section called ***--dbpath `<path>`**: Use **`<path>`** To Find RPM
Database*](s1-rpm-verify-what-to-verify.html#s2-rpm-verify-dbpath-option)

<div class="sect1">

# <span id="s1-rpm-verify-what-it-does">**rpm -V** — What Does it Do?</span>

From time to time, it's necessary to make sure that everything on your
system is "OK". Are you sure the packages you've installed are still
configured properly? Have there been any changes made that you don't
know about? Did you mistakenly start a recursive delete in `/usr` and
now have to assess the damage?

RPM can help. It can alert you to changes made to any of the files
installed by RPM. Also, if a package requires capabilities provided by
another package, it can make sure the other package is installed, too.

The command **rpm -V** (The options **-y** and **--verify** are
equivalent) verifies an installed package. Before we see how this is
done, let's take a step back and look at the big picture.

Every time a package is installed, upgraded, or erased, the changes are
logged in RPM's database. It's necessary for RPM to keep track of this
information; otherwise it wouldn't be able to perform these operations
correctly. You can think of the RPM database (and the disk space it
consumes) as being the "price of admission" for the easy package
management that RPM provides.
[<span class="footnote">\[1\]</span>](#FTN.AEN3891)

The RPM database reflects the configuration of the system on which it
resides. When RPM accesses the database to see how files should be
manipulated during an install, upgrade, or erase, it is using the
database as a mirror of the system's configuration.

However, we can also use the system configuration as a mirror of the RPM
database. What does this "backward" view give us? What purpose would be
served?

The purpose would be to see if the system configuration accurately
reflects the contents of the RPM database. If the system configuration
*doesn't* match the database, then we can reach one of two conclusions:

1.  The RPM database has become corrupt. The system configuration is
    unchanged.

2.  The RPM database is intact. The system configuration has changed.

While it would be foolish to state that an RPM database has *never*
become corrupt, it is a sufficiently rare occurrence that the second
conclusion is much more likely. So RPM gives us a powerful verification
tool, essentially for free.

<div class="sect2">

## <span id="s2-rpm-verify-what-does-it-verify">What Does it Verify?</span>

It would be handy if RPM did nothing more than verify that every file
installed by a package actually exists on your system. In reality, RPM
does much more. It makes sure that if a package depends on other
packages to provide certain capabilities, the necessary packages are, in
fact, installed. If the package builder created one, RPM will also run a
special verification script that can verify aspects of the package's
installation that RPM cannot.

Finally, every file installed by RPM is examined. No less than *nine*
different attributes of each file can be checked. Here is the list of
attributes:

  - Owner

  - Group

  - Mode

  - MD5 Checksum

  - Size

  - Major Number

  - Minor Number

  - Symbolic Link String

  - Modification Time

Let's take a look at each of these attributes and why they are good
things to check:

<div class="sect3">

### <span id="s3-rpm-verify-file-ownership">File Ownership</span>

Most operating systems today keep track of each file's creator. This is
done primarily for resource accounting. Linux and UNIX also use file
ownership to help determine access rights to the file. In addition, some
files, when executed by a user, can temporarily change the user's ID,
normally to a more privileged ID. Therefore, any change of file
ownership may have far reaching effects on data security and system
availability.

</div>

<div class="sect3">

### <span id="s3-rpm-verify-file-group">File Group</span>

In a similar manner to file ownership, a "group" specification is
attached to each file. Primarily used for determining access rights, a
file's group specification can also become a user's group ID, should
that user execute the file's contents. Therefore, any changes in a
file's group specification are important, and should be monitored.

</div>

<div class="sect3">

### <span id="s3-rpm-verify-file-mode">File Mode</span>

Encompassing the file's "permissions", the mode is a set of bits that
specifies permitted access for the file's owner, group members, and
everyone else. Even more important are two additional bits that
determine whether a user's group or user ID should be changed if they
execute the program contained in the file. Since these little bombshells
can let any user become `root` for the duration of the program, it pays
to be extra careful with a file's permissions.

</div>

<div class="sect3">

### <span id="s3-rpm-verify-md5-checksum">MD5 Checksum</span>

The MD5 checksum of a file is simply a 128-bit number that is
mathematically derived from the contents of the file. The MD5 algorithm
was designed by Ron Rivest, the "R" in the popular RSA public-key
encryption algorithm. The "MD" in "MD5" stands for *Message Digest*,
which is a pretty accurate description of what it does.

Unlike literary digests, an MD5 checksum conveys no information about
the contents of the original file. However, it possesses one unique
trait:

  - *Any* change to the file, no matter how small, results in a change
    to the MD5 checksum.
    [<span class="footnote">\[2\]</span>](#FTN.AEN3976)

RPM creates MD5 checksums of all files it manipulates, and stores them
in its database. For all intents and purposes, if one of these files is
changed, the MD5 checksum will change, and RPM will detect it.

</div>

<div class="sect3">

### <span id="s3-rpm-verify-file-size">File Size</span>

As if the use of MD5 isn't enough, RPM also keeps track of file sizes. A
difference of even one byte more or less will not go unnoticed.

</div>

<div class="sect3">

### <span id="s3-rpm-verify-major-number">Major Number</span>

Device character and block files possess a major number. The major
number is used to communicate information to the device driver
associated with the special file. For instance, under Linux the special
files for SCSI disk drives should have a major number of 8, while the
major number for an IDE disk drive's special file would be 3. As you can
imagine, any change to a file's major number can have disastrous
effects, and is tracked by RPM.

</div>

<div class="sect3">

### <span id="s3-rpm-verify-minor-number">Minor Number</span>

A file's minor number is similar in concept to the major number, but
conveys different information to the device driver. In the case of disk
drives, this information can consist of a unit identifier. Should the
minor number change, RPM will detect it.

</div>

<div class="sect3">

### <span id="s3-rpm-verify-symbolic-link">Symbolic Link</span>

If the file in question is really a symbolic link, the text string
containing the name of the linked-to file is checked.

</div>

<div class="sect3">

### <span id="s3-rpm-verify-modification-time">Modification Time</span>

Most operating systems keep track of the date and time that a file was
last modified. RPM uses this to its advantage by keeping modification
times in its database.

</div>

</div>

</div>

### Notes

|                                                                   |                                                                                                                                                                                                                 |
| ----------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](ch-rpm-verify.html#AEN3891) | Actually, the price is fairly low. For a completely RPM-based Linux distribution, it would be unusual to have a database over 5MB in size.                                                                      |
| [<span class="footnote">\[2\]</span>](ch-rpm-verify.html#AEN3976) | From a strictly theoretical standpoint, this is not entirely true. Using the lingo of cryptologists, it is believed to be "computationally infeasible" to find two messages that produce the same MD5 checksum. |

<div class="NAVFOOTER">

-----

|                                         |                    |                                             |
| :-------------------------------------- | :----------------: | ------------------------------------------: |
| [Prev](s1-rpm-query-handy-queries.html) | [Home](index.html) |           [Next](s1-rpm-verify-output.html) |
| A Few Handy Queries                     |  [Up](p108.html)   | When Verification Fails — **rpm -V** Output |

</div>
