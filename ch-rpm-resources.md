<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-specref-conditionals.html)

[Next](s1-rpm-resources-where-to-talk.html)

-----

<div class="appendix">

# <span id="ch-rpm-resources"></span>Appendix F. RPM-related Resources

There are a number of resources available to help you with RPM, over and
above the RPM **man** page, and this book. Here are some pointers to
them.

<div class="sect1">

# <span id="s1-rpm-resources-where-to-get">Where to Get RPM</span>

Perhaps before asking, Where can I get RPM? it might be better to see if
RPM is already installed on your system. If you have Red Hat Linux on
your system, it's there already. But be sure to check on other systems —
people are porting RPM to different systems every day, and it just might
be there waiting for you.

Here's a quick way to see if RPM is installed on your system:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>% rpm --version
RPM version 4.2

%
      </code></pre></td>
</tr>
</tbody>
</table>

If this command doesn't work, it might be that your path doesn't include
the directory where RPM resides. Check the usual "binary" directories
before declaring RPM a no-show\!

<div class="sect2">

## <span id="s2-rpm-resources-ftp-sites">FTP Sites</span>

If you can't find RPM on your system, you'll have to grab a copy by FTP.
RPM can be found at `ftp.rpm.org`. It is no longer available from
`ftp.redhat.com` since version 2.5.1.

</div>

<div class="sect2">

## <span id="s2-rpm-resources-what-do-i-need">What Do I Need?</span>

Once you find a nearby site with RPM, and have found the directory where
it's kept, you'll notice a variety of files, all starting with "`rpm`".
What are they? Which ones do you need? Here's a representative list,
along with the ways in which each file would be used:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>ftp&gt; ls
227 Entering Passive Mode (66,187,233,245,39,44)
150 Here comes the directory listing.
-rw-r--r--    1 2369   300      79155 Sep 17 21:17 popt-1.7-8x.alpha.rpm
-rw-r--r--    1 2369   300      69704 Sep 17 21:17 popt-1.7-8x.i386.rpm
-rw-r--r--    1 2369   300      88284 Sep 17 21:17 popt-1.7-8x.ia64.rpm
-rw-rw-r--    1 2369   300     574549 Sep 17 20:57 popt-1.7.tar.gz
-rw-r--r--    1 2369   300    2647153 Sep 17 21:17 rpm-4.1-8x.alpha.rpm
-rw-r--r--    1 2369   300    2222224 Sep 17 21:17 rpm-4.1-8x.i386.rpm
-rw-r--r--    1 2369   300    3390500 Sep 17 21:17 rpm-4.1-8x.ia64.rpm
-rw-r--r--    1 2369   300    6469152 Sep 17 21:17 rpm-4.1-8x.src.rpm
-rw-rw-r--    1 2369   300    5670825 Sep 17 20:55 rpm-4.1.i386.tar.gz
-rw-rw-r--    1 2369   300    6494145 Sep 17 19:38 rpm-4.1.tar.gz
-rw-r--r--    1 2369   300      83884 Sep 17 21:17 rpm-build-4.1-8x.alpha.rpm
-rw-r--r--    1 2369   300      79365 Sep 17 21:17 rpm-build-4.1-8x.i386.rpm
-rw-r--r--    1 2369   300      95701 Sep 17 21:17 rpm-build-4.1-8x.ia64.rpm
-rw-r--r--    1 2369   300    3798135 Sep 17 21:17 rpm-devel-4.1-8x.alpha.rpm
-rw-r--r--    1 2369   300    3259143 Sep 17 21:17 rpm-devel-4.1-8x.i386.rpm
-rw-r--r--    1 2369   300    3978604 Sep 17 21:17 rpm-devel-4.1-8x.ia64.rpm
-rw-r--r--    1 2369   300     104653 Sep 17 21:17 rpm-python-4.1-8x.alpha.rpm
-rw-r--r--    1 2369   300      97407 Sep 17 21:17 rpm-python-4.1-8x.i386.rpm
-rw-r--r--    1 2369   300     132830 Sep 17 21:17 rpm-python-4.1-8x.ia64.rpm
226 Directory send OK.

ftp&gt;
        </code></pre></td>
</tr>
</tbody>
</table>

