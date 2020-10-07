<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-verify-output.html)

Chapter 6. Using RPM to Verify Installed Packages

[Next](s1-rpm-verify-we-lied.html)

-----

<div class="sect1">

# <span id="s1-rpm-verify-what-to-verify">Selecting What to Verify, and How</span>

There are several ways to verify packages installed on your system. If
you've taken a look at RPM's query command, you'll find that many of
them are similar. Let's start with the simplest method of specifying
packages — the package label.

<div class="sect2">

## <span id="s2-rpm-verify-package-label">The Package Label — Verify an Installed Package Against the RPM Database</span>

You can simply follow the **rpm -V** command with all or part of a
package label. As with every other RPM command that accepts package
labels, you'll need to carefully specify each part of the label you
include. Keep in mind that package names are case-sensitive, so **rpm -V
PackageName** and **rpm -V packagename** are *not* the same. Let's
verify the `initscripts` package:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -V initscripts
# 
          </code></pre></td>
</tr>
</tbody>
</table>

While it looks like RPM didn't do anything, the following steps were
performed:

  - For every file in the package, RPM checked the nine file attributes
    that were discussed above.

  - If the package was built with dependencies, the RPM database was
    searched to ensure the packages that satisfy those dependencies were
    installed.

  - If the package was built with a verification script, that script was
    executed.

In our example, each of these steps was performed without error — the
package verified successfully. Remember, with **rpm -V** you'll only see
output if a package fails to verify.

</div>

<div class="sect2">

## <span id="s2-rpm-verify-a-option">**-a** — Verify All Installed Packages Against the RPM Database</span>

If you add **-a** to **rpm -V**, you can easily verify every installed
package on your system. It might take a while, but when it's done,
you'll know exactly what's been changed on your system:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Va
.M5....T   /usr/X11R6/lib/X11/fonts/misc/fonts.dir
missing    /var/spool/at/.lockfile
missing    /var/spool/at/spool
S.5....T   /usr/lib/rhs/glint/icon.pyc
..5....T c /etc/inittab
..5.....   /usr/bin/loadkeys

#
          </code></pre></td>
</tr>
</tbody>
</table>

Don't be too surprised if **rpm -Va** turns up a surprising number of
files that failed verification. RPM's verification process is *very*
strict\! In many cases, the changes flagged don't indicate problems —
they are only an indication of your system's configuration being
different than what the builders of the installed packages had on
*their* system. Also, some attributes change during normal system
operation. However, it would be wise to check into each verification
failure, just to make sure.

</div>

<div class="sect2">

## <span id="s2-rpm-verify-f-option">**-f `<file>`** — Verify the Package Owning **`<file>`** Against the RPM Database</span>

Imagine this: you're hard at work when a program you've used a million
times before suddenly stops working. What do you do? Well, before using
RPM, you probably tried to find other files associated with that program
and see if they had changed recently.

Now you can let RPM do at least part of that sleuthing for you. Simply
direct RPM to verify the package owning the ailing program:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>% rpm -Vf /sbin/cardmgr
S.5....T c /etc/sysconfig/pcmcia

% 
          </code></pre></td>
</tr>
</tbody>
</table>

Hmmmm. Looks like a config file was recently changed.

This isn't to say that using RPM to verify a package will always get you
out of trouble, but it's such a quick step it should be one of the first
things you try. Here's an example of **rpm -Vf** not working out as
well:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>% rpm -Vf /etc/blunder
file /etc/blunder is not owned by any package

% 
          </code></pre></td>
</tr>
</tbody>
</table>

