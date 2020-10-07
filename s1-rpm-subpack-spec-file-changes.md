<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-subpack-example-intro.html)

Chapter 18. Creating Subpackages

[Next](s1-rpm-subpack-build-time-scripts.html)

-----

<div class="sect1">

# <span id="s1-rpm-subpack-spec-file-changes">Spec File Changes For Subpackages</span>

The creation of subpackages is based strictly on the contents of the
spec file. This doesn't mean that you'll have to learn an entirely new
set of tags, conditionals, and directives in order to create
subpackages. In fact, you'll only need to learn one.

The primary change to a spec file is structural and starts with the
definition of a preamble for each subpackage.

<div class="sect2">

## <span id="s2-rpm-subpack-subpackage-preamble">The Subpackage's "Preamble"</span>

When we introduced RPM package building in [Chapter
10](ch-rpm-basics.html), we said that every spec file contains a
preamble. The preamble contains a variety of tags that define all sorts
of information about the package. In a single package situation, the
preamble must be at the start of the spec file. The spec file we're
creating will have one there, too.

When creating a spec file that will build subpackages, each subpackage
also needs a preamble of its own. These "sub-preambles" need only define
information for the subpackage when that information differs from what
is defined in the main preamble. For example, if we wanted to define an
installation prefix for a subpackage, we would add the appropriate
**prefix** tag to that subpackage's preamble. That subpackage would then
be relocatable.

In a single-package spec file, there is nothing that explicitly
identifies the preamble, other than its position at the top of the file.
For subpackages, however, we need to be a bit more explicit. So we use
the **%package** directive to identify the preamble for each subpackage.

<div class="sect3">

### <span id="s3-rpm-subpack-package-directive">The **%package** Directive</span>

The **%package** directive actually performs two functions. As we
mentioned above, it is used to denote the start of a subpackage's
preamble. It also plays a role in forming the subpackage's name. As an
example, let's say the main preamble contains the following **name**
tag:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>name: foo

            </code></pre></td>
</tr>
</tbody>
</table>

Later in the spec file, there is a **%package** directive:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%package bar

            </code></pre></td>
</tr>
</tbody>
</table>

This would result in the name of the subpackage being `foo-bar`.

In this way, it's easy to see the relationship of the subpackage to the
main package (or other subpackages, for that matter). Of course, this
naming convention might not be appropriate in every case. So there is an
option to the **%package** directive for just this circumstance.

</div>

<div class="sect3">

### <span id="s3-rpm-subpack-package-directive-n-option">Adding **-n** To the **%package** directive</span>

The **-n** option is used to change the final name of a subpackage from
`<mainpackage>`-`<subpackage>` to `<subpackage>`. Let's modify the
**%package** directive in our example above to be:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%package -n bar

            </code></pre></td>
</tr>
</tbody>
</table>

The result is that the subpackage name would then be `bar` instead of
`foo-bar`.

</div>

<div class="sect3">

### <span id="s3-rpm-subpack-updating-spec-file">Updating Our Spec File</span>

Let's apply some of our newly found knowledge to the spec file we're
writing. Here's the list of subpackages that we need to create:

  - The server subpackage, to be called `foo-server`.

  - The client subpackage, to be called `foo-client`.

  - The `baz` development library subpackage, to be called `bazlib`.

Since our package name is `foo`, and since the **%package** directive
creates subpackage names by prepending the package name, the
**%package** directives for the `foo-server` and `foo-client`
subpackages would be written as:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%package server
%package client

            </code></pre></td>
</tr>
</tbody>
</table>

Since the `baz` library's package name is *not* to start with `foo`, we
need to use the **-n** option on its **%package** directive:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%package -n bazlib

            </code></pre></td>
</tr>
</tbody>
</table>

Our requirements further state that `foo-server` and `foo-client` are to
have the same version as the main package.

One of the time-saving aspects of using subpackages is that there is no
need to duplicate information for each subpackage if it is already
defined in the main package. Therefore, since the main package's
preamble has a **version** tag defining the version as 2.7, the two
subpackages that lack a **version** tag in their preambles will simply
inherit the main package's version definition.

Since the `bazlib` subpackage's preamble contains a **version** tag, it
must have its own unique version.

