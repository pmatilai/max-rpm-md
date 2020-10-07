<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-checksig-using-rpm-k.html)

[Next](s1-rpm-miscellania-rpm2cpio.html)

-----

<div class="chapter">

# <span id="ch-rpm-miscellania"></span>Chapter 8. Miscellanea

As with any other large, complex subject, there are always some
leftovers — things that just don't seem to fit in any one category. RPM
is no exception. This chapter covers those aspects of RPM that can only
be called "miscellanea"…

<div class="sect1">

# <span id="s1-rpm-miscellania-other-options">Other RPM Options</span>

The following options are not normally used on a day to day basis.
However, some of them can be quite important when the need arises. One
such option is **--rebuilddb**.

<div class="sect2">

## <span id="s2-rpm-miscellania-rebuilddb">**--rebuilddb** — Rebuild RPM database</span>

We all hope the day never comes, and for many of us, it never does. But
still, there is a chance that one day, while you're busy using RPM to
install or upgrade a package, you'll see this message:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>free list corrupt (42)- contact rpm-list@redhat.com

          </code></pre></td>
</tr>
</tbody>
</table>

Once this happens, you'll find there's very little that you can do,
RPM-wise. However, before you fire off an e-mail to the RPM mailing
list, you might try the **--rebuilddb** option. The format of the
command is simple:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>rpm --rebuilddb

          </code></pre></td>
</tr>
</tbody>
</table>

The command produces no output, either. After a few minutes, it
completes with nary a peep. Here's an example of **--rebuilddb** being
used on an RPM database that wasn't corrupt. First, let's look at the
files that comprise the database:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># cd /var/lib/rpm
# ls
total 3534
-rw-r--r--   1 root     root      1351680 Oct 17 10:35 fileindex.rpm
-rw-r--r--   1 root     root        16384 Oct 17 10:35 groupindex.rpm
-rw-r--r--   1 root     root        16384 Oct 17 10:35 nameindex.rpm
-rw-r--r--   1 root     root      2342536 Oct 17 10:35 packages.rpm
-rw-r--r--   1 root     root        16384 Oct 17 10:35 providesindex.rpm
-rw-r--r--   1 root     root        16384 Oct 17 10:35 requiredby.rpm

#
          </code></pre></td>
</tr>
</tbody>
</table>

Then, we issue the command:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm --rebuilddb
#
          </code></pre></td>
</tr>
</tbody>
</table>

After a few minutes, the command completes, and we take a look at the
files again:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls
total 3531
-rw-r--r--   1 root     root      1351680 Oct 17 20:50 fileindex.rpm
-rw-r--r--   1 root     root        16384 Oct 17 20:50 groupindex.rpm
-rw-r--r--   1 root     root        16384 Oct 17 20:50 nameindex.rpm
-rw-r--r--   1 root     root      2339080 Oct 17 20:50 packages.rpm
-rw-r--r--   1 root     root        16384 Oct 17 20:50 providesindex.rpm
-rw-r--r--   1 root     root        16384 Oct 17 20:50 requiredby.rpm

# 
          </code></pre></td>
</tr>
</tbody>
</table>

You'll note that `packages.rpm` decreased in size. This is due to a
side-effect of the **--rebuilddb** option — While it is going through
the database, it is getting rid of unused portions of the database. Our
example was performed on a newly installed system where only one or two
packages had been upgraded, so the reduction in size was small. For a
system that has been through a complete upgrade, the difference would be
more dramatic.

Does this mean that you should rebuild the database every once in a
while? Not really. Since RPM eventually will make use of the holes,
there's no major advantage to regular rebuilds. However, when an
RPM-based system has undergone a major upgrade, it certainly wouldn't
hurt to spend a few minutes using **--rebuilddb** to clean things up.

</div>

<div class="sect2">

## <span id="s2-rpm-miscellania-initdb-option">**--initdb** — Create a New RPM Database</span>

If you are already using RPM, the **--initdb** option is one you'll
probably never have to use. The **--initdb** option is used to create a
new RPM database. That's why you'll probably not need it if you're
already using RPM — you already have an RPM database.

It might seem that the **--initdb** option would be dangerous. After
all, won't it trash your current database if you mistakenly use it?
Fortunately, the answer is no. If there is an RPM database in place
already, it's still perfectly safe to use the option, even though it
won't accomplish much. As an example, here's a listing of the files that
make up the RPM database on a Red Hat Linux system:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls /var/lib/rpm
total 3559
-rw-r--r--   1 root     root        16384 Jan  8 22:10 conflictsindex.rpm
-rw-r--r--   1 root     root      1351680 Jan  8 22:10 fileindex.rpm
-rw-r--r--   1 root     root        16384 Jan  8 22:10 groupindex.rpm
-rw-r--r--   1 root     root        16384 Jan  8 22:10 nameindex.rpm
-rw-r--r--   1 root     root      2349640 Jan  8 22:10 packages.rpm
-rw-r--r--   1 root     root        16384 Jan  8 22:10 providesindex.rpm
-rw-r--r--   1 root     root        16384 Jan  8 22:10 requiredby.rpm

