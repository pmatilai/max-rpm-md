<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-build-starting-build.html)

Chapter 11. Building Packages: A Simple Example

[Next](ch-rpm-b-command.html)

-----

<div class="sect1">

# <span id="s1-rpm-build-when-things-go-wrong">When Things Go Wrong</span>

This example is a bit of a fairy tale, in that it went perfectly the
first time. In real life, it often takes several tries to get it right.

<div class="sect2">

## <span id="s2-rpm-build-problems-during-build">Problems During the Build</span>

As we alluded to earlier in the chapter, RPM can stop at various points
in the build process. This allows package builders to look through the
build directory and make sure everything is proceeding properly. If
there are problems, stopping during the build process permits them to
see exactly what is going wrong, and where. Here is a list of points RPM
can be stopped at during the build:

  - After the **%prep** section.

  - After doing some cursory checks on the **%files** list.

  - After the **%build** section.

  - After the **%install** section.

  - After the binary package has been created.

In addition, there is also a method that permits the package builder to
"short circuit" the build process and direct RPM to skip over the
initial steps. This is handy when the application is not yet ready for
packaging and needs some fine tuning. This way, once the package builds,
installs, and operates properly, the required patches to the original
sources can be created, and plugged into the package's spec file.

</div>

<div class="sect2">

## <span id="s2-rpm-build-testing-packages">Testing Newly Built Packages</span>

Of course, the fact that an application has been packaged successfully
doesn't necessarily mean that it will operate correctly when the package
is actually installed. Testing is required. In the case of our example,
it's perfect and doesn't need such testing.
[<span class="footnote">\[1\]</span>](#FTN.AEN5947) But here is how
testing would proceed:

The first step is to find a test system. If you thought of simply using
the build system, *bzzzzt*, try again\! Think about it — in the course
of building the package, the build system actually had the application
installed on it. That is how RPM gets the files that are to be packaged:
by building the software, installing it, and grabbing copies of the
installed files, which are found using the **%files** list.

Some of you dissenters that have read the first half of the book might
be thinking, "Why not just install the package on the build system using
the **--replacefiles** option? That way, it'll just blow away the files
installed by the build process and replace them with the packaged
files." Well, you folks get a *bzzzzt*, too\! Here's why.

Say, for example, that the software you're packaging installs a bunch of
files — maybe a hundred. What does this mean? Well for one thing, it
means that the package's **%files** list is going to be quite large. For
another thing, the sheer number of files makes it likely that you'll
miss one or two. What would happen then?

When RPM builds the software, there's no problem: the software builds,
and the application's makefile merrily installs all the files. The next
step in RPM's build process is to collect the files by reading the
**%files** list, and to add each file listed to a **cpio** archive. What
happens to the files you've missed? Nothing — they aren't added to the
package file, but they *are* on your build system, installed just where
they should be.

Next, when the package is installed using **--replacefiles**, RPM
dutifully installs each of the packaged files, replacing the ones
originally installed on the build system. The missed files? They aren't
overwritten by RPM since they weren't in the package. But they're still
on disk, right where the application expects them to be\! If you go to
test the application then, it will find every file it needs. But not
every file came from the package. Bad news\! Using a different system on
which the application had never been built is one sure way to test for
missing files.

That wraps up our fictional build. Now that we have some experience with
RPM's build process, we can take a more in-depth look at RPM's build
command.

</div>

</div>

### Notes

|                                                                                       |                                   |
| ------------------------------------------------------------------------------------- | --------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-build-when-things-go-wrong.html#AEN5947) | Like we said, it's a fairy tale\! |

<div class="NAVFOOTER">

-----

|                                          |                         |                                |
| :--------------------------------------- | :---------------------: | -----------------------------: |
| [Prev](s1-rpm-build-starting-build.html) |   [Home](index.html)    |  [Next](ch-rpm-b-command.html) |
| Starting the Build                       | [Up](ch-rpm-build.html) | **rpmbuild** Command Reference |

</div>
