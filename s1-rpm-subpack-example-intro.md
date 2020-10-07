<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-subpack-why.html)

Chapter 18. Creating Subpackages

[Next](s1-rpm-subpack-spec-file-changes.html)

-----

<div class="sect1">

# <span id="s1-rpm-subpack-example-intro">Our Example Spec File: Subpackages Galore\!</span>

Throughout this chapter, we'll be constructing a spec file that will
consist of a number of subpackages. Let's start by listing the spec
file's requirements:

  - The main package name is to be `foo`.

  - The version is to be 2.7.

  - There are three subpackages:
    
      - The server subpackage, to be called `foo-server`.
    
      - The client subpackage, to be called `foo-client`.
    
      - The `baz` development library subpackage, to be called `bazlib`.

  - The `bazlib` subpackage has a version of 5.6.

  - Each subpackage will have its own **summary** and **description**
    tags.

Every spec file starts with a preamble, and this one is no different. In
this case, the preamble will contain the following tags:

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

        </code></pre></td>
</tr>
</tbody>
</table>

As we can see, there's nothing different here: this is an ordinary spec
file so far. Let's delve into things a bit more and see what we'll need
to add to this spec file to create the subpackages we require.

</div>

<div class="NAVFOOTER">

-----

|                                 |                           |                                               |
| :------------------------------ | :-----------------------: | --------------------------------------------: |
| [Prev](s1-rpm-subpack-why.html) |    [Home](index.html)     | [Next](s1-rpm-subpack-spec-file-changes.html) |
| Why Are They Needed?            | [Up](ch-rpm-subpack.html) |             Spec File Changes For Subpackages |

</div>
