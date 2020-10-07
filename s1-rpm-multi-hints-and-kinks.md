<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-multi-platform-dependent-conditional.md)

Chapter 19. Building Packages for Multiple Architectures and Operating
Systems

[Next](ch-rpm-rw-build.md)

-----

<div class="sect1">

# <span id="s1-rpm-multi-hints-and-kinks">Hints and Kinks</span>

There isn't much in the way of hard and fast rules when it comes to
multi-platform package building. But in general, the following uses of
RPM's multi-platform capabilities seem to work the best:

  - The **exclude`xxx`** and **exclusive`xxx`** tags are best used when
    it's known there's no reason for the package to be built on specific
    architectures.

  - The **%if`xxx`** and **%ifn`xxx`** conditionals are most likely to
    be used in the following areas:
    
      - Controlling the inclusion of **%patch** macros for
        platform-specific patches.
    
      - Setting up platform-specific initialization prior to building
        the software.
    
      - Tailoring the **%files** list when the software creates
        platform-specific files.

Given that some software is more easily ported to different platforms
than others, this list is far from complete. If there's one thing to
remember about multi-platform package building, it's don't be afraid to
experiment\!

</div>

<div class="NAVFOOTER">

-----

|                                                          |                         |                              |
| :------------------------------------------------------- | :---------------------: | ---------------------------: |
| [Prev](s1-rpm-multi-platform-dependent-conditional.md) |   [Home](index.md)    | [Next](ch-rpm-rw-build.md) |
| Platform-Dependent Conditionals                          | [Up](ch-rpm-multi.md) |  Real-World Package Building |

</div>
