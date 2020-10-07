<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-subpack-build-time-scripts.md)

Chapter 18. Creating Subpackages

[Next](ch-rpm-multi.md)

-----

<div class="sect1">

# <span id="s1-rpm-subpack-building-subpackages">Building Subpackages</span>

Now it's time to give our example spec file a try. The build process is
not that much different from a single-package spec file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba foo-2.7.spec
* Package: foo
* Package: foo-server
* Package: foo-client
* Package: bazlib
…
Executing: %prep
…
Executing: %build
…
Executing: %install
…
Executing: special doc
+ cd /usr/src/redhat/BUILD
+ cd foo-2.7
+ DOCDIR=//usr/doc/foo-2.7-1
+ DOCDIR=//usr/doc/foo-server-2.7-1
+ DOCDIR=//usr/doc/foo-client-2.7-1
+ DOCDIR=//usr/doc/bazlib-5.6-1
+ exit 0
Binary Packaging: foo-2.7-1
Finding dependencies...
usr/local/foo-file
1 block
Generating signature: 0
Wrote: /usr/src/redhat/RPMS/i386/foo-2.7-1.i386.rpm
Binary Packaging: foo-server-2.7-1
Finding dependencies...
usr/local/server-file
1 block
Generating signature: 0
Wrote: /usr/src/redhat/RPMS/i386/foo-server-2.7-1.i386.rpm
Binary Packaging: foo-client-2.7-1
Finding dependencies...
usr/local/client-file
1 block
Generating signature: 0
Wrote: /usr/src/redhat/RPMS/i386/foo-client-2.7-1.i386.rpm
Binary Packaging: bazlib-5.6-1
Finding dependencies...
usr/local/bazlib-file
1 block
Generating signature: 0
Wrote: /usr/src/redhat/RPMS/i386/bazlib-5.6-1.i386.rpm
…
Source Packaging: foo-2.7-1
foo-2.7.spec
foo-2.7.tgz
4 blocks
Generating signature: 0
Wrote: /usr/src/redhat/SRPMS/foo-2.7-1.src.rpm

#
        </code></pre></td>
</tr>
</tbody>
</table>

Starting at the top, we start the build with the usual command.
Immediately following the command, RPM indicates that four packages are
to be built from this spec file. The **%prep**, **%build**, and
**%install** scripts then execute as usual.

Next, RPM executes its "special doc" internal script, even though we
haven't declared any files to be documentation. It's worth noting,
however, that the `DOCDIR` environment variables show that if the spec
file *had* declared some of the files as documentation, RPM would have
created the appropriate documentation directories for each of the
packages.

At this point, RPM creates the binary packages. As we can see, each
package contains the file defined in its **%files** list.

Finally, the source package file is created. It contains the spec file
and the original sources, just like any other source package.

