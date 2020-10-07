<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-miscellania.md)

Chapter 8. Miscellanea

[Next](s1-rpm-miscellania-srpms.md)

-----

<div class="sect1">

# <span id="s1-rpm-miscellania-rpm2cpio">Using **rpm2cpio**</span>

From time to time, you might find it necessary to extract one or more
files from a package file. One way to do this would be to:

  - Install the package

  - Make a copy of the file(s) you need

  - Erase the package

An easier way would be to use **rpm2cpio**.

<div class="sect2">

## <span id="s2-rpm-miscellania-rpm2cpio-what-it-does">**rpm2cpio** — What does it do?</span>

As the name implies, **rpm2cpio** takes an RPM package file and converts
it to a **cpio** archive. Because it's written to be used primarily as a
filter, there's not much to be specified. **rpm2cpio** takes only only
one argument, and even *that's* optional\!

The optional argument is the name of the package file to be converted.
If there is no filename specified on the command line, **rpm2cpio** will
simply read from standard input and convert *that* to a **cpio**
archive. Let's give it a try:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm2cpio logrotate-1.0-1.i386.rpm
0707020001a86a000081a4000000000000000000000001313118bb000002c200000008000
000030000000000000000000000190000e73eusr/man/man8/logrotate.8.&quot; logrotate
 - log fi
le rotator
.TH rpm 8 &quot;28 November 1995&quot; &quot;Red Hat Software&quot; &quot;Red Hat Linux&quot;
.SH NAME

          </code></pre></td>
</tr>
</tbody>
</table>

(We've just shown the first few lines of output.)

What on earth is all that stuff? Remember, **rpm2cpio** is written as a
filter. It writes the **cpio** archive contained in the package file to
standard output, which, if you've not redirected it somehow, is your
screen. Here's a more reasonable example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm2cpio logrotate-1.0-1.i386.rpm &gt; blah.cpio
# file blah.cpio
blah.cpio: ASCII cpio archive (SVR4 with CRC)

#
          </code></pre></td>
</tr>
</tbody>
</table>

Here we've directed **rpm2cpio** to convert the `logrotate` package
file. We've also redirected **rpm2cpio**'s output to a file called
`blah.cpio`. Next, using the **file** command, we find that the
resulting file is indeed a true-blue **cpio** archive file. The
following command is entirely equivalent to the one above and shows
**rpm2cpio**'s ability to read the package file from its standard input:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># cat logrotate-1.0-1.i386.rpm | rpm2cpio &gt; blah.cpio
#
          </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect2">

## <span id="s2-rpm-miscellania-rpm2cpio-listing-files">A more real-world example — Listing the files in a package file</span>

While there's nothing wrong with using **rpm2cpio** to actually create a
**cpio** archive file, it's takes a few more steps and uses a bit more
disk space than is strictly necessary. A somewhat cleaner approach would
be to pipe **rpm2cpio**'s output directly into **cpio**:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm2cpio logrotate-1.0-1.i386.rpm  | cpio -t
usr/man/man8/logrotate.8
usr/sbin/logrotate
14 blocks

#
          </code></pre></td>
</tr>
</tbody>
</table>

In this example, we used the **-t** option to direct **cpio** to produce
a "table of contents" of the archive created by **rpm2cpio**. This can
make it much easier to get the right filename and path when you want to
extract a file.

</div>

<div class="sect2">

## <span id="s2-rpm-miscellania-rpm2cpio-extract">Extracting one or more files from a package file</span>

Continuing the example above, let's extract the man page from the
`logrotate` package. In the table of contents, we see that the full path
is `usr/man/man8/logrotate.8`. All we need to do is to use the filename
and path as shown below:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm2cpio logrotate-1.0-1.i386.rpm |cpio -ivd usr/man/man8/logrotate.8
usr/man/man8/logrotate.8
14 blocks

#
          </code></pre></td>
</tr>
</tbody>
</table>

In this case, the **cpio** options **-i**, **-v**, and **-d** direct
**cpio** to:

  - Extract one or more files from an archive.

  - Display the names of any files processed, along with the size of the
    archive file, in 512-byte blocks.
    [<span class="footnote">\[1\]</span>](#FTN.AEN5057)

  - Create any directories that precede the filename specified in the
    **cpio** command.

So where did the file end up? The last option (**-d**) to **cpio**
provides a hint. Let's take a look:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -al
total 5
-rw-rw-r--   1 root     root         3918 May 30 11:02 logrotate-1.0-1.i386.rpm
drwx------   3 root     root         1024 Jul 14 12:42 usr

# cd usr
# ls -al
total 1
drwx------   3 root     root         1024 Jul 14 12:42 man

# cd man
# ls -al
total 1
drwx------   2 root     root         1024 Jul 14 12:42 man8

# cd man8
# ls -al
total 1
-rw-r--r--   1 root     root          706 Jul 14 12:42 logrotate.8

# cat logrotate.8
.\&quot; logrotate - log file rotator
.TH rpm 8 &quot;28 November 1995&quot; &quot;Red Hat Software&quot; &quot;Red Hat Linux&quot;
.SH NAME
logrotate \- log file rotator
.SH SYNOPSIS
\fBlogrotate\fP [configfiles] 
.SH DESCRIPTION
\fBlogrotate\fP is a tool to prevent log files from growing without
…

#
          </code></pre></td>
</tr>
</tbody>
</table>

Since the current directory didn't have a `usr/man/man8/` path in it,
the **-d** option caused **cpio** to create all the directories leading
up to the `logrotate.8` file in the current directory. Based on this,
it's probably safest to use **cpio** *outside* the normal system
directories unless you're comfortable with **cpio**, and you know what
you're doing\!

</div>

</div>

### Notes

|                                                                                 |                                                                                                        |
| ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| [<span class="footnote">\[1\]</span>](s1-rpm-miscellania-rpm2cpio.md#AEN5057) | Note that the size displayed by **cpio** is the size of the **cpio** archive and not the package file. |

<div class="NAVFOOTER">

-----

|                                 |                               |                                          |
| :------------------------------ | :---------------------------: | ---------------------------------------: |
| [Prev](ch-rpm-miscellania.md) |      [Home](index.md)       |    [Next](s1-rpm-miscellania-srpms.md) |
| Miscellanea                     | [Up](ch-rpm-miscellania.md) | Source Package Files and How To Use Them |

</div>