In addition, each subpackage must have its own **summary** tag.

So based on these requirements, our spec file now looks like this:

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

%package client
Summary: The foo client

%package -n bazlib
Version: 5.6
Summary: The baz library

            </code></pre></td>
</tr>
</tbody>
</table>

We can see the subpackage structure starting to appear now.

</div>

<div class="sect3">

### <span id="s3-rpm-subpack-required-tags">Required Tags In Subpackages</span>

There are a few more tags we should add to the subpackages in our
example spec file. In fact, if these tags are *not* present, RPM will
issue a most impressive warning:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba foo-2.7.spec
* Package: foo
* Package: foo-server
Field must be present    : Description
Field must be present    : Group
* Package: foo-client
Field must be present    : Description
Field must be present    : Group
* Package: bazlib
Field must be present    : Description
Field must be present    : Group

Spec file check failed!!
Tell rpm-list@redhat.com if this is incorrect.

#
            </code></pre></td>
</tr>
</tbody>
</table>

Our spec file is incomplete. The bottom line is that each subpackage
*must* have these three tags:

1.  The **%description** tag.

2.  The **group** tag.

3.  The **summary** tag.

It's easy to see that the first two tags are required, but what about
**summary**? Well, we lucked out on that one: we already included a
**summary** for each subpackage in our example spec file.

Let's take a look at the **%description** tag first.

</div>

<div class="sect3">

### <span id="s3-rpm-subpack-description-tag">The **%description** Tag</span>

As you've probably noticed, the **%description** tag differs from other
tags. First of all, it starts with a percent sign. Secondly, its data
can span multiple lines. The third difference is that the
**%description** tag must include the name of the subpackage it
describes. This is done by appending the subpackage name to the
**%description** tag itself. So given these **%package** directives:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%package server
%package client
%package -n bazlib

            </code></pre></td>
</tr>
</tbody>
</table>

our **%description** tags would start with:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%description server
%description client
%description -n bazlib

            </code></pre></td>
</tr>
</tbody>
</table>

Notice that we've included the **-n** option in the **%description** tag
for `bazlib`. This was intentional, as it makes the name completely
unambiguous.

</div>

<div class="sect3">

### <span id="s3-rpm-subpack-spec-file-so-far">Our Spec File So Far…</span>

OK, let's take a look at the spec file after we've added the appropriate
**%description**s, along with **group** tags for each subpackage:

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

            </code></pre></td>
</tr>
</tbody>
</table>

Let's take a look at what we've done. We've created a main preamble as
we normally would. We then created three additional preambles, each
starting with a **%package** directive. Finally, we added a few tags to
the subpackage preambles.

But what about **version** tags? Aren't the `server` and `client`
subpackages missing them?

Not really. Remember that if a subpackage is missing a given tag, it
will inherit the value of that tag from the main preamble. We're well on
our way to having a complete spec file, but we aren't quite there yet.

Let's continue by looking at the next part of the spec file that changes
when building subpackages.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-subpack-files-list">The **%files** List</span>

In an ordinary single-package spec file, the **%files** list is used to
determine which files are actually going to be packaged. It is no
different when building subpackages. What *is* different, is that there
must be a **%files** list for each subpackage.

Since each **%files** list must be associated with a particular
**%package** directive, we simply label each **%files** list with the
name of the subpackage, as specified by each **%package** directive.
Going back to our example, our **%package** lines were:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%package server
%package client
%package -n bazlib

          </code></pre></td>
</tr>
</tbody>
</table>

Therefore, our **%files** lists should start with:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%files server
%files client
%files -n bazlib

          </code></pre></td>
</tr>
</tbody>
</table>

In addition, we need the main package's **%files** list, which remains
unnamed:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%files

          </code></pre></td>
</tr>
</tbody>
</table>

The contents of each **%files** list is dictated entirely by the
software's requirements. If, for example, a certain file needs to be
packaged in more than one package, it's perfectly all right to include
the filename in more than one list.

<div class="sect3">

### <span id="s3-rpm-subpack-controlling-packages">Controlling Packages With the **%files** List</span>

