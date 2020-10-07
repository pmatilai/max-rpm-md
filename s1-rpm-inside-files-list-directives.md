<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-inside-files-list.md)

Chapter 13. Inside the Spec File

[Next](s1-rpm-inside-package-directive.md)

-----

<div class="sect1">

# <span id="s1-rpm-inside-files-list-directives">Directives For the **%files** list</span>

The **%files** list may contain a number of different directives. They
are used to:

  - Identify documentation and configuration files.

  - Ensure that a file has the correct permissions and ownership set.

  - Control which aspects of a file are to be checked during package
    verification.

  - Eliminate some of the tedium in creating the **%files** list.

In the **%files** list, one or more directives may be placed on a line,
separated by spaces, before one or more filenames. Therefore, if
**%foo** and **%bar** are two **%files** list directives, they may be
applied to a file `baz` in the following manner:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%foo %bar baz

        </code></pre></td>
</tr>
</tbody>
</table>

Now it's time to take a look at the directives that inhabit the
**%files** list.

<div class="sect2">

## <span id="s2-rpm-inside-flist-file-directives">File-related Directives</span>

RPM processes files differently according to their type. However, RPM
does not have a method of automatically determining file types.
Therefore, it is up to the package builder to appropriately mark files
in the **%files** list. This is done using one of the directives below.

Keep in mind that not every file will need to be marked. As you read the
following sections, you'll see that directives are only used in special
circumstances. In most packages, the majority of files in the **%files**
list will not need to be marked.

<div class="sect3">

### <span id="s3-rpm-inside-flist-doc-directive">The **%doc** Directive</span>

The **%doc** directive flags the filename(s) that follow, as being
documentation. RPM keeps track of documentation files in its database,
so that a user can easily find information about an installed package.
In addition, RPM can create a package-specific documentation directory
during installation and copy documentation into it. Whether or not this
additional step is taken, is dependent on how a file is specified. Here
is an example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%doc README
%doc /usr/local/foonly/README

            </code></pre></td>
</tr>
</tbody>
</table>

The file `README` exists in the software's top-level directory during
the build, and is included in the package file. When the package is
installed, RPM creates a directory in the documentation directory named
the same as the package (ie, `<software>`-`<version>`-`<release>`), and
copies the `README` file there. The newly created directory and the
`README` file are marked in the RPM database as being documentation. The
default documentation directory is `/usr/doc`, and can be changed by
setting the **defaultdocdir** `rpmrc` file entry. For more information
on `rpmrc` files, please see [Appendix B](ch-rpmrc-file.md).

The file `/usr/local/foonly/README` was installed into that directory
during the build and is included in the package file. When the package
is installed, the `README` file is copied into `/usr/local/foonly` and
marked in the RPM database as being documentation.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-flist-config-directive">The **%config** Directive</span>

The **%config** directive is used to flag the specified file as being a
configuration file. RPM performs additional processing for config files
when packages are erased, and during installations and upgrades. This is
due to the nature of config files: They are often changed by the system
administrator, and those changes should not be lost.

There is a restriction to the **%config** directive, and that
restriction is that no more than one filename may follow the
**%config**. This means that the following example is the only allowable
way to specify config files:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%config /etc/foonly

            </code></pre></td>
</tr>
</tbody>
</table>

Note that the full path to the file, as it is installed at build time,
is required.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-flist-attr-directive">The **%attr** Directive</span>

The **%attr** directive permits finer control over three key file
attributes:

1.  The file's permissions, or "mode".

2.  The file's user ID.

3.  The file's group ID.

The **%attr** directive has the following format:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%attr(&lt;mode&gt;, &lt;user&gt;, &lt;group&gt;) file

            </code></pre></td>
</tr>
</tbody>
</table>

The mode is specified in the traditional numeric format, while the user
and group are specified as a string, such as "**root**". Here's a sample
**%attr** directive:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%attr(755, root, root) foo.bar

            </code></pre></td>
