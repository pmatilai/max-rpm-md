<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-query-parts.html)

Chapter 5. Getting Information About Packages

[Next](ch-rpm-verify.html)

-----

<div class="sect1">

# <span id="s1-rpm-query-handy-queries">A Few Handy Queries</span>

Below are some examples of situations you might find yourself in, and
ways you can use RPM to get the information you need. Keep in mind that
these are just examples. Don't be afraid to experiment\!

<div class="sect2">

## <span id="s2-rpm-query-finding-config-files">Finding Config Files Based on a Program Name</span>

You're setting up a new system, and you'd like to implement some
system-wide aliases for people using the Bourne Again SHell, **bash**.
The problem is you just can't remember the name of the system-wide
initialization file used by **bash**, or where it resides:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qcf /bin/bash
/etc/bashrc

#
          </code></pre></td>
</tr>
</tbody>
</table>

Rather than spending time trying to hunt down the file, RPM finds it for
you in seconds.

</div>

<div class="sect2">

## <span id="s2-rpm-query-uninstalled-packages">Learning More About an Uninstalled Package</span>

Practically any option can be combined with **-qp** to extract
information from a .rpm file. Let's say you have an unknown .rpm file,
and you'd like to know a bit more before installing it:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qpil foo.bar
Name        : rpm                  Distribution: Red Hat Linux Vanderbilt
Version     : 2.3                        Vendor: Red Hat Software
Release     : 1                      Build Date: Tue Dec 24 09:07:59 1996
Install date: (none)                 Build Host: porky.redhat.com
Group       : Utilities/System       Source RPM: rpm-2.3-1.src.rpm
Size        : 631157
Summary     : Red Hat Package Manager
Description :
RPM is a powerful package manager, which can be used to build, install,
query, verify, update, and uninstall individual software packages. A
package consists of an archive of files, and package information,
including name, version, and description.
/bin/rpm
/usr/bin/find-provides
/usr/bin/find-requires
/usr/bin/gendiff
/usr/bin/rpm2cpio
/usr/doc/rpm-2.3-1
…
/usr/src/redhat/SOURCES
/usr/src/redhat/SPECS
/usr/src/redhat/SRPMS

# 
          </code></pre></td>
</tr>
</tbody>
</table>

By displaying the package information, we know that we have a package
file containing RPM version 2.3. We can then peruse the file list, and
see exactly what it would install before installing it.

</div>

<div class="sect2">

## <span id="s2-rpm-query-finding-docs">Finding Documentation for a Specific Package</span>

Picking on **bash** some more, you realize that your knowledge of the
software is lacking. You'd like to see when it was installed on your
system, and what documentation is available for it:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qid bash
Name        :bash                   Distribution: Red Hat Linux (Picasso)
Version     :1.14.6                       Vendor: Red Hat Software
Release     :2                        Build Date: Sun Feb 25 13:59:26 1996
Install date:Mon May 13 12:47:22 1996 Build Host: porky.redhat.com
Group       :Shells                   Source RPM: bash-1.14.6-2.src.rpm
Size        :486557
Description :GNU Bourne Again Shell (bash)
/usr/doc/bash-1.14.6-2
/usr/doc/bash-1.14.6-2/NEWS
/usr/doc/bash-1.14.6-2/README
/usr/doc/bash-1.14.6-2/RELEASE
/usr/info/bash.info.gz
/usr/man/man1/bash.1

#
          </code></pre></td>
</tr>
</tbody>
</table>

You never realized that there could be so much documentation for a
shell\!

</div>

<div class="sect2">

## <span id="s2-rpm-query-similar-packages">Finding Similar Packages</span>

Looking at `bash`'s information, we see that it belongs to the group
"Shells". You're not sure what other shell packages are installed on
your system. If you can find other packages in the "Shells" group,
you'll have found the other installed shells:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qa --queryformat &#39;%10{NAME} %20{GROUP}\n&#39; | grep -i shells
       ash               Shells
      bash               Shells
       csh               Shells
        mc               Shells
      tcsh               Shells

#
          </code></pre></td>
</tr>
</tbody>
</table>

Now you can query each of these packages, and learn more about them,
too. [<span class="footnote">\[1\]</span>](#FTN.AEN3711)

</div>

<div class="sect2">

## <span id="s2-rpm-query-recently-installed-packages">Finding Recently Installed Packages, Part I</span>

You remember installing a new package a few days ago. All you know for
certain is that the package installed a new command in the `/bin`
directory. Let's try to find the package:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># find /bin -type f -mtime -14 | rpm -qF
rpm-2.3-1

#
          </code></pre></td>
</tr>
</tbody>
</table>

Looks like RPM version 2.3 was installed sometime in the last two weeks.

</div>

<div class="sect2">

## <span id="s2-rpm-query-recently-installed-packages2">Finding Recently Installed Packages, Part II</span>

Another way to see which packages were recently installed is to use the
**--queryformat** option:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qa --queryformat &#39;%{installtime} %{name}-%{version}-%{release} %{installtime:date}\n&#39; | sort -nr +1 | sed -e &#39;s/^[^ ]* //&#39;
rpm-devel-2.3-1 Thu Dec 26 23:02:05 1996
rpm-2.3-1 Thu Dec 26 23:01:51 1996
pgp-2.6.3usa-2 Tue Oct 22 19:39:09 1996
…
pamconfig-0.50-5 Tue Oct 15 17:23:22 1996
setup-1.5-1 Tue Oct 15 17:23:21 1996

#
          </code></pre></td>
</tr>
</tbody>
</table>

By having RPM include the installation time in numeric form, it was
simple to sort the packages and then use **sed** to remove the
user-unfriendly numeric time.

</div>

<div class="sect2">

## <span id="s2-rpm-query-largest-packages">Finding the Largest Installed Packages</span>

Let's say that you're running low on disk space, and you'd like to see
what packages you have installed, along with the amount of space each
package takes up. You'd also like to see the largest packages first, so
you can get back as much disk space as possible:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qa --queryformat &#39;%{name}-%{version}-%{release} %{size}\n&#39; | sort -nr +1
kernel-source-2.0.18-5  20608472
tetex-0.3.4-3  19757371
emacs-el-19.34-1  12259914
…
rootfiles-1.3-1  3494
mkinitrd-1.0-1  1898
redhat-release-4.0-1  22

#
          </code></pre></td>
</tr>
</tbody>
</table>

If you don't build custom kernels, or use TeX, it's easy to see how much
space could be reclaimed by removing those packages.

</div>

</div>

### Notes

|                                                                                |                                                                                                                                                                                                                                                                                                  |
| ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [<span class="footnote">\[1\]</span>](s1-rpm-query-handy-queries.html#AEN3711) | Did you see this example and say to yourself, "Hey, they could've used the -g option to query for that group directly"? If you did, you've been paying attention. This is a more general way of searching the RPM database for information: we just happened to search by group in this example. |

<div class="NAVFOOTER">

-----

|                                 |                         |                                        |
| :------------------------------ | :---------------------: | -------------------------------------: |
| [Prev](s1-rpm-query-parts.html) |   [Home](index.html)    |             [Next](ch-rpm-verify.html) |
| The Parts of an RPM Query       | [Up](ch-rpm-query.html) | Using RPM to Verify Installed Packages |

</div>
