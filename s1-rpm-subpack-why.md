<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-subpack.md)

Chapter 18. Creating Subpackages

[Next](s1-rpm-subpack-example-intro.md)

-----

<div class="sect1">

# <span id="s1-rpm-subpack-why">Why Are They Needed?</span>

If all the software in the world followed the usual "one source, one
binary" structure, there would be no need for subpackages. After all,
RPM handles the building and packaging of a program into a single
package file just fine.

But software doesn't always conform to this simplistic structure. It's
not unusual for software to support two or more different modes of
operation. A client/server program, for example, comes in two flavors: a
client, and a server.

And it can get more complicated than that. Sometimes software relies on
another program so completely that the two cannot be built separately.
The result is often several packages.

While it is certainly possible that some convoluted procedure could be
devised to force these kinds of software into a single-package
structure, it makes more sense to let RPM manage the creation of
subpackages. Why? From the package builder's viewpoint, the main reason
to use subpackages is to eliminate any duplication of effort.

By using subpackages, there's no need to maintain separate spec files
and endure the resulting headaches when new versions of the software
become available. By keeping everything in one spec file, new software
versions can be quickly integrated, and every related subpackage rebuilt
with a single command.

But that's enough of the preliminaries. Let's see how subpackages are
created.

</div>

<div class="NAVFOOTER">

-----

|                             |                           |                                             |
| :-------------------------- | :-----------------------: | ------------------------------------------: |
| [Prev](ch-rpm-subpack.md) |    [Home](index.md)     |   [Next](s1-rpm-subpack-example-intro.md) |
| Creating Subpackages        | [Up](ch-rpm-subpack.md) | Our Example Spec File: Subpackages Galore\! |

</div>
