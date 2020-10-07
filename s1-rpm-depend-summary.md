<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-depend-manual-dependencies.md)

Chapter 14. Adding Dependency Information to a Package

[Next](ch-rpm-reloc.md)

-----

<div class="sect1">

# <span id="s1-rpm-depend-summary">To Summarize…</span>

RPM's dependency processing is based on tracking the capabilities a
package provides, and the capabilities a package requires. A package's
requirements can come from two places:

1.  Shared library requirements, automatically determined by RPM.

2.  The **Requires** tag line, manually added to the package's spec
    file.

These requirements can be viewed by using RPM's **--requires** query
option. A specific requirement can be viewed by using the
**--whatrequires** query option. Both options are fully described in
[Chapter 5](ch-rpm-query.md).

The capabilities a package provides, can come from three places:

1.  Shared library sonames, automatically determined by RPM.

2.  The **Provides** tag line, manually added to the package's spec
    file.

3.  The package's name (and optionally, version/epoch number).

The first two types of information can be viewed by using RPM's
**--provides** query option. A specific capability can be viewed by
using the **--whatprovides** query option. Both options are fully
described in [Chapter 5](ch-rpm-query.md).

The package name and version are not considered capabilities that are
explicitly provided. Therefore, if a search using **--provides** or
**--whatprovides** comes up dry, try simply looking for a package by
that name.

As you've probably gathered by now, using manual dependencies requires
some level of synchronization between packages. This can be tricky,
particularly if you're not responsible for both packages. But RPM's
dependency processing can make life easier for your users.

</div>

<div class="NAVFOOTER">

-----

|                                                |                          |                              |
| :--------------------------------------------- | :----------------------: | ---------------------------: |
| [Prev](s1-rpm-depend-manual-dependencies.md) |    [Home](index.md)    |    [Next](ch-rpm-reloc.md) |
| Manual Dependencies                            | [Up](ch-rpm-depend.md) | Making a Relocatable Package |

</div>
