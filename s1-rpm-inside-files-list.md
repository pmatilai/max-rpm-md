<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-inside-macros.md)

Chapter 13. Inside the Spec File

[Next](s1-rpm-inside-files-list-directives.md)

-----

<div class="sect1">

# <span id="s1-rpm-inside-files-list">The **%files** List</span>

The **%files** list indicates to RPM which files on the build system are
to be packaged. The list consists of one file per line. The file may
have one or more directives preceding it. These directives give RPM
additional information about the file and are discussed more fully
below.

Normally, each file includes its full path. The path performs two
functions. First, it specifies the file's location on the build system.
Second, it denotes where the file should be placed when the package is
to be installed. [<span class="footnote">\[1\]</span>](#FTN.AEN8695)

For packages that create directories containing hundreds of files, it
can be quite cumbersome creating a list that contains every file. To
make this situation a bit easier, if the **%files** list contains a path
to a directory, RPM will automatically package every file in that
directory, as well as every file in each subdirectory. Shell-style
globbing can also be used in the **%files** list.

</div>

### Notes

|                                                                              |                                                                                                                                                             |
| ---------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-inside-files-list.md#AEN8695) | This is not entirely the case when a relocatable package is being built. For more information on relocatable packages, see [Chapter 15](ch-rpm-reloc.md). |

<div class="NAVFOOTER">

-----

|                                                |                          |                                                  |
| :--------------------------------------------- | :----------------------: | -----------------------------------------------: |
| [Prev](s1-rpm-inside-macros.md)              |    [Home](index.md)    | [Next](s1-rpm-inside-files-list-directives.md) |
| Macros: Helpful Shorthand for Package Builders | [Up](ch-rpm-inside.md) |               Directives For the **%files** list |

</div>
