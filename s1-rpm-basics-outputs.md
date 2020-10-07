<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-basics-the-engine.html)

Chapter 10. The Basics of Developing With RPM

[Next](s1-rpm-basics-and-now.html)

-----

<div class="sect1">

# <span id="s1-rpm-basics-outputs">The Outputs</span>

The end product of this entire process is a source package file and a
binary package file.

<div class="sect2">

## <span id="s2-rpm-basics-source-package-file">The Source Package File</span>

The source package file is a specially formatted archive that contains
the following files:

  - The original compressed **tar** file(s).

  - The spec file.

  - The patches.

Since the source package contains everything needed to create the binary
package, the source package, *and* provide the original sources, it's a
great way to distribute source code. As mentioned earlier, it's also a
great way to archive all the information needed to rebuild a particular
version of the package.

</div>

<div class="sect2">

## <span id="s2-rpm-basics-binary-package">The Binary RPM</span>

The binary package file is the one part of the entire RPM building
process that is most visible to the user. It contains the files that
comprise the application, along with any additional information needed
to install and erase it. The binary package file is where the "rubber
hits the road."

</div>

</div>

<div class="NAVFOOTER">

-----

|                                       |                          |                                    |
| :------------------------------------ | :----------------------: | ---------------------------------: |
| [Prev](s1-rpm-basics-the-engine.html) |    [Home](index.html)    | [Next](s1-rpm-basics-and-now.html) |
| The Engine: RPM                       | [Up](ch-rpm-basics.html) |                           And Now… |

</div>
