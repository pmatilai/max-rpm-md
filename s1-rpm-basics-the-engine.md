<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-basics.md)

Chapter 10. The Basics of Developing With RPM

[Next](s1-rpm-basics-outputs.md)

-----

<div class="sect1">

# <span id="s1-rpm-basics-the-engine">The Engine: RPM</span>

At the center of the action is RPM. It performs a number of steps during
the build process:

  - Executes the commands and macros in the prep section of the spec
    file.

  - Checks the contents of the file list.

  - Executes the commands and macros in the build section of the spec
    file.

  - Executes the commands and macros in the install section of the spec
    file. Any macros in the file list are executed at this time, too.

  - Creates the binary package file.

  - Creates the source package file.

By using different options on the RPM command line, the build process
can be stopped at any of the steps above. This makes the initial
building of a package that much easier, as it is then possible to see
whether each step completed successfully before continuing on to the
next step.

</div>

<div class="NAVFOOTER">

-----

|                                   |                          |                                    |
| :-------------------------------- | :----------------------: | ---------------------------------: |
| [Prev](ch-rpm-basics.md)        |    [Home](index.md)    | [Next](s1-rpm-basics-outputs.md) |
| The Basics of Developing With RPM | [Up](ch-rpm-basics.md) |                        The Outputs |

</div>
