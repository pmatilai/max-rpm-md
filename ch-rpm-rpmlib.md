<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-rw-build-package-building.html)

[Next](s1-rpm-rpmlib-functions.html)

-----

<div class="chapter">

# <span id="ch-rpm-rpmlib"></span>Chapter 21. A Guide to the RPM Library API

In this chapter, we'll explore the functions used internally by RPM.
These functions are available for anyone to use, making it possible to
add RPM functionality to new and existing programs. Rather than
continually refer to "the RPM library" throughout this chapter, we'll
use the name of the library's main include file — rpmlib.

<div class="sect1">

# <span id="s1-rpm-rpmlib-overview">An Overview of rpmlib</span>

There are a number of files that make up rpmlib. First and foremost, of
course, is the rpmlib library, `librpm.a`. This library contains all the
functions required to implement all the basic functions contained in
RPM.

The remaining files define the various data structures, parameters, and
symbols used by rpmlib:

  - `rpmlib.h`

  - `dbindex.h`

  - `header.h`

In general, `rpmlib.h` will always be required. When using rpmlib's
header-related functions, `header.h` will be required, while the
database-related function will require `dbindex.h`. As each function is
described in this chapter, we'll provide the function's prototype as
well as the **\#include** statements the function requires.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                               |                    |                                      |
| :-------------------------------------------- | :----------------: | -----------------------------------: |
| [Prev](s1-rpm-rw-build-package-building.html) | [Home](index.html) | [Next](s1-rpm-rpmlib-functions.html) |
| Package Building                              |  [Up](p5206.html)  |                     rpmlib Functions |

</div>