One spec file. One set of sources. One build command. Four packages.
[<span class="footnote">\[1\]</span>](#FTN.AEN11163) All in all, a
pretty good deal, isn't it?

<div class="sect2">

## <span id="s2-rpm-subpack-subpackage-testing">Giving Subpackages the Once-Over</span>

Let's take a look at our newly created packages. As with any other
package, each subpackage should be tested by installing it on a system
that has not had that software installed before. In this section, we'll
just snoop around the subpackages and point out how they differ from
packages built one to a spec file.

First, let's just look at each package's information:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qip foo-2.7-1.i386.rpm
Name        : foo                   Distribution: (none)
Version     : 2.7                         Vendor: (none)
Release     : 1                       Build Date: Wed Nov 06 13:33:37 1996
Install date: (none)                  Build Host: foonly.rpm.org
Group       : bogus/junque            Source RPM: foo-2.7-1.src.rpm
Size        : 35
Summary     : The foo app, and the baz library needed to build it
Description :
This is the long description of the foo app, and the baz library needed to
build it...

#
# rpm -qip foo-server-2.7-1.i386.rpm
Name        : foo-server            Distribution: (none)
Version     : 2.7                         Vendor: (none)
Release     : 1                       Build Date: Wed Nov 06 13:33:37 1996
Install date: (none)                  Build Host: foonly.rpm.org
Group       : bogus/junque            Source RPM: foo-2.7-1.src.rpm
Size        : 42
Summary     : The foo server
Description :
This is the long description for the foo server...

#
# rpm -qip foo-client-2.7-1.i386.rpm
Name        : foo-client            Distribution: (none)
Version     : 2.7                         Vendor: (none)
Release     : 1                       Build Date: Wed Nov 06 13:33:37 1996
Install date: (none)                  Build Host: foonly.rpm.org
Group       : bogus/junque            Source RPM: foo-2.7-1.src.rpm
Size        : 42
Summary     : The foo client
Description :
This is the long description for the foo client...

#
# rpm -qip bazlib-5.6-1.i386.rpm
Name        : bazlib                Distribution: (none)
Version     : 5.6                         Vendor: (none)
Release     : 1                       Build Date: Wed Nov 06 13:33:37 1996
Install date: (none)                  Build Host: foonly.rpm.org
Group       : bogus/junque            Source RPM: foo-2.7-1.src.rpm
Size        : 38
Summary     : The baz library
Description :
This is the long description for the bazlib...

#
          </code></pre></td>
</tr>
</tbody>
</table>

Here we've used RPM's query capability to display a list of summary
information for each package. A few points are worth noting.

First, each package lists `foo-2.7-1.src.rpm` as its source package
file. This is the only way to tell if two package files were created
from the same set of sources. Trying to use a package's name as an
indicator is futile, as the `bazlib` package shows us.

The next thing to notice is that the summaries and descriptions for each
package are specific to that package. Since these tags were placed and
named according to each package, that should be no surprise.

Finally, we can see that each package's version has been either
"inherited" from the main package's preamble, or, as in the case of the
`bazlib` package, the main package's version has been overridden by a
**version** tag added to `bazlib`'s preamble.

If we look at the source package's information, we see that its
information has been taken entirely from the main package's set of tags:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qip foo-2.7-1.src.rpm
Name        : foo                   Distribution: (none)
Version     : 2.7                         Vendor: (none)
Release     : 1                       Build Date: Wed Nov 06 13:33:37 1996
Install date: (none)                  Build Host: foonly.rpm.org
Group       : bogus/junque            Source RPM: (none)
Size        : 1415
Summary     : The foo app, and the baz library needed to build it
Description :
This is the long description of the foo app, and the baz library needed to
build it...

# 
          </code></pre></td>
</tr>
</tbody>
</table>

It's easy to see that if there was no **%files** list for the main
package, and therefore, no main package, the tags in the main preamble
would still be used in the source package. This is why RPM enforces the
requirement that the main preamble contain **copyright**,
**%description**, and **group** tags. So, here's a word to the wise:
Don't put something stupid in the main preamble's **%description** just
to satisfy RPM. Your witty saying will be immortalized for all time in
every source package you distribute.
[<span class="footnote">\[2\]</span>](#FTN.AEN11210)

<div class="sect3">

### <span id="s3-rpm-subpack-install-erase-scripts-verify">Verifying Subpackage-specific Install and Erase Scripts</span>

The easiest way to verify that the **%pre** scripts we defined for each
package were actually used is to simply install each package:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Uvh foo-2.7-1.i386.rpm
foo                    This is the foo package preinstall script
##################################################

#
# rpm -Uvh foo-server-2.7-1.i386.rpm
foo-server             This is the foo-server subpackage preinstall script
##################################################

#
# rpm -Uvh foo-client-2.7-1.i386.rpm
foo-client             This is the foo-client subpackage preinstall script
##################################################

#
# rpm -Uvh bazlib-5.6-1.i386.rpm
bazlib                 This is the bazlib subpackage preinstall script
##################################################

# 
            </code></pre></td>
</tr>
</tbody>
</table>

As expected, the unique **%pre** script for each package has been
included. Of course, if we hadn't wanted to actually install the
packages, we could have used RPM's **--scripts** option to display the
scripts:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qp --scripts foo-2.7-1.i386.rpm
preinstall script:
echo &quot;This is the foo package preinstall script&quot;

postinstall script:
(none)
preuninstall script:
(none)
postuninstall script:
(none)
verify script:
(none)

#
            </code></pre></td>
</tr>
</tbody>
</table>

This approach might be a bit safer, particularly if installing the newly
built package would disrupt operations on your build system.

</div>

</div>

</div>

### Notes

|                                                                                          |                                                 |
| ---------------------------------------------------------------------------------------- | ----------------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-subpack-building-subpackages.md#AEN11163) | Five, if you count the source package.          |
| [<span class="footnote">\[2\]</span>](s1-rpm-subpack-building-subpackages.md#AEN11210) | Yes, the author found out about this hard way\! |

<div class="NAVFOOTER">

-----

|                                                |                           |                                                                    |
| :--------------------------------------------- | :-----------------------: | -----------------------------------------------------------------: |
| [Prev](s1-rpm-subpack-build-time-scripts.md) |    [Home](index.md)     |                                          [Next](ch-rpm-multi.md) |
| Build-Time Scripts: Unchanged For Subpackages  | [Up](ch-rpm-subpack.md) | Building Packages for Multiple Architectures and Operating Systems |

</div>
