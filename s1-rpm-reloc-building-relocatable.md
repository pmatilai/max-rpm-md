<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-reloc-wrinkles.html)

Chapter 15. Making a Relocatable Package

[Next](ch-rpm-anywhere.html)

-----

<div class="sect1">

# <span id="s1-rpm-reloc-building-relocatable">Building a Relocatable Package</span>

For this example, we'll use our tried-and-true `cdplayer` application.
Let's start by reviewing the spec file for possible problems:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#
# Example spec file for cdplayer app...
#
Summary:A CD player app that rocks!
Name: cdplayer
…
%files
%doc README
/usr/local/bin/cdp
/usr/local/bin/cdplay
%doc /usr/local/man/man1/cdp.1
%config /etc/cdp-config

        </code></pre></td>
</tr>
</tbody>
</table>

Everything looks all right, except for the **%files** list. There are
files in `/usr/local/bin`, a **man** page in `/usr/local/man/man1`, and
a config file in `/etc`. A prefix of `/usr/local` would work pretty
well, except for that `cdp-config` file.

For the sake of this first build, we'll declare the config file
unnecessary and remove it from the **%files** list. We'll then add a
**prefix** tag line, setting the prefix to `/usr/local`. After these
changes are made, let's try a build:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba cdplayer-1.0.spec
* Package: cdplayer
+ umask 022
+ echo Executing: %prep
Executing: %prep
+ cd /usr/src/redhat/BUILD
+ cd /usr/src/redhat/BUILD
+ rm -rf cdplayer-1.0
+ gzip -dc /usr/src/redhat/SOURCES/cdplayer-1.0.tgz
…
Binary Packaging: cdplayer-1.0-1
Package Prefix = usr/local
File doesn&#39;t match prefix (usr/local): /usr/doc/cdplayer-1.0-1
File not found: /usr/doc/cdplayer-1.0-1
Build failed.

# 
        </code></pre></td>
</tr>
</tbody>
</table>

The build proceeded normally up to the point of actually creating the
binary package. The Package Prefix = usr/local line confirms that RPM
picked up our **prefix** tag line. But the build stopped — and on a file
called `/usr/doc/cdplayer-1.0-1`. But that file isn't even in the
**%files** list. What's going on?