(Note that the issue surrounding RPM and symbolic links mentioned in
[the Section called *A Tricky Detail* in Chapter
5](s1-rpm-query-parts.html#s4-rpm-query-tricky-detail) also applies to
**rpm -Vf**. Watch those symlinks\!)

</div>

<div class="sect2">

## <span id="s2-rpm-verify-p-option">**-p `<file>`** — Verify Against a Specific Package File</span>

Unlike the previous options to **rpm -V**, each of which verified one or
more packages against RPM's database, the **-p** option performs the
same verification, but against a package file. Why on earth would you
want to do this when the RPM database is sitting there just waiting to
be used?

Well, what if you didn't *have* an RPM database? While it isn't a common
occurrence, power failures, hardware problems, and inadvertent deletions
(along with non-existent backups) can leave your system "sans database".
Then your system hiccups — what do you do now?

This is where a CD full of package files can be worth its weight in
gold. Simply mount the CD and verify to your heart's content:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Vp /mnt/cdrom/RedHat/RPMS/i386/adduser-1.1-1.i386.rpm
#
          </code></pre></td>
</tr>
</tbody>
</table>

Whatever else might be wrong with this system, at least we can add new
users. But what if you have *many* packages to verify? It would be a
very slow process doing it one package at a time. That's where the next
option comes in handy…

</div>

<div class="sect2">

## <span id="s2-rpm-verify-g-option">**-g `<group>`** — Verify Packages Belonging To **`<group>`**</span>

When a package is built, the package builder must classify the package,
grouping it with other packages that perform similar functions. RPM
gives you the ability to verify installed packages based on their
groups. For example, there is a group known as `Shells`. This group
consists of packages that contain, strangely enough, shells. Let's
verify the proper installation of every shell-related package on the
system:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Vg Shells
missing    /etc/bashrc

#
          </code></pre></td>
</tr>
</tbody>
</table>

One thing to keep in mind is that group specifications are
case-sensitive. Issuing the command **rpm -Vg shells** wouldn't verify
many packages:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Vg shells
group shells does not contain any packages

#
          </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect2">

## <span id="s2-rpm-verify-nodeps-option">**--nodeps**: Do Not Check Dependencies During Verification</span>

When the **--nodeps** option is added to a verify command, RPM will
bypass its dependency verification processing. In this example, we've
added the **-vv** option to so we can watch RPM at work:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Vvv rpm
D: opening database in //var/lib/rpm/
D: verifying record number 2341208
D: dependencies: looking for libz.so.1
D: dependencies: looking for libdb.so.2
D: dependencies: looking for libc.so.5

#
          </code></pre></td>
</tr>
</tbody>
</table>

As we can see, there are three different capabilities that the **rpm**
package requires:

  - `libz.so.1`

  - `libdb.so.2`

  - `libc.so.5`

If we add the **--nodeps** option, the dependency verification of the
three capabilities is no longer performed:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Vvv --nodeps rpm
D: opening database in //var/lib/rpm/
D: verifying record number 2341208

#
          </code></pre></td>
</tr>
</tbody>
</table>

The line D: verifying record number 2341208 indicates that RPM's normal
file-based verification proceeded normally.

</div>

<div class="sect2">

## <span id="s2-rpm-verify-noscripts-option">**--noscripts**: Do Not Execute Verification Script</span>

Adding the **--noscripts** option to a verify command prevents execution
of the verification scripts of each package being verified. In the
following example, the package verification script is executed:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Vvv bother
D: opening database in //var/lib/rpm/
D: verifying record number 616728
D: verify script found - running from file /var/tmp/rpm-321.vscript
+ PATH=/sbin:/bin:/usr/sbin:/usr/bin:/usr/X11R6/bin
+ export PATH
+ echo This is the bother 3.5 verification script
This is the bother 3.5 verification script

#
          </code></pre></td>
</tr>
</tbody>
</table>

While the actual script is not very interesting, it did execute when the
package was being verified. In the next example, we'll use the
**--noscripts** option to prevent its execution:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Vvv --noscripts bother
D: opening database in //var/lib/rpm/
D: verifying record number 616728

#
          </code></pre></td>
</tr>
</tbody>
</table>

As expected, the output is identical to the prior example — minus the
lines dealing with the verification script, of course.

</div>

<div class="sect2">

## <span id="s2-rpm-verify-nofiles-option">**--nofiles**: Do Not Verify File Attributes</span>

The **--nofiles** option disables RPM's file-related verification
processing. When this option is used, only the verification script and
dependency verification processing are performed. In this example, the
package has a file-related verification problem:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Vvv bash
D: opening database in //var/lib/rpm/
D: verifying record number 279448
D: dependencies: looking for libc.so.5
D: dependencies: looking for libtermcap.so.2
missing    /etc/bashrc

#
          </code></pre></td>
</tr>
</tbody>
</table>

When the **--nofiles** option is added, the missing file doesn't cause a
message any more:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Vvv --nofiles bash
D: opening database in //var/lib/rpm/
D: verifying record number 279448
D: dependencies: looking for libc.so.5
D: dependencies: looking for libtermcap.so.2

#
          </code></pre></td>
</tr>
</tbody>
</table>

This is not to say that the missing file problem is solved, just that no
file verification was performed.

</div>

<div class="sect2">

## <span id="s2-rpm-verify-v-option">**-v** — Display Additional Information</span>

Although RPM won't report an error with the command syntax if you
include the **-v** option, you won't see much in the way of additional
output:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Vv bash
#
          </code></pre></td>
</tr>
</tbody>
</table>

Even if there are verification errors, adding **-v** won't change the
output:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Vv apmd
S.5....T   /etc/rc.d/init.d/apm
S.5....T   /usr/X11R6/bin/xapm

#
          </code></pre></td>
</tr>
</tbody>
</table>

The only time that the **-v** option *will* produce output is when the
package being verified has a verification script. Any normal output from
the script won't be displayed by RPM, when run without **-v**:
[<span class="footnote">\[1\]</span>](#FTN.AEN4311)

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -V bother
#
          </code></pre></td>
</tr>
</tbody>
</table>

But when **-v** is added, the script's non-error-related output is
displayed:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Vv bother
This is the bother 3.5 verification script

#
          </code></pre></td>
</tr>
</tbody>
</table>

If you're looking for more insight into RPM's inner workings, you'll
have to try the next option:

</div>

<div class="sect2">

## <span id="s2-rpm-verify-vv-option">**-vv** — Display Debugging Information</span>

Sometimes it's necessary to have even *more* information than we can get
with **-v**. By adding another **v**, that's just what we'll get:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Vvv rpm
D: opening database in //var/lib/rpm/
D: verifying record number 2341208
D: dependencies: looking for libz.so.1
D: dependencies: looking for libdb.so.2
D: dependencies: looking for libc.so.5

#
          </code></pre></td>
</tr>
</tbody>
</table>

The lines starting with D: have been added by using **-vv**. We can see
where the RPM database is located and what record number contains
information on the `rpm-2.3-1` package. Following that is the list of
dependencies that the `rpm` package requires.

In the vast majority of cases, it will not be necessary to use **-vv**.
It is normally used by software engineers working on RPM itself, and the
output can change without notice. However, it's a handy way to gain
insights into RPM.

</div>

<div class="sect2">

## <span id="s2-rpm-verify-dbpath-option">**--dbpath `<path>`**: Use **`<path>`** To Find RPM Database</span>

In order for RPM to do its handiwork, it needs access to an RPM
database. Normally, this database exists in the directory specified by
the `rpmrc` file entry, **dbpath**. By default, **dbpath** is set to
`/var/lib/rpm`.

Although the **dbpath** entry can be modified in the appropriate `rpmrc`
file, the **--dbpath** option is probably a better choice when the
database path needs to be changed temporarily. An example of a time the
**--dbpath** option would come in handy is when it's necessary to
examine an RPM database copied from another system. Granted, it's not a
common occurrence, but it's difficult to handle any other way.

</div>

<div class="sect2">

## <span id="s2-rpm-verify-root-option">**--root `<path>`**: Set Alternate Root to **`<path>`**</span>

Adding **--root `<path>`** to a verify command forces RPM to assume that
the directory specified by **`<path>`** is actually the "root"
directory. In addition, RPM expects its database to reside in the
directory specified by the **dbpath** `rpmrc` file entry, relative to
**`<path>`**. [<span class="footnote">\[2\]</span>](#FTN.AEN4384)

Normally this option is only used during an initial system install, or
when a system has been booted off a "rescue disk", and some packages
need to be re-installed in order to restore normal operation.

</div>

<div class="sect2">

## <span id="s2-rpm-verify-rcfile-option">**--rcfile `<rcfile>`**: Set Alternate `rpmrc` file to **`<rcfile>`**</span>

The **--rcfile** option is used to specify a file containing default
settings for RPM. Normally, this option is not needed. By default, RPM
uses `/etc/rpmrc` and a file named `.rpmrc`, located in your login
directory.

This option would be used if there was a need to switch between several
sets of RPM options. Software developer and package builders will be the
people using **--rcfile**. For more information on `rpmrc` files, see
[Appendix B](ch-rpmrc-file.html).

</div>

</div>

### Notes

|                                                                                  |                                                                                     |
| -------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-verify-what-to-verify.html#AEN4311) | Failure messages will always be displayed.                                          |
| [<span class="footnote">\[2\]</span>](s1-rpm-verify-what-to-verify.html#AEN4384) | For more information on `rpmrc` file entries, see [Appendix B](ch-rpmrc-file.html). |

<div class="NAVFOOTER">

-----

|                                             |                          |                                    |
| :------------------------------------------ | :----------------------: | ---------------------------------: |
| [Prev](s1-rpm-verify-output.html)           |    [Home](index.html)    | [Next](s1-rpm-verify-we-lied.html) |
| When Verification Fails — **rpm -V** Output | [Up](ch-rpm-verify.html) |                 We've Lied to You… |

</div>
