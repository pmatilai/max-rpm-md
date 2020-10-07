<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-subpack-building-subpackages.md)

[Next](s1-rpm-multi-multi-platform-easier.md)

-----

<div class="chapter">

# <span id="ch-rpm-multi"></span>Chapter 19. Building Packages for Multiple Architectures and Operating Systems

While RPM certainly makes packaging software as easy as possible, it
doesn't end there. RPM gives you the tools you need to build a package
on different types of computers. More importantly, RPM makes it possible
to build packages on different types of computers *using a single spec
file*. Those of you that have developed software for different computers
know the importance of maintaining a single set of sources. RPM lets you
continue that practice through the package building phase.

Before we get into RPM's capabilities, let's do a quick review of what
is involved in developing software for different types of computer
systems.

<div class="sect1">

# <span id="s1-rpm-multi-primer">Architectures and Operating Systems: A Primer</span>

From a software engineering standpoint, there are only two major
differences between any two computer systems:

1.  The architecture implemented by the computer's hardware.

2.  The system software running on the computer.

The first difference is built into the computer. The architecture is the
manner in which the computer system was designed. It includes the number
and type of registers present in the processor, the number of machine
instructions, what operations they perform, and so on. For example,
every "PC" today, no matter who built it, is based on the Intel x86
architecture.

The second difference is more under our control. The operating system is
software that controls how the system operates. Different operating
systems have different methods of storing information on disk, different
ways of implementing functions used by programs, and different hardware
requirements.

As far as package building is concerned, two systems with the same
architecture running two different operating systems, are as different
as two systems with different architectures running the same operating
system. In the first case, the software being packaged for different
operating systems will differ due to the differences between the
operating systems. In the second case, the software being packaged for
different architectures will differ due to the underlying differences in
hardware. [<span class="footnote">\[1\]</span>](#FTN.AEN11266)

RPM supports differences in architecture and operating system equally.
If there is a tag, `rpmrc` file entry, or conditional that is used to
support architectural differences, there is a corresponding tag, entry,
or conditional that supports operating system differences.

<div class="sect2">

## <span id="s2-rpm-multi-platforms">Let's Just Call Them Platforms</span>

In order to keep the duplication in this chapter to a minimum, we'll
refer to a computer of a given architecture running a given operating
system as a *platform*. If another system differs in either aspect, it
is considered a different platform.

OK, now that we've gotten through the preliminaries, let's look at RPM's
multi-platform capabilities.

</div>

</div>

</div>

### Notes

|                                                                   |                                                                                                                                                                                                  |
| ----------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [<span class="footnote">\[1\]</span>](ch-rpm-multi.md#AEN11266) | This is a somewhat simplistic view of the matter, as it's common for incompatibilities to crop up between two different implementations of the same operating system on different architectures. |

<div class="NAVFOOTER">

-----

|                                                  |                    |                                                           |
| :----------------------------------------------- | :----------------: | --------------------------------------------------------: |
| [Prev](s1-rpm-subpack-building-subpackages.md) | [Home](index.md) |           [Next](s1-rpm-multi-multi-platform-easier.md) |
| Building Subpackages                             |  [Up](p5206.md)  | What Does RPM Do To Make Multi-Platform Packaging Easier? |

</div>
