<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-install-handy-options.html)

Chapter 2. Using RPM to Install Packages

[Next](ch-rpm-erase.html)

-----

<div class="sect1">

# <span id="s1-rpm-install-additional-options">Additional options to **rpm -i**</span>

Normally **rpm -i**, perhaps with the **-v** and **-h**, is all you'll
need. However, there may be times when a basic install is not going to
get the job done. Fortunately, RPM has a wealth of install options to
make the tough times a little easier. As with any other powerful tool,
you should understand these options before putting them to use. Let's
take a look at them:

<div class="sect2">

## <span id="s2-rpm-install-vv-option">Getting a *lot* more information with **-vv**</span>

Sometimes it's necessary to have even *more* information than we can get
with **-v**. By adding another **v**, we can start to see more of RPM's
inner workings:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -ivv eject-1.2-2.i386.rpm
D: installing eject-1.2-2.i386.rpm
Installing eject-1.2-2.i386.rpm
D: package: eject-1.2-2 files test = 0
D: running preinstall script (if any)
D: setting file owners and groups by name (not id)
D: ///usr/bin/eject owned by root (0), group root (0) mode 755
D: ///usr/man/man1/eject.1 owned by root (0), group root (0) mode 644
D: running postinstall script (if any)

#
          </code></pre></td>
</tr>
</tbody>
</table>