</tr>
</tbody>
</table>

This would set `foo.bar`'s permissions to 755. The file would be owned
by user root, group root. If a particular attribute does not need to be
specified (usually because the file is installed with that attribute set
properly), then that attribute may be replaced with a dash:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%attr(755, -, root) foo.bar

            </code></pre></td>
</tr>
</tbody>
</table>

The main reason to use the **%attr** directive is to permit users
without root access to build packages. The techniques for doing this
(and a more in-depth discussion of the **%attr** directive) can be found
in [Chapter 16](ch-rpm-anywhere.md).

</div>

<div class="sect3">

### <span id="s3-rpm-inside-flist-defattr-directive">The **%defattr** Directive</span>

The **%defattr** directive allows setting of default attributes for
files and directives. The **%defattr** has a similar format to the
**%attr** directive:

1.  The default permissions, or "mode" for files.

2.  The default user id.

3.  The default group id.

4.  The default permissions, or "mode" for directories.

The **%attr** directive has the following format:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%defattr(&lt;file mode&gt;, &lt;user&gt;, &lt;group&gt;, &lt;dir mode&gt;)

            </code></pre></td>
</tr>
</tbody>
</table>

As with **%attr** if a particular attribute does not need to be
specified (usually because the file is installed with that attribute set
properly), then that attribute may be replaced with a dash. In addition
the directory mode may be ommited. **%defattr** tends to be used at the
top of **%files**.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-flist-ghost-directive">The **%ghost** Directive</span>

As we mentioned in [the Section called *The **%files**
List*](s1-rpm-inside-files-list.md), if a file is specified in the
**%files** list, that file will automatically be included in the
package. There are times when a file should be owned by the package but
not installed - log files and state files are good examples of cases you
might desire this to happen.

The way to achieve this, is to use the **%ghost** directive. By adding
this directive to the line containing a file, RPM will know about the
ghosted file, but will not add it to the package. However it still needs
to be in the buildroot. Here's an example of **%ghost** in action.

The `blather-1.0` package logs to `/var/log/blather.log` in it's default
config. In the spec file, the `/var/log/blather.log` file is included in
the **%files** list. We can see that blather.log belongs to the package,
and it is removed when the package is.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%install
touch $RPM_BUILD_ROOT%{_localstatedir}/log/blather.log
…
%files
…
%ghost %{_localstatedir}/log/blather.log
…

# rpm -qf /var/log/blather.log
blather-1.0-1

# rpm -ql blather | grep blather.log


# rpm -e blather &amp;&amp; ls /var/log/blather.log
ls: /var/log/blather.log: No such file or directory

            </code></pre></td>
</tr>
</tbody>
</table>

There file touched in the **%install** stage will not be installed to
`/var/log/blather.log` although it will be added to the rpm database, as
we can see from querying the file, however it is not visible from a
package listing, but as it is owned by the package it will be removed
when the package is removed. In addition it is possible to use setperms
to fix the permissions on a **%ghost** file.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -al /var/log/blather.log
-rw-r--r--    1 root     root         3448 Jun 18 17:00 /var/log/blather.log

#chmod 666 /var/log/blather.log
# ls -al /var/log/blather.log
-rw-rw-rw-    1 root     root         3448 Jun 18 17:00 /var/log/blather.log

#rpm --setperms blather
# ls -al /var/log/blather.log
-rw-r--r--    1 root     root         3448 Jun 18 17:00 /var/log/blather.log

            </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect3">

### <span id="s3-rpm-inside-flist-verify-directive">The **%verify** Directive</span>

RPM's ability to verify the integrity of the software it has installed
is impressive. But sometimes it's a bit *too* impressive. After all, RPM
can verify as many as nine different aspects of every file. The
**%verify** directive can control which of these file attributes are to
be checked when an RPM verification is done. Here are the attributes,
along with the names used by the **%verify** directive:

1.  Owner (**owner**)

2.  Group (**group**)