#
          </code></pre></td>
</tr>
</tbody>
</table>

Next, let's use the **--initdb** option, just to see what it does to
this database:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm --initdb
# ls /var/lib/rpm
total 3559
-rw-r--r--   1 root     root        16384 Jan  8 22:10 conflictsindex.rpm
-rw-r--r--   1 root     root      1351680 Jan  8 22:10 fileindex.rpm
-rw-r--r--   1 root     root        16384 Jan  8 22:10 groupindex.rpm
-rw-r--r--   1 root     root        16384 Jan  8 22:10 nameindex.rpm
-rw-r--r--   1 root     root      2349640 Jan  8 22:10 packages.rpm
-rw-r--r--   1 root     root        16384 Jan  8 22:10 providesindex.rpm
-rw-r--r--   1 root     root        16384 Jan  8 22:10 requiredby.rpm

# 
          </code></pre></td>
</tr>
</tbody>
</table>

Since an RPM database existed already, the **--initdb** option did no
harm to it — there was no change to the database files.

The only other option that can be used with **--initdb** is
**--dbpath**. This permits the easy creation of a new RPM database in
the directory specified with the **--dbpath** option.

</div>

<div class="sect2">

## <span id="s2-rpm-miscellania-quiet-option">**--quiet** — Produce as little output as possible</span>

Adding the **--quiet** option to any RPM command directs RPM to produce
as little output as possible. For example, RPM's build command (the
subject of the second half of this book) normally produces reams of
output; by adding the **--quiet** option, this is all you'll see:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba --quiet bother-3.5.spec
* Package: bother
1 block
3 blocks

#
          </code></pre></td>
</tr>
</tbody>
</table>

The **--quiet** option can silence even the mighty **-vv** option:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Uvv --quiet eject-1.2-2.i386.rpm
#
          </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect2">

## <span id="s2-rpm-miscellania-help-option">**--help** — Display a help message</span>

RPM includes a concise built-in help message for those times when you
need a reminder about a particular command. Normally you'll want to use
the **--help** option by itself, though you might want to pipe the
output through a pager such as **less**, since the output is more than
one screen long:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm --help|less
RPM version 2.3
Copyright (C) 1995 - Red Hat Software
This may be freely redistributed under the terms of the GNU Public License

usage:
   --help               - print this message
   --version    - print the version of rpm being used
   all modes support the following arguments:
      --rcfile &lt;file&gt;   - use &lt;file&gt; instead of /etc/rpmrc and $HOME/.rpmrc
       -v                     - be a little more verbose
       -vv                    - be incredibly verbose (for debugging)
   -q                   - query mode
      --root &lt;dir&gt;        - use &lt;dir&gt; as the top level directory
      --dbpath &lt;dir&gt;      - use &lt;dir&gt; as the directory for the database
      --queryformat &lt;s&gt;   - use s as the header format (implies -i)
   install, upgrade and query (with -p) allow ftp URL&#39;s to be used in place
   of file names as well as the following options:

      --ftpproxy &lt;host&gt;   - hostname or IP of ftp proxy

      --ftpport &lt;port&gt;    - port number of ftp server (or proxy)

          </code></pre></td>
</tr>
</tbody>
</table>

This is just the first screen of RPM's help command. To see the rest,
give the command a try. Practically everything there is to know about
RPM is present in the **--help** output. It's a bit too concise to learn
RPM from, but it's enough to refresh your memory when the syntax of a
particular option escapes you.

</div>

<div class="sect2">

## <span id="s2-rpm-miscellania-version-option">**--version** — Display the current RPM version</span>

If you're not sure what version of RPM is presently installed on your
system, the easiest way to find out is to ask RPM itself using the
**--version** option:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm --version
RPM version 2.3

#
          </code></pre></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div class="NAVFOOTER">

-----

|                                          |                    |                                          |
| :--------------------------------------- | :----------------: | ---------------------------------------: |
| [Prev](s1-rpm-checksig-using-rpm-k.html) | [Home](index.html) | [Next](s1-rpm-miscellania-rpm2cpio.html) |
| Using **rpm -K**                         |  [Up](p108.html)   |                       Using **rpm2cpio** |

</div>