Take a closer look at the **%files** list. See the line that reads
**%doc README**? In [the Section called *The **%doc** Directive* in
Chapter
13](s1-rpm-inside-files-list-directives.html#s3-rpm-inside-flist-doc-directive),
we discussed how the **%doc** directive creates a directory under
`/usr/doc`. That's the file that killed the build — the directory
created by the **%doc** directive.

Let's temporarily remove that line from the **%files** list and try
again:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba cdplayer-1.0.spec
* Package: cdplayer
+ umask 022
+ echo Executing: %prep
Executing: %prep
+ cd /usr/src/redhat/BUILD
+ cd /usr/src/redhat/BUILD
+ rm -rf cdplayer-1.0
+ gzip -dc /usr/src/redhat/SOURCES/cdplayer-1.0.tgz
…
Binary Packaging: cdplayer-1.0-1
Package Prefix = usr/local
Finding dependencies...
Requires (2): libc.so.5 libncurses.so.2.0
bin/cdp
bin/cdplay
man/man1/cdp.1
90 blocks
Generating signature: 0
Wrote: /usr/src/redhat/RPMS/i386/cdplayer-1.0-1.i386.rpm
+ umask 022
+ echo Executing: %clean
Executing: %clean
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
+ exit 0
Source Packaging: cdplayer-1.0-1
cdplayer-1.0.spec
cdplayer-1.0.tgz
82 blocks
Generating signature: 0
Wrote: /usr/src/redhat/SRPMS/cdplayer-1.0-1.src.rpm

# 
        </code></pre></td>
</tr>
</tbody>
</table>

The build completed normally. Note how the files to be placed in the
binary package file are listed, minus the prefix of `/usr/local`. Some
of you might be wondering why the `cdp.1` file didn't cause problems.
After all, it had a **%doc** directive, too. The answer lies in the way
the file was specified. Since the file was specified using an absolute
path, and that path started with the prefix `/usr/local`, there was no
problem. A more complete discussion of the **%doc** directive can be
found in [the Section called *The **%doc** Directive* in Chapter
13](s1-rpm-inside-files-list-directives.html#s3-rpm-inside-flist-doc-directive).

<div class="sect2">

## <span id="s2-rpm-reloc-tying-loose-ends">Tying Up the Loose Ends</span>

In the course of building this package, we ran into two hitches:

1.  The config file `cdp-config` couldn't be installed in `/etc`.

2.  The `README` file could not be packaged using the **%doc**
    directive.

Both of these issues are due to the fact that the files' paths do not
start with the default prefix path `/usr/local`. Does this mean this
package cannot be relocated? Possibly, but there are two options to
consider. The first option is to review the prefix. In the case of our
example, if we chose a prefix of `/usr` instead of `/usr/local`, the
`README` file could be packaged using the **%doc** directive, since the
default documentation directory is `/usr/doc`. Another approach would be
to use the **%docdir** directive to define another documentation-holding
directory somewhere along the prefix path.
[<span class="footnote">\[1\]</span>](#FTN.AEN9954)

This approach wouldn't work for `/etc/cdp-config`, though. To package
that file, we'd need to resort to more extreme measures. Basically, this
approach would entail packaging the file in an acceptable directory
(something under `/usr/local`) and using the **%post** post-install
script to move the file to `/etc`. Pointing a symlink at the config file
is another possibility.

Of course, this approach has some problems. First, you'll need to write
a **%postun** script to undo what the **%post** script does.
[<span class="footnote">\[2\]</span>](#FTN.AEN9968) A **%verifyscript**
that made sure the files were in place would be nice, too. Second,
because the file or symlink wasn't installed by RPM, there's no entry
for it in the RPM database. This reduces the utility of RPM's **-c** and
**-d** options when issuing queries. Finally, if you actually move files
around using the **%post** script, the files you move will not verify
properly, and when the package is erased, your users will get some
disconcerting messages when RPM can't find the moved files to erase
them. If you have to resort to these kinds of tricks, it's probably best
to forget trying to make the package relocatable.

</div>

<div class="sect2">

## <span id="s2-rpm-reloc-test-drive">Test-Driving a Relocatable Package</span>

Looks like `cdplayer` is a poor candidate for being made relocatable.
However, since we did get a hamstrung version to build successfully, we
can use it to show how to test a relocatable package.

First, let's see if the binary package file's prefix has been recorded
properly. We can do this by using the **--queryformat** option to RPM's
query mode:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qp --queryformat &#39;%{DEFAULTPREFIX}\n&#39; cdplayer-1.0-1.i386.rpm
/usr/local

#
          </code></pre></td>
</tr>
</tbody>
</table>

The **DEFAULTPREFIX** tag directs RPM to display the prefix used during
the build. As we can see, it's `/usr/local`, just as we intended. The
**--queryformat** option is discussed in [the Section called
***--queryformat** — Construct a Custom Query Response* in Chapter
5](s1-rpm-query-parts.html#s3-rpm-query-queryformat-option).

So it looks like we have a relocatable package. Let's try a couple of
installs and see if we really *can* install it in different locations.
First, let's try a regular install with no prefix specified:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Uvh cdplayer-1.0-1.i386.rpm
cdplayer    ##################################################

#
          </code></pre></td>
</tr>
</tbody>
</table>

That seemed to work well enough. Let's see if the files went where we
intended:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -al /usr/local/bin
total 558
-rwxr-xr-x   1 root     root        40739 Oct  7 13:23 cdp*
lrwxrwxrwx   1 root     root           18 Oct  7 13:40 cdplay -&gt; /usr/local/bin/cdp*
…
# ls -al /usr/local/man/man1
total 9
-rwxr-xr-x   1 root     root         4550 Oct  7 13:23 cdp.1*
…

#
          </code></pre></td>
</tr>
</tbody>
</table>

Looks good. Let's erase the package and reinstall it with a different
prefix:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -e cdplayer
# rpm -Uvh --prefix /usr/foonly/blather cdplayer-1.0-1.i386.rpm
cdplayer    ##################################################

#
          </code></pre></td>
</tr>
</tbody>
</table>

(We should mention that directories `foonly` and `blather` didn't exist
prior to installing `cdplayer`.)

RPM has another tag that can be used with the **--queryformat** option.
It's called **INSTALLPREFIX** and using it displays the prefix under
which a package was installed. Let's give it a try:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -q --queryformat &#39;%{INSTALLPREFIX}\n&#39; cdplayer
/usr/foonly/blather

#
          </code></pre></td>
</tr>
</tbody>
</table>

As we can see, it displays the prefix we entered on the command line.
Let's see if the files were installed as we directed:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># cd /usr/foonly/blather/
# ls -al
total 2
drwxr-xr-x   2 root     root         1024 Oct  7 13:45 bin/
drwxr-xr-x   3 root     root         1024 Oct  7 13:45 man/

#
          </code></pre></td>
</tr>
</tbody>
</table>

So far, so good — the proper directories are there. Let's look at the
**man** page first:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># cd /usr/foonly/blather/man/man1/
# ls -al
total 5
-rwxr-xr-x   1 root     root         4550 Oct  7 13:23 cdp.1*

#
          </code></pre></td>
</tr>
</tbody>
</table>

That looks ok. Now on to the files in `bin`:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># cd /usr/foonly/blather/bin
# ls -al
total 41
-rwxr-xr-x   1 root     root        40739 Oct  7 13:23 cdp*
lrwxrwxrwx   1 root     root           18 Oct  7 13:45 cdplay -&gt; /usr/local/bin/cdp

#
          </code></pre></td>
</tr>
</tbody>
</table>

Uh-oh. That `cdplay` symlink isn't right. What happened? If we look at
`cdplayer`'s makefile, we see the answer:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>install: cdp cdp.1.Z
…
ln -s /usr/local/bin/cdp /usr/local/bin/cdplay

          </code></pre></td>
</tr>
</tbody>
</table>

Ah, when the software is installed during RPM's build process, the make
file sets up the symbolic link. Looking back at the **%files** list, we
see `cdplay` listed. RPM blindly packaged the symlink, complete with its
non-relocatable string. This is why we mentioned absolute symlinks as a
prime example of non-relocatable software.

Fortunately, this problem isn't that difficult to fix. All we need to do
is change the line in the makefile that creates the symlink from:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>ln -s /usr/local/bin/cdp /usr/local/bin/cdplay

          </code></pre></td>
</tr>
</tbody>
</table>

To:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>ln -s ./cdp /usr/local/bin/cdplay

          </code></pre></td>
</tr>
</tbody>
</table>

Now `cdplay` will always point to `cdp`, no matter where it's installed.
When building relocatable packages, relative symlinks are your friend\!

After rebuilding the package, let's see if our modifications have the
desired effect. First, a normal install with the default prefix:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Uvh cdplayer-1.0-1.i386.rpm
cdplayer    ##################################################

# cd /usr/local/bin/
# ls -al cdplay
lrwxrwxrwx   1 root     root           18 Oct  8 22:32 cdplay -&gt; ./cdp*

# 
          </code></pre></td>
</tr>
</tbody>
</table>

Next, we'll try a second install using the **--prefix** option (after we
first delete the original package):

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -e cdplayer
# rpm -Uvh --prefix /a/dumb/prefix cdplayer-1.0-1.i386.rpm
cdplayer    ##################################################

# cd /a/dumb/prefix/bin/
# ls -al cdplay
lrwxrwxrwx   1 root     root           30 Oct  8 22:34 cdplay -&gt; ./cdp*

#
          </code></pre></td>
</tr>
</tbody>
</table>

As you can see, the trickiest part about building relocatable packages
is making sure the software you're packaging is up to the task. Once
that part of the job is done, the actual modifications are
straightforward.

In the next chapter, we'll cover how packages can be built in
non-standard directories, as well as how non-root users can build
packages.

</div>

</div>

### Notes

|                                                                                       |                                                                                                                                                                                                                                                                                                                                             |
| ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-reloc-building-relocatable.html#AEN9954) | For more information on the **%docdir** directive, please see [the Section called *The **%docdir** Directive* in Chapter 13](s1-rpm-inside-files-list-directives.html#s3-rpm-inside-docdir-directive).                                                                                                                                      |
| [<span class="footnote">\[2\]</span>](s1-rpm-reloc-building-relocatable.html#AEN9968) | Install and erase-time scripts have an environment variable, `RPM_INSTALL_PREFIX`, that can be used to write scripts that are able to act appropriately if the package is relocated. See [the Section called *Install/Erase-time Scripts* in Chapter 13](s1-rpm-inside-scripts.html#s2-rpm-inside-erase-time-scripts) for more information. |

<div class="NAVFOOTER">

-----

|                                          |                         |                                          |
| :--------------------------------------- | :---------------------: | ---------------------------------------: |
| [Prev](s1-rpm-reloc-wrinkles.html)       |   [Home](index.html)    |             [Next](ch-rpm-anywhere.html) |
| Relocatable Wrinkles: Things to Consider | [Up](ch-rpm-reloc.html) | Making a Package That Can Build Anywhere |

</div>
