<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-basics-and-now.html)

[Next](s1-rpm-build-getting-sources.html)

-----

<div class="chapter">

# <span id="ch-rpm-build"></span>Chapter 11. Building Packages: A Simple Example

In the previous chapter, we looked at RPM's build process from a
conceptual level. In this chapter, we will be performing an actual build
using RPM. In order to keep things understandable for this first pass,
the build will be very simple. Once we've covered the basics, we'll
present more real-world examples in later chapters.

<div class="sect1">

# <span id="s1-rpm-build-creating-build-directories">Creating the Build Directory Structure</span>

RPM requires a set of directories in which to perform the build. While
the directories' locations and names can be changed, unless there's a
reason to do so, it's best to use the default layout. Note that if
you've installed RPM, the build directories are most likely in place
already.

The normal directory layout consists of a single top-level directory
(The default name is `/usr/src/redhat`), with five subdirectories. The
five subdirectories and their functions are:

  - `/usr/src/redhat/SOURCES` — Contains the original sources, patches,
    and icon files.

  - `/usr/src/redhat/SPECS` — Contains the spec files used to control
    the build process.

  - `/usr/src/redhat/BUILD` — The directory in which the sources are
    unpacked, and the software is built.

  - `/usr/src/redhat/RPMS` — Contains the binary package files created
    by the build process.

  - `/usr/src/redhat/SRPMS` — Contains the source package files created
    by the build process.

In general, there are no special requirements that need to be met when
creating these directories. In fact, the only important requirement is
that the `BUILD` directory be part of a filesystem with sufficient free
space to build the largest package expected. Here is a directory listing
showing a typical build directory tree:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -lF /usr/src/redhat
total 5
drwxr-xr-x   3 root     root         1024 Aug  5 13:12 BUILD/
drwxr-xr-x   3 root     root         1024 Jul 17 17:51 RPMS/
drwxr-xr-x   4 root     root         1024 Aug  4 22:31 SOURCES/
drwxr-xr-x   2 root     root         1024 Aug  5 13:12 SPECS/
drwxr-xr-x   2 root     root         1024 Aug  4 22:28 SRPMS/

#
        </code></pre></td>
</tr>
</tbody>
</table>

Now that we have the directories ready to go, it's time to prepare for
the build. For the remainder of this chapter, we'll be building a
fictional piece of software known as `cdplayer`.
[<span class="footnote">\[1\]</span>](#FTN.AEN5544)

</div>

</div>

### Notes

|                                                                  |                                                                                                                                                                                                                                         |
| ---------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](ch-rpm-build.html#AEN5544) | In reality, this software is a mercilessly hacked version of `cdp`, which was written by Sariel Har-Peled. The software was hacked to provide a simple example package, and in no way represents the fine work done by Sariel on `cdp`. |

<div class="NAVFOOTER">

-----

|                                    |                    |                                           |
| :--------------------------------- | :----------------: | ----------------------------------------: |
| [Prev](s1-rpm-basics-and-now.html) | [Home](index.html) | [Next](s1-rpm-build-getting-sources.html) |
| And Now…                           |  [Up](p5206.html)  |                       Getting the Sources |

</div>