The **%files** list wields considerable power over subpackages. It's
even possible to prevent a package from being created by using the
**%files** list. But is there a reason why you'd want to go to the
trouble of setting up subpackages, only to keep one from being created?

Actually, there is. Take, for example, the case where
client/server-based software is to be packaged. Certainly, it makes
sense to create two subpackages: one for the client and one for the
server. But what about the main package? Is there any need for it?

Quite often there's no need for a main package. In those cases, removing
the main **%files** list entirely will result in no main package being
built.

</div>

<div class="sect3">

### <span id="s3-rpm-subpack-point-worth-noting">A Point Worth Noting</span>

Please keep in mind that an empty **%files** list (ie, a **%files** list
that contains no files) is *not* the same as not having a **%files**
list at all. As we noted above, entirely removing a **%files** list
results in RPM not creating that package. However, if RPM comes across a
**%files** list with no files, it will happily create an empty package
file.

This feature (which also works with subpackage **%files** lists) comes
in handy when used in concert with conditionals. If a **%files** list is
enclosed by a conditional, the package will be created (or not) based on
the evaluation of the conditional.

</div>

<div class="sect3">

### <span id="s3-rpm-subpack-spec-file-revisited">Our Spec File So Far…</span>

Ok, let's update our example spec file. Here's what it looks like after
adding each of the subpackages' **%files** lists:

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

%package client
Summary: The foo client
Group: bogus/junque

%package -n bazlib
Version: 5.6
Summary: The baz library
Group: bogus/junque


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

As you can see we've added **%files** lists for:

  - The main `foo` package.

  - The `foo-server` subpackage.

  - The `foo-client` subpackage.

  - The `bazlib` subpackage.

Each package contains a single file.
[<span class="footnote">\[1\]</span>](#FTN.AEN11083) If there was no
need for a main package, we could simply remove the unnamed **%files**
list. Keep in mind that even if you do not create a main package, the
tags defined in the main package's preamble *will* appear somewhere —
specifically, in the source package file.

Let's look at the last subpackage-specific part of the spec file: the
install- and erase-time scripts.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-subpack-install-erase-scripts">Install- and Erase-time Scripts</span>

The install- and erase-time scripts, **%pre**, **%preun**, **%post**,
and **%postun**, can all be named using exactly the same method as was
used for the other subpackage-specific sections of the spec file. The
script used during package verification, **%verifyscript**, can be made
package-specific as well. Using the subpackage structure from our
example spec file, we would end up with script definitions like:

  - **%pre server**

  - **%postun client**

  - **%preun -n bazlib**

  - **%verifyscript client**

Other than the change in naming, there's only one thing to be aware of
when creating scripts for subpackages. It's important that you consider
the possibility of scripts from various subpackages interacting with
each other. Of course, this is simply good script-writing practice, even
if the packages involved are not related.

<div class="sect3">

### <span id="s3-rpm-subpack-spec-file-listing-scripts">Back At the Spec File…</span>

Here we've added some scripts to our spec file. So that our example
doesn't get too complex, we've just added preinstall scripts for each
package:

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


%pre
echo &quot;This is the foo package preinstall script&quot;

%pre server
echo &quot;This is the foo-server subpackage preinstall script&quot;

%pre client
echo &quot;This is the foo-client subpackage preinstall script&quot;

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

As pre-install scripts go, these don't do very much. But they will allow
us to see how subpackage-specific scripts can be defined.

Those of you that have built packages before probably realize that our
spec file is missing something. Let's add that part now.

</div>

</div>

</div>

### Notes

|                                                                                       |                                        |
| ------------------------------------------------------------------------------------- | -------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-subpack-spec-file-changes.html#AEN11083) | Hey, we said it was a simple example\! |

<div class="NAVFOOTER">

-----

|                                             |                           |                                                |
| :------------------------------------------ | :-----------------------: | ---------------------------------------------: |
| [Prev](s1-rpm-subpack-example-intro.html)   |    [Home](index.html)     | [Next](s1-rpm-subpack-build-time-scripts.html) |
| Our Example Spec File: Subpackages Galore\! | [Up](ch-rpm-subpack.html) |  Build-Time Scripts: Unchanged For Subpackages |

</div>