3.  Mode (**mode**)

4.  MD5 Checksum (**md5**)

5.  Size (**size**)

6.  Major Number (**maj**)

7.  Minor Number (**min**)

8.  Symbolic Link String (**symlink**)

9.  Modification Time (**mtime**)

How is **%verify** used? Say, for instance, that a package installs
device files. Since the owner of a device will change, it doesn't make
sense to have RPM verify the device file's owner/group and give out a
false alarm. Instead, the following **%verify** directive could be used:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%verify(mode md5 size maj min symlink mtime) /dev/ttyS0

            </code></pre></td>
</tr>
</tbody>
</table>

We've left out **owner** and **group**, since we'd rather RPM not verify
those. [<span class="footnote">\[1\]</span>](#FTN.AEN8936)

However, if all you want to do is prevent RPM from verifying one or two
attributes, you can use **%verify**'s alternate syntax:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%verify(not owner group) /dev/ttyS0

            </code></pre></td>
</tr>
</tbody>
</table>

This use of **%verify** produces identical results to the previous
example.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-inside-flist-directory-directives">Directory-related Directives</span>

While the two directives in this section perform different functions,
each is related to directories in some way. Let's see what they do:

<div class="sect3">

### <span id="s3-rpm-inside-docdir-directive">The **%docdir** Directive</span>

The **%docdir** directive is used to add a directory to the list of
directories that will contain documentation. RPM includes the
directories `/usr/doc`, `/usr/info`, and `/usr/man` in the **%docdir**
list by default.

For example, if the following line is part of the **%files** list:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%docdir /usr/blather

            </code></pre></td>
</tr>
</tbody>
</table>

any files in the **%files** list that RPM packages from `/usr/blather`
will be included in the package as usual, but will also be automatically
flagged as documentation. This directive is handy when a package creates
its own documentation directory and contains a large number of files.
Let's give it a try by adding the following line to our spec file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%docdir /usr/blather

            </code></pre></td>
</tr>
</tbody>
</table>

Our **%files** list contains no references to the several files the
package installs in the `/usr/blather` directory. After building the
package, looking at the package's file list shows:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qlp ../RPMS/i386/blather-1.0-1.i386.rpm
…

#
            </code></pre></td>
</tr>
</tbody>
</table>

Wait a minute: There's nothing there, not even `/usr/blather`\! What
happened?

The problem is that **%docdir** only directs RPM to mark the specified
directory as holding documentation. It *doesn't* direct RPM to package
any files in the directory. To do that, we need to clue RPM in to the
fact that there are files in the directory that must be packaged.

One way to do this is to simply add the files to the **%files** list:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%docdir /usr/blather
/usr/blather/INSTALL

            </code></pre></td>
</tr>
</tbody>
</table>

Looking at the package, we see that `INSTALL` was packaged:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qlp ../RPMS/i386/blather-1.0-1.i386.rpm
…
/usr/blather/INSTALL

#
            </code></pre></td>
</tr>
</tbody>
</table>

Directing RPM to only show the documentation files, we see that
`INSTALL` has indeed been marked as documentation, even though the
**%doc** directive had not been used:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qdp ../RPMS/i386/blather-1.0-1.i386.rpm
…
/usr/blather/INSTALL

#
            </code></pre></td>
</tr>
</tbody>
</table>

Of course, if you go to the trouble of adding each file to the
**%files** list, it wouldn't be that much more work to add **%doc** to
each one. So the way to get the most benefit from **%docdir** is to add
another line to the **%files** list:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%docdir /usr/blather
/usr/blather

            </code></pre></td>
</tr>
</tbody>
</table>

Since the first line directs RPM to flag any file in `/usr/blather` as
being documentation, and the second line tells RPM to automatically
package any files found in `/usr/blather`, every single file in there
will be packaged *and* marked as documentation:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qdp ../RPMS/i386/blather-1.0-1.i386.rpm
/usr/blather
/usr/blather/COPYING
/usr/blather/INSTALL
/usr/blather/README
…

#
            </code></pre></td>
</tr>
</tbody>
</table>

The **%docdir** directive can save quite a bit of effort in creating the
**%files** list. The only caveat is that you must be sure the directory
will only contain files you want marked as documentation. Keep in mind,
also, that all subdirectories of the **%docdir**'ed directory will be
marked as documentation directories, too.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-dir-directive">The **%dir** Directive</span>

As we mentioned in [the Section called *The **%files**
List*](s1-rpm-inside-files-list.md), if a directory is specified in
the **%files** list, the contents of that directory, and the contents of
every directory under it, will automatically be included in the package.
While this feature can be handy (assuming you are *sure* that every file
under the directory should be packaged) there are times when this could
be a problem.

The way to get around this, is to use the **%dir** directive. By adding
this directive to the line containing the directory, RPM will package
only the directory itself, regardless of what files are in the directory
at the time the package is created. Here's an example of **%dir** in
action.

The `blather-1.0` package creates the directory `/usr/blather` as part
of its build. It also puts several files in that directory. In the spec
file, the `/usr/blather` directory is included in the **%files** list:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%files
…
/usr/blather
…

            </code></pre></td>
</tr>
</tbody>
</table>

There are no other entries in the **%files** list that have
`/usr/blather` as part of their path. After building the package, we use
RPM to look at the files in the package:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qlp ../RPMS/i386/blather-1.0-1.i386.rpm
…
/usr/blather
/usr/blather/COPYING
/usr/blather/INSTALL
/usr/blather/README
…

#
            </code></pre></td>
</tr>
</tbody>
</table>

The files present in `/usr/blather` at the time the package was built
were included in the package automatically, without entering their names
in the **%files** list.

However, after changing the `/usr/blather` line in the **%files** list
to:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%dir /usr/blather

            </code></pre></td>
</tr>
</tbody>
</table>

and rebuilding the package, a listing of the package's files now
includes only the `/usr/blather` directory:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qlp ../RPMS/i386/blather-1.0-1.i386.rpm
…
/usr/blather
…

#
            </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect3">

### <span id="s3-rpm-inside-flist-f-option">**-f `<file>`** — Read the **%files** List From **`<file>`**</span>

The **-f** option is used to direct RPM to read the **%files** list from
the named file. Like the **%files** list in a spec file, the file named
using the **-f** option should contain one filename per line and also
include any of the directives named in this section.

Why is it necessary to read filenames from a file rather than have the
filenames in the spec file? Here's a possible reason:

The filenames' paths may contain a directory name that can only be
determined at build-time, such as an architecture specification. The
list of files, minus the variable part of the path, can be created, and
**sed** can be used at build-time to update the path appropriately.

It's not necessary that every filename to be packaged reside in the
file. If there are any filenames present in the spec file, they will be
packaged as well:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%files latex -f tetex-latex-skel
/usr/bin/latex
/usr/bin/pslatex
…

            </code></pre></td>
</tr>
</tbody>
</table>

Here, the filenames present in the file `tetex-latex-skel` would be
packaged, followed by every filename following the **%files** line.

</div>

</div>

</div>

### Notes

|                                                                                         |                                                                                                                                                                                                              |
| --------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [<span class="footnote">\[1\]</span>](s1-rpm-inside-files-list-directives.md#AEN8936) | RPM will automatically exclude file attributes from verification if it doesn't make sense for the type of file. In our example, getting the MD5 checksum of a device file is an example of such a situation. |

<div class="NAVFOOTER">

-----

|                                       |                          |                                              |
| :------------------------------------ | :----------------------: | -------------------------------------------: |
| [Prev](s1-rpm-inside-files-list.md) |    [Home](index.md)    | [Next](s1-rpm-inside-package-directive.md) |
| The **%files** List                   | [Up](ch-rpm-inside.md) |             The Lone Directive: **%package** |

</div>