The lines starting with D: have been added by using **-vv**. The line
ending with "files test = 0", means that RPM is actually going to
install the package. If the number were non-zero, it would mean that the
**--test** option was present, and RPM would not actually perform the
installation. For more information on using **--test** with **rpm -i**,
see [the Section called ***--test**: Perform Installation Tests
Only*](s1-rpm-install-additional-options.html#s2-rpm-install-test-option).

Continuing with the above example, we see that RPM next executes a
pre-install script (if there is one), followed by the actual
installation of the files in the package. There is one line for each
file being installed, and that line shows the filename, ownership, group
membership, and permissions (or mode) applied to the file. With larger
packages, the output from **-vv** can get quite lengthy\! Finally, RPM
runs a post-install script, if one exists for the package. We'll be
discussing pre- and post-install scripts in more detail in [the Section
called ***--noscripts**: Do Not Execute Pre- and Post-install
Scripts*](s1-rpm-install-additional-options.html#s2-rpm-install-noscripts).

In the vast majority of cases, it will not be necessary to use **-vv**.
It is normally used by software engineers working on RPM itself, and the
output can change without notice. However, it's a handy way to gain
insights into RPM's inner workings.

</div>

<div class="sect2">

## <span id="s2-rpm-install-test-option">**--test**: Perform Installation Tests Only</span>

There are times when it's more appropriate to take it slow and not try
to install a package right away. RPM provides the **--test** option for
that. As the names implies, it performs all the checks that RPM normally
does during an install, but stops short of actually performing the steps
necessary to install the package:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -i --test eject-1.2-2.i386.rpm
#
          </code></pre></td>
</tr>
</tbody>
</table>

Once again, there's not very much output. This is because the test
succeeded; had there been a problem, the output would have been a bit
more interesting. In this example, there are some problems:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -i --test rpm-2.0.11-1.i386.rpm
/bin/rpm conflicts with file from rpm-2.3-1
/usr/bin/gendiff conflicts with file from rpm-2.3-1
/usr/bin/rpm2cpio conflicts with file from rpm-2.3-1
/usr/bin/rpmconvert conflicts with file from rpm-2.3-1
/usr/man/man8/rpm.8 conflicts with file from rpm-2.3-1
error: rpm-2.0.11-1.i386.rpm cannot be installed

#
          </code></pre></td>
</tr>
</tbody>
</table>

If you'll note the version numbers, we're trying to install an older
version of RPM (2.0.11) "on top of" a newer version(2.3). RPM faithfully
reported the various file conflicts and summarized with a message saying
that the install would not have proceeded, even if **--test** had not
been on the command line.

The **--test** option will also catch dependency-related problems:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -i --test blather-7.9-1.i386.rpm
failed dependencies:
        bother &gt;= 3.1 is needed by blather-7.9-1

#
          </code></pre></td>
</tr>
</tbody>
</table>

Here's a tip for all you script-writers out there: RPM will return a
non-zero status if the **--test** option detects problems…

</div>

<div class="sect2">

## <span id="s2-rpm-install-replacepkgs">**--replacepkgs**: Install the Package Even If Already Installed</span>

The **--replacepkgs** option is used to force RPM to install a package
that it believes to be installed already. This option is normally used
if the installed package has been damaged somehow and needs to be fixed
up.

To see how the **--replacepkgs** option works, let's first install some
software:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -iv cdp-0.33-2.i386.rpm
Installing cdp-0.33-2.i386.rpm

#
          </code></pre></td>
</tr>
</tbody>
</table>

OK, now that we have `cdp-0.33-2` installed, let's see what happens if
we try to install the same version "on top of" itself:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -iv cdp-0.33-2.i386.rpm
Installing cdp-0.33-2.i386.rpm
package cdp-0.33-2 is already installed
error: cdp-0.33-2.i386.rpm cannot be installed

#
          </code></pre></td>
</tr>
</tbody>
</table>

That didn't go very well. Let's see what adding **--replacepkgs** will
do :

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -iv --replacepkgs cdp-0.33-2.i386.rpm
Installing cdp-0.33-2.i386.rpm

#
          </code></pre></td>
</tr>
</tbody>
</table>

Much better. The original package was replaced by a new copy of itself.

</div>

<div class="sect2">

## <span id="s2-rpm-install-replacefiles-option">**--replacefiles**: Install the Package Even If It Replaces Another Package's Files</span>

While the **--replacepkgs** option permitted a package to be installed
"on top of" itself, **--replacefiles** is used to allow a package to
overwrite files belonging to a different package. Sounds strange? Let's
go over it in a bit more detail.

One thing that sets RPM apart from many other package managers is that
it keeps track of all the files it installs in a database. Each file's
database entry contains a variety of information about the file,
including a means of summarizing the file's contents.
[<span class="footnote">\[1\]</span>](#FTN.AEN970) By using these
summaries, known as *MD5 checksums*, RPM can determine if a particular
file is going to be replaced by a file with the same name, but different
contents. Here's an example:

Package "A" installs a file (we'll call it `/bin/foo.bar`). Once Package
A is installed, `foo.bar` resides happily in the `/bin` directory. In
the RPM database, there is an entry for `/bin/foo.bar`, including the
file's MD5 checksum.

However, there is a another package, "B". Package B also has a file
called `foo.bar` that *it* wants to install in `/bin`. There can't be
two files in the same directory with the same name. The files are
different; their MD5 checksums do not match. What happens if Package B
is installed? Let's find out. Here, we've installed a package:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -iv cdp-0.33-2.i386.rpm
Installing cdp-0.33-2.i386.rpm

#
          </code></pre></td>
</tr>
</tbody>
</table>

OK, no problem there. But we have another package to install. In this
case, it is a new release of the `cdp` package. It should be noted that
RPM's detection of file conflicts does not depend on the two packages
being related. It is strictly based on the name of the file, the
directory in which it resides, and the file's MD5 checksum. Here's what
happens when we try to install the package:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -iv cdp-0.33-3.i386.rpm
Installing cdp-0.33-3.i386.rpm
/usr/bin/cdp conflicts with file from cdp-0.33-2
error: cdp-0.33-3.i386.rpm cannot be installed

#
          </code></pre></td>
</tr>
</tbody>
</table>

What's happening? The package `cdp-0.33-2` has a file, `/usr/bin/cdp`,
that it installed. Sure enough, there it is. Let's highlight the size
and creation date of the file for future reference:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -al /usr/bin/cdp
-rwxr-xr-x   1 root     root        34460 Feb 25 14:27 /usr/bin/cdp

#
          </code></pre></td>
</tr>
</tbody>
</table>

The package we just tried to install, `cdp-0.33-3` (note the different
release), also installs a file `cdp` in `/usr/bin`. Since there is a
conflict, that means that the two package's `cdp` files must be
different — their checksums don't match. Because of this, RPM won't let
the second package install. But with **--replacefiles**, we can force
RPM to let the `/usr/bin/cdp` from `cdp-0.33-3` replace the
`/usr/bin/cdp` from `cdp-0.33-2`:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -iv --replacefiles cdp-0.33-3.i386.rpm
Installing cdp-0.33-3.i386.rpm

#
          </code></pre></td>
</tr>
</tbody>
</table>

Taking a closer look at `/usr/bin/cdp`, we find that they certainly
*are* different, both in size and creation date:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -al /usr/bin/cdp
-rwxr-xr-x   1 root     root        34444 Apr 24 22:37 /usr/bin/cdp

#
          </code></pre></td>
</tr>
</tbody>
</table>

File conflicts should be a relatively rare occurrence. They only happen
when two packages attempt to install files with the same name but
different contents. There are two possible reasons for this to happen:

  - Installing a newer version of a package without erasing the older
    version. A newer version of a package is a *wonderful* source of
    file conflicts against older versions — the filenames remain the
    same, but the contents change. We used it in our example because
    it's an easy way to show what happens when there are file conflicts.
    However, it is usually a *bad* idea when it comes to doing this as a
    way to upgrade packages. RPM has a special option for this (**rpm
    -U**) that is discussed in [Chapter 4](ch-rpm-upgrade.html).

  - Installing two unrelated packages that each install a file with the
    same name. This may happen because of poor package design (hence the
    file residing in more than one package), or a lack of coordination
    between the people building the packages.

<div class="sect3">

### <span id="s3-rpm-install-replacefiles-and-config-files">**--replacefiles** and Config Files</span>

What happens if a conflicting file is a config file that you've sweated
over and worked on until it's just right? Will issuing a
**--replacefiles** on a package with a conflicting config file blow all
your changes away?

No\! RPM won't cook your goose.
[<span class="footnote">\[2\]</span>](#FTN.AEN1054)

It will save any changes you've made, to a config file called
`<file>`.rpmsave. Let's give it a try:

As system administrator, you want to make sure your new users have a
rich environment the first time they log in. So you've come up with a
really nifty `.bashrc` file that will be executed whenever they log in.
Knowing that *everyone* will enjoy your wonderful `.bashrc` file, you
place it in `/etc/skel`. That way, every time a new account is created,
your `.bashrc` will be copied into the new user's login directory.

Not realizing that the `.bashrc` file you modified in `/etc/skel` is
listed as a config file in a package called (strangely enough)
`etcskel`, you decide to experiment with RPM using the `etcskel`
package. First you try to install it:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -iv etcskel-1.0-100.i386.rpm
etcskel       /etc/skel/.bashrc conflicts with file from etcskel-1.0-3
error: etcskel-1.0-100.i386.rpm cannot be installed

#
            </code></pre></td>
</tr>
</tbody>
</table>

Hmmm. That didn't work. Wait a minute\! I can add **--replacefiles** to
the command and it should install just fine:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -iv --replacefiles etcskel-1.0-100.i386.rpm
Installing etcskel-1.0-100.i386.rpm
warning: /etc/skel/.bashrc saved as /etc/skel/.bashrc.rpmsave

#
            </code></pre></td>
</tr>
</tbody>
</table>

Wait a minute… That's my customized `.bashrc`\! Was it *really* saved?

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -al /etc/skel/
total 8
-rwxr-xr-x   1 root     root          186 Oct 12  1994 .Xclients
-rw-r--r--   1 root     root         1126 Aug 23  1995 .Xdefaults
-rw-r--r--   1 root     root           24 Jul 13  1994 .bash_logout
-rw-r--r--   1 root     root          220 Aug 23  1995 .bash_profile
-rw-r--r--   1 root     root          169 Jun 17 20:02 .bashrc
-rw-r--r--   1 root     root          159 Jun 17 20:46 .bashrc.rpmsave
drwxr-xr-x   2 root     root         1024 May 13 13:18 .xfm
lrwxrwxrwx   1 root     root            9 Jun 17 20:46 .xsession -&gt; .Xclients

# cat /etc/skel/.bashrc.rpmsave
# .bashrc
# User specific aliases and functions
# Modified by the sysadmin
uptime
# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi

#
            </code></pre></td>
</tr>
</tbody>
</table>

Whew\! You heave a sigh of relief, and study the new `.bashrc` to see if
the changes need to be integrated into your customized version.

</div>

<div class="sect3">

### <span id="s3-rpm-install-replacefiles-trouble">**--replacefiles** Can Mean Trouble Down the Road</span>

While **--replacefiles** can make today's difficult install go away, it
can mean big headaches in the future. When the time comes for erasing
the packages involved in a file conflict, bad things can happen.

What bad things? Well, files can be deleted. Here's how, in three easy
steps:

1.  Two packages are installed. When the second package is installed,
    there is a conflict with a file installed by the first package.
    Therefore, the **--replacefiles** option is used to force RPM to
    replace the conflicting file with the one from the second package.

2.  At some point in the future, the second package is erased.

3.  The conflicting file is gone\!

Let's look at an example. First, we install a new package. Next, we take
a look at a file it installed, noting the size and creation date.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -iv cdp-0.33-2.i386.rpm
Installing cdp-0.33-2.i386.rpm

# ls -al /usr/bin/cdp
-rwxr-xr-x   1 root     root        34460 Feb 25 14:27 /usr/bin/cdp

#
            </code></pre></td>
</tr>
</tbody>
</table>

Next, we try to install a newer release of the same package. It fails:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -iv cdp-0.33-3.i386.rpm
Installing cdp-0.33-3.i386.rpm
/usr/bin/cdp conflicts with file from cdp-0.33-2
error: cdp-0.33-3.i386.rpm cannot be installed

#
            </code></pre></td>
</tr>
</tbody>
</table>

So, we use **--replacefiles** to convince the newer package to install.
We note that the newer package installed a file on top of the file
originally installed:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -iv --replacefiles cdp-0.33-3.i386.rpm
Installing cdp-0.33-3.i386.rpm

# ls -al /usr/bin/cdp
-rwxr-xr-x   1 root     root        34444 Apr 24 22:37 /usr/bin/cdp

#
            </code></pre></td>
</tr>
</tbody>
</table>

The original `cdp` file, 34,460 bytes long, and dated February 25th, has
been replaced with a file with the same name, but 34,444 bytes long from
the 24th of April. The original file is long gone.

Next, we erased the package we just installed.
[<span class="footnote">\[3\]</span>](#FTN.AEN1144) Finally, we tried to
find the file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -e cdp-0.33-3
# ls -al /usr/bin/cdp
ls: /usr/bin/cdp: No such file or directory

#
            </code></pre></td>
</tr>
</tbody>
</table>

The file is gone. Why is this? The reason is that `/usr/bin/cdp` from
the first package was replaced when the second package was installed
using the **--replacefiles** option. Then, when the second package was
erased, the `/usr/bin/cdp` file was removed, since it belonged to the
second package. If the first package had been erased first, there would
have been no problem, since RPM would have realized that the first
package's file had already been deleted, and would have left the file in
place.

The only problem with this state of affairs is that the first package is
still installed, *except* for `/usr/bin/cdp`. So now there's a partially
installed package on the system. What to do? Perhaps it's time to
exercise your new-found knowledge by issuing an **rpm -i --replacepkgs**
command to fix up the first package…

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-install-nodeps">**--nodeps**: Do Not Check Dependencies Before Installing Package</span>

One day it'll happen. You'll be installing a new package, when suddenly,
the install bombs:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -i blather-7.9-1.i386.rpm
failed dependencies:
        bother &gt;= 3.1 is needed by blather-7.9-1

#
          </code></pre></td>
</tr>
</tbody>
</table>

What happened? The problem is that the package you're installing
requires another package to be installed in order for it to work
properly. In our example, the `blather` package won't work properly
unless the `bother` package (and more specifically, `bother` version 3.1
or later) is installed. Since our system doesn't have an appropriate
version of `bother` installed at all, RPM aborted the installation of
`blather`.

Now, 99 times out of 100, this exactly the right thing for RPM to do.
After all, if the package doesn't have everything it needs to work
properly, why try to install it? Well, as with everything else in life,
there are exceptions to the rule. And that is why there is a
**--nodeps** option.

Adding the **--nodeps** options to an install command directs RPM to
ignore any dependency-related problems and to complete the package
installation. Going back to our example above, let's add the
**--nodeps** option to the command line and see what happens:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -i --nodeps blather-7.9-1.i386.rpm
#
          </code></pre></td>
</tr>
</tbody>
</table>

The package was installed without a peep. Whether it will work properly
is another matter, but it is installed. In general, it's not a good idea
to use **--nodeps** to get around dependency problems. The package
builders included the dependency requirements for a reason, and it's
best not to second-guess them.

</div>

<div class="sect2">

## <span id="s2-rpm-install-force-option">**--force**: The Big Hammer</span>

Adding **--force** to an install command is a way of saying "Install it
anyway\!" In essence, it adds **--replacepkgs** and **--replacefiles**
to the command. Like a big hammer, **--force** is an irresistible force
[<span class="footnote">\[4\]</span>](#FTN.AEN1209) that makes things
happen. In fact, the only thing that will prevent a **--force**'ed
install from proceeding is a dependency conflict.

And like a big hammer, it pays to fully understand why you need to use
**--force** before actually using it.

</div>

<div class="sect2">

## <span id="s2-rpm-install-excludedocs-option">**--excludedocs**: Do Not Install Documentation For This Package</span>

RPM has a number of good features. One of them is the fact that RPM
classifies the files it installs into one of three categories:

1.  Config files.

2.  Files containing documentation.

3.  All other files.

RPM uses the **--excludedocs** option to prevent files classified as
documentation from being installed. In the following example, we know
that the package contains documentation: specifically, the man page,
`/usr/man/man1/cdp.1`. Let's see how **--excludedocs** keeps it from
being installed:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -iv --excludedocs cdp-0.33-3.i386.rpm
Installing cdp-0.33-3.i386.rpm

# ls -al /usr/man/man1/cdp.1
ls: /usr/man/man1/cdp.1: No such file or directory

#
          </code></pre></td>
</tr>
</tbody>
</table>

The primary reason to use **--excludedocs** is to save on disk space.
The savings can be sizeable. For example, on an RPM-installed Linux
system, there can be over 5,000 documentation files, using nearly 50
megabytes.

If you like, you can make **--excludedocs** the default for *all*
installs. To do this, simply add the following line to `/etc/rpmrc`,
`.rpmrc` in your login directory, or the file specified with the
**--rcfile** (which is discussed in [the Section called ***--rcfile
`<rcfile>`**: Use **`<rcfile>`** As An Alternate `rpmrc`
File*](s1-rpm-install-additional-options.html#s2-rpm-install-rcfile))
option:

**excludedocs: 1**

After that, every time an **rpm -i** command is run, it will not install
any documentation files.
[<span class="footnote">\[5\]</span>](#FTN.AEN1261)

</div>

<div class="sect2">

## <span id="s2-rpm-install-includedocs">**--includedocs**: Install Documentation For This Package</span>

As the name implies, **--includedocs** directs RPM to install any files
marked as being documentation. This option is normally not required,
unless the rpmrc file entry "**excludedocs: 1**" is included in the
referenced rpmrc file. Here's an example. Note that in this example,
`/etc/rpmrc` contains "**excludedocs: 1**", which directs RPM not to
install documentation files:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls /usr/man/man1/cdp.1
ls: /usr/man/man1/cdp.1: No such file or directory

# rpm -iv cdp-0.33-3.i386.rpm
Installing cdp-0.33-3.i386.rpm

# ls /usr/man/man1/cdp.1
ls: /usr/man/man1/cdp.1: No such file or directory

#
          </code></pre></td>
</tr>
</tbody>
</table>

Here we've checked to make sure that the cdp man page did not previously
exist on the system. Then after installing the cdp package, we find that
the "**excludedocs: 1**" in `/etc/rpmrc` did its job: the man page
wasn't installed. Let's try it again, this time adding the
**--includedocs** option:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls /usr/man/man1/cdp.1
ls: /usr/man/man1/cdp.1: No such file or directory

# rpm -iv --includedocs cdp-0.33-3.i386.rpm
Installing cdp-0.33-3.i386.rpm

# ls /usr/man/man1/cdp.1
-rw-r--r--   1 root     root         4550 Apr 24 22:37 /usr/man/man1/cdp.1

#
          </code></pre></td>
</tr>
</tbody>
</table>

The **--includedocs** option overrode the rpmrc file's "**excludedocs:
1**" entry, causing RPM to install the documentation file.

</div>

<div class="sect2">

## <span id="s2-rpm-install-prefix">**--prefix `<path>`**: Relocate the package to **`<path>`**, if possible</span>

Some packages give the person installing them flexibility in determining
where on their system they should be installed. These are known as
relocatable packages. A relocatable package differs from a package that
cannot be relocated, in only one way — the definition of a default
prefix. Because of this, it takes a bit of additional effort to
determine if a package is relocatable. But here's an RPM command that
can be used to find out:
[<span class="footnote">\[6\]</span>](#FTN.AEN1314)

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>rpm -qp --queryformat &quot;%{defaultprefix}\n&quot; &lt;packagefile&gt;
          </code></pre></td>
</tr>
</tbody>
</table>

Just replace `<packagefile>` with the name of the package file you want
to check out. If the package is not relocatable, you'll only see the
word (none). If, on the other hand, the command displays a path, that
means the package is relocatable. Unless specified otherwise, every file
in the package will be installed somewhere below the path specified by
the default prefix.

What if you want to specify otherwise? Easy: just use the **--prefix**
option. Let's give it a try:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qp --queryformat &quot;%{defaultprefix}\n&quot; cdplayer-1.0-1.i386.rpm
/usr/local

# rpm -i --prefix /tmp/test cdplayer-1.0-1.i386.rpm
#
          </code></pre></td>
</tr>
</tbody>
</table>

Here we've used our magic query command to determine that the `cdplayer`
package is relocatable. It normally installs below `/usr/local`, but we
wanted to move it around. By adding the **--prefix** option, we were
able to make the package install in `/tmp/test`. If we take a look
there, we'll see that RPM created all the necessary directories to hold
the package's files:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -lR /tmp/test/
total 2
drwxr-xr-x   2 root     root         1024 Dec 16 13:21 bin/
drwxr-xr-x   3 root     root         1024 Dec 16 13:21 man/

/tmp/test/bin:
total 41
-rwxr-xr-x   1 root     root        40739 Oct 14 20:25 cdp*
lrwxrwxrwx   1 root     root           17 Dec 16 13:21 cdplay -&gt; /tmp/test/bin/cdp*

/tmp/test/man:
total 1
drwxr-xr-x   2 root     root         1024 Dec 16 13:21 man1/

/tmp/test/man/man1:
total 5
-rwxr-xr-x   1 root     root         4550 Oct 14 20:25 cdp.1*

#
          </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect2">

## <span id="s2-rpm-install-noscripts">**--noscripts**: Do Not Execute Pre- and Post-install Scripts</span>

Before we talk about the **--noscripts** option, we need to cover a bit
of background. In [the Section called *Getting a *lot* more information
with
**-vv***](s1-rpm-install-additional-options.html#s2-rpm-install-vv-option),
we saw some output from an install using the **-vv** option. As can be
seen, there are two lines that mention pre-install and post-install
scripts. When some packages are installed, they may require that certain
programs be executed before, after, or before *and* after the package's
files are copied to disk.
[<span class="footnote">\[7\]</span>](#FTN.AEN1358)

The **--noscripts** option prevents these scripts from being executed
during an install. *This is a very dangerous thing to do\!* The
**--noscripts** option is really meant for package builders to use
during the development of their packages. By preventing the pre- and
post-install scripts from running, a package builder can keep a buggy
package from bringing down their development system. Once the bugs are
found and eliminated, the **--noscripts** option is no longer necessary.

</div>

<div class="sect2">

## <span id="s2-rpm-install-percent">**--percent**: Not Meant for Human Consumption</span>

An option that will probably *never* be very popular is **--percent**.
This option is meant to be used by programs that interact with the user,
perhaps presenting a graphical user interface for RPM. When the
**--percent** option is used, RPM displays a series of numbers. Each
number is a percentage that indicates how far along the install is. When
the number reaches 100%, the installation is complete.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -i --percent iBCS-1.2-3.i386.rpm
%f iBCS:1.2:3
%% 0.002140
%% 1.492386
%% 5.296632
%% 9.310026
%% 15.271010
%% 26.217846
%% 31.216000
%% 100.000000
%% 100.000000

#
          </code></pre></td>
</tr>
</tbody>
</table>

The list of percentages will vary depending on the number of files in
the package, but every package ends at 100% when completely installed.

</div>

<div class="sect2">

## <span id="s2-rpm-install-rcfile">**--rcfile `<rcfile>`**: Use **`<rcfile>`** As An Alternate `rpmrc` File</span>

The **--rcfile** option is used to specify a file containing default
settings for RPM. Normally, this option is not needed. By default, RPM
uses `/etc/rpmrc` and a file named `.rpmrc` located in your login
directory.

This option would be used if there was a need to switch between several
sets of RPM defaults. Software developers and package builders will
normally be the only people using the **--rcfile** option. For more
information on `rpmrc` files, see [Appendix B](ch-rpmrc-file.html).

</div>

<div class="sect2">

## <span id="s2-rpm-install-root">**--root `<path>`**: Use **`<path>`** As An Alternate Root</span>

Adding **--root `<path>`** to an install command forces RPM to assume
that the directory specified by `<path>` is actually the "root"
directory. The **--root** option affects every aspect of the install
process, so pre- and post-install scripts are run with `<path>` as their
root directory (using `chroot(2)`, if you must know). In addition, RPM
expects its database to reside in the directory specified by the
**dbpath** `rpmrc` file entry, relative to `<path>`.
[<span class="footnote">\[8\]</span>](#FTN.AEN1421)

Normally this option is only used during an initial system install, or
when a system has been booted off a "rescue disk" and some packages need
to be re-installed.

</div>

<div class="sect2">

## <span id="s2-rpm-install-dbpath">**--dbpath `<path>`**: Use **`<path>`** To Find RPM Database</span>

In order for RPM to do its handiwork, it needs access to an RPM
database. Normally, this database exists in the directory specified by
the `rpmrc` file entry, **dbpath**. By default, **dbpath** is set to
`/var/lib/rpm`.

Although the `dbpath` entry can be modified in the appropriate `rpmrc`
file, the **--dbpath** option is probably a better choice when the
database path needs to be changed temporarily. An example of a time the
**--dbpath** option would come in handy is when it's necessary to
examine an RPM database copied from another system. Granted, it's not a
common occurrence, but it's difficult to handle any other way.

</div>

<div class="sect2">

## <span id="s2-rpm-install-ftpport">**--ftpport `<port>`**: Use **`<port>`** In FTP-based Installs</span>

Back in [the Section called *URLs — Another Way to Specify Package
Files*](s1-rpm-install-performing-install.html#s2-rpm-install-urls) we
showed how RPM can access package files by the use of a URL. We also
mentioned that some systems may not use the standard FTP port. In those
cases, it's necessary to give RPM the proper port number to use. As we
mentioned above, one approach is to embed the port number in the URL
itself.

Another approach is to use the **--ftpport** option. RPM will access the
desired port when this option, along with the port number, is added to
the command line. In cases where the desired port seldom changes, it may
be entered in an `rpmrc` file by using the **ftpport** entry.
[<span class="footnote">\[9\]</span>](#FTN.AEN1462)

</div>

<div class="sect2">

## <span id="s2-rpm-install-ftpproxy">**--ftpproxy `<host>`**: Use **`<host>`** As Proxy In FTP-based Installs</span>

Many companies and Internet Service Providers (ISPs) employ various
methods to protect their network connections against misuse. One of
these methods is to use a system that will process all FTP requests on
behalf of the other systems on the company or ISP network. By having a
single computer act as a proxy for the other systems, it serves to
protect the other systems against any FTP-related misuse.

When RPM is employed on a network with an FTP proxy system, it will be
necessary for RPM to direct all its FTP requests to the FTP proxy. RPM
will send its FTP requests to the specified proxy system when the
**--ftpproxy** option, along with the proxy hostname, is added to the
command line.

In cases where the proxy host seldom changes, it may be entered in an
`rpmrc` file by using the **ftpproxy** entry.
[<span class="footnote">\[10\]</span>](#FTN.AEN1481)

</div>

<div class="sect2">

## <span id="s2-rpm-install-ignorearch">**--ignorearch**: Do Not Verify Package Architecture</span>

When a package file is created, RPM specifies the architecture, or type
of computer hardware, for which the package was created. This is a good
thing, as the architecture is one of the main factors in determining
whether a package written for one computer is going to be compatible
with another computer.

When a package is installed, RPM uses the **arch\_compat** `rpmrc`
entries in order to determine what are normally considered compatible
architectures. Unless you're porting RPM to a new architecture, you
shouldn't make any changes to these entries.
[<span class="footnote">\[11\]</span>](#FTN.AEN1495) While RPM attempts
to make the right decisions regarding package compatibility, there are
times when it errs on the side of conservatism. In those cases, it's
necessary to override RPM's decision. The **--ignorearch** option is
used in those cases. When added to the command line, RPM will not
perform any architecture-related checking.

Unless you really know what you're doing, you should *never* use
**--ignorearch**\!

</div>

<div class="sect2">

## <span id="s2-rpm-install-ignoreos">**--ignoreos**: Do Not Verify Package Operating System</span>

When a package file is created, RPM specifies the operating system for
which the package was created. This is a good thing as the operating
system is one of the main factors in determining whether a package
written for one computer is going to be compatible with another
computer.

When a package is installed, RPM uses the **os\_compat** `rpmrc` entries
to determine what are normally considered compatible operating systems.
Unless you're porting RPM to a new operating system, you shouldn't make
any changes to these entries.
[<span class="footnote">\[12\]</span>](#FTN.AEN1514) While RPM attempts
to make the right decisions regarding package compatibility, there are
times when it errs on the side of conservatism. In those cases, it's
necessary to override RPM's decision. The **--ignoreos** option is used
in those cases. When added to the command line, RPM will not perform any
operating system-related checking.

Unless you really know what you're doing, you should *never* use
**--ignoreos**\!

</div>

</div>

### Notes

|                                                                                        |                                                                                                                                                                                                                                                                          |
| -------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [<span class="footnote">\[1\]</span>](s1-rpm-install-additional-options.html#AEN970)   | We'll get more into this aspect of RPM in [the Section called ***rpm -V** — What Does it Do?* in Chapter 6](ch-rpm-verify.html#s1-rpm-verify-what-it-does) when we discuss **rpm -V**.                                                                                   |
| [<span class="footnote">\[2\]</span>](s1-rpm-install-additional-options.html#AEN1054)  | You'll have to do that yourself\!                                                                                                                                                                                                                                        |
| [<span class="footnote">\[3\]</span>](s1-rpm-install-additional-options.html#AEN1144)  | For more information on erasing packages with **rpm -e**, see [Chapter 3](ch-rpm-erase.html).                                                                                                                                                                            |
| [<span class="footnote">\[4\]</span>](s1-rpm-install-additional-options.html#AEN1209)  | No pun intended.                                                                                                                                                                                                                                                         |
| [<span class="footnote">\[5\]</span>](s1-rpm-install-additional-options.html#AEN1261)  | For more information on rpmrc files, refer to [Appendix B](ch-rpmrc-file.html).                                                                                                                                                                                          |
| [<span class="footnote">\[6\]</span>](s1-rpm-install-additional-options.html#AEN1314)  | We discuss RPM's query commands in [Chapter 5](ch-rpm-query.html).                                                                                                                                                                                                       |
| [<span class="footnote">\[7\]</span>](s1-rpm-install-additional-options.html#AEN1358)  | It's possible to use RPM's query command to see if a package has pre- or post-install scripts. See [the Section called ***--scripts** — Show Scripts Associated With a Package* in Chapter 5](s1-rpm-query-parts.html#s3-rpm-query-scripts-option) for more information. |
| [<span class="footnote">\[8\]</span>](s1-rpm-install-additional-options.html#AEN1421)  | For more information on `rpmrc` file entries, see [Appendix B](ch-rpmrc-file.html).                                                                                                                                                                                      |
| [<span class="footnote">\[9\]</span>](s1-rpm-install-additional-options.html#AEN1462)  | The use of `rpmrc` files is described in [Appendix B](ch-rpmrc-file.html).                                                                                                                                                                                               |
| [<span class="footnote">\[10\]</span>](s1-rpm-install-additional-options.html#AEN1481) | The use of `rpmrc` files is described in [Appendix B](ch-rpmrc-file.html).                                                                                                                                                                                               |
| [<span class="footnote">\[11\]</span>](s1-rpm-install-additional-options.html#AEN1495) | If you *are* porting RPM, you'll find more on **arch\_compat** in [the Section called ***`xxx`\_compat** — Define Compatible Architectures* in Chapter 19](s1-rpm-multi-build-install-detection.html#s3-rpm-multi-xxx-compat).                                           |
| [<span class="footnote">\[12\]</span>](s1-rpm-install-additional-options.html#AEN1514) | If you *are* porting RPM, you'll find more on **os\_compat** in [the Section called ***`xxx`\_compat** — Define Compatible Architectures* in Chapter 19](s1-rpm-multi-build-install-detection.html#s3-rpm-multi-xxx-compat).                                             |

<div class="NAVFOOTER">

-----

|                                           |                           |                             |
| :---------------------------------------- | :-----------------------: | --------------------------: |
| [Prev](s1-rpm-install-handy-options.html) |    [Home](index.html)     |   [Next](ch-rpm-erase.html) |
| Two handy options                         | [Up](ch-rpm-install.html) | Using RPM to Erase Packages |

</div>