Although the version numbers may change, the types of files kept in this
directory will not. Here's the first group of files:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>-rw-r--r--    1 2369   300      79155 Sep 17 21:17 popt-1.7-8x.alpha.rpm
-rw-r--r--    1 2369   300      69704 Sep 17 21:17 popt-1.7-8x.i386.rpm
-rw-r--r--    1 2369   300      88284 Sep 17 21:17 popt-1.7-8x.ia64.rpm
-rw-r--r--    1 2369   300    2647153 Sep 17 21:17 rpm-4.1-8x.alpha.rpm
-rw-r--r--    1 2369   300    2222224 Sep 17 21:17 rpm-4.1-8x.i386.rpm
-rw-r--r--    1 2369   300    3390500 Sep 17 21:17 rpm-4.1-8x.ia64.rpm

        </code></pre></td>
</tr>
</tbody>
</table>

The files above are the binary package files for RPM version 4.1,
release 8x (intended for Red Hat Linux 8.x), on the Digital Alpha, the
Intel x86, and the Intel IA-64. Note that the version number will change
in time, but the other parts of the file naming convention won't. As
binary package files, they must be installed using RPM. So if you don't
have RPM yet, they won't do you much good.
[<span class="footnote">\[1\]</span>](#FTN.AEN18238)

Let's look at the next file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>-rw-r--r--    1 2369   300    6469152 Sep 17 21:17 rpm-4.1-8x.src.rpm

        </code></pre></td>
</tr>
</tbody>
</table>

This is the source package file for RPM version 4.1, release 8x. Like
the binary packages, the source package requires RPM to install —
therefore, it cannot be used to perform an initial install of RPM. Let's
see what else there is here:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>-rw-r--r--    1 2369   300    3798135 Sep 17 21:17 rpm-devel-4.1-8x.alpha.rpm
-rw-r--r--    1 2369   300    3259143 Sep 17 21:17 rpm-devel-4.1-8x.i386.rpm
-rw-r--r--    1 2369   300    3978604 Sep 17 21:17 rpm-devel-4.1-8x.ia64.rpm

        </code></pre></td>
</tr>
</tbody>
</table>

The files above are binary package files that contain the `rpm-devel`
subpackage. The `rpm-devel` package contains header files and the RPM
library, and is used for developing programs that can perform
RPM-related functions. These files cannot be used to get RPM running.
That leaves two files left:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>-rw-rw-r--  1 root  97  278620 Jul 18 06:05 rpm-2.2.2-1.i386.cpio.gz
-rw-rw-r--  1 root  97  356943 Jul 18 06:05 rpm-2.2.2.tar.gz

        </code></pre></td>
</tr>
</tbody>
</table>

The first file is a gzipped **cpio** archive of the files comprising
RPM. After uncompressing the file, **cpio** can be used to extract the
files and place them on your system. Note, however, that there is a
**cpio** archive for the i386 architecture only. To extract the files,
issue the following command:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># zcat file.cpio.gz | (cd / ; cpio --extract)
#
        </code></pre></td>
</tr>
</tbody>
</table>

(When actually issuing the command, `file.cpio.gz` should be replaced
with the actual name of the **cpio** archive.)

Note that the archive should be extracted using GNU **cpio** version
2.4.1 or greater. It may also be necessary to issue the following
command prior to using RPM:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># mkdir /var/lib/rpm
#
        </code></pre></td>
</tr>
</tbody>
</table>

The last file, `rpm-2.2.2.tar.gz`, contains the sources for RPM. Using
it, you can build RPM from scratch. This is the most involved option,
but it is the only choice for people interested in porting RPM to a new
architecture. See [Chapter 8](ch-rpm-miscellania.html) for an example of
RPM being built from the sources.

</div>

</div>

</div>

### Notes

|                                                                       |                                                                                                                                                                                                                                                                        |
| --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](ch-rpm-resources.html#AEN18238) | If your goal is to install RPM on one of these systems, it might be a good idea to copy the appropriate binary package. That way, once you have RPM running, you can reinstall it with the **--force** option to ensure that RPM is properly installed and configured. |

<div class="NAVFOOTER">

-----

|                                          |                    |                                             |
| :--------------------------------------- | :----------------: | ------------------------------------------: |
| [Prev](s1-rpm-specref-conditionals.html) | [Home](index.html) | [Next](s1-rpm-resources-where-to-talk.html) |
| Conditionals                             | [Up](p14028.html)  |                     Where to Talk About RPM |

</div>
