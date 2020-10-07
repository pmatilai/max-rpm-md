<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-subpack-spec-file-changes.html)

Chapter 18. Creating Subpackages

[Next](s1-rpm-subpack-building-subpackages.html)

-----

<div class="sect1">

# <span id="s1-rpm-subpack-build-time-scripts">Build-Time Scripts: Unchanged For Subpackages</span>

While creating subpackages changes the general structure of the spec
file, there's one section that doesn't change: the build-time scripts.
This means there is only one set of **%prep**, **%build**, and
**%install** scripts in any spec file.

Of course, even if RPM doesn't require any changes to these scripts, you
still might need to make some subpackage-related changes to them.
Normally these changes are related to doing whatever is required to get
the all the software unpacked, built, and installed. For example, if
packaging client/server software, the software for both the client *and*
the server must be unpacked, and then both the client *and* server
binaries must be built and installed.

<div class="sect2">

## <span id="s2-rpm-subpack-one-last-look">Our Spec File: One Last Look…</span>

Let's add some build-time scripts and take a final look at the spec
file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Name: foo
Version: 2.7
Release: 1
Source: foo-2.7.tgz
License: probably not
Summary: The foo app, and the baz library needed to build it
Group: bogus/junque
%description
This is the long description of the foo app, and the baz library needed to
build it...

%package server
Summary: The foo server
Group: bogus/junque
%description server
This is the long description for the foo server...

%package client
Summary: The foo client
Group: bogus/junque
%description client
This is the long description for the foo client...

%package -n bazlib
Version: 5.6
Summary: The baz library
Group: bogus/junque
%description -n bazlib
This is the long description for the bazlib...

%prep
%setup

%build
make

%install
make install

%pre
echo &quot;This is the foo package preinstall script&quot;

%pre server
echo &quot;This is the foo-server subpackage preinstall script&quot;

#%pre client
#echo &quot;This is the foo-client subpackage preinstall script&quot;

%pre -n bazlib
echo &quot;This is the bazlib subpackage preinstall script&quot;

%files
/usr/local/foo-file

%files server
/usr/local/server-file

%files client
/usr/local/client-file

%files -n bazlib
/usr/local/bazlib-file

          </code></pre></td>
</tr>
</tbody>
</table>

As you can see, the build-time scripts are about as simple as they can
be. [<span class="footnote">\[1\]</span>](#FTN.AEN11138)

</div>

</div>

### Notes

|                                                                                        |                                                                                                                             |
| -------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-subpack-build-time-scripts.html#AEN11138) | This is the advantage to making up an example. A more real-world spec file would undoubtedly have more interesting scripts. |

<div class="NAVFOOTER">

-----

|                                               |                           |                                                  |
| :-------------------------------------------- | :-----------------------: | -----------------------------------------------: |
| [Prev](s1-rpm-subpack-spec-file-changes.html) |    [Home](index.html)     | [Next](s1-rpm-subpack-building-subpackages.html) |
| Spec File Changes For Subpackages             | [Up](ch-rpm-subpack.html) |                             Building Subpackages |

</div>
