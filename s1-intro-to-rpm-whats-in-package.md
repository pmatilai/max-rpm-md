<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-intro-to-rpm-rpm-design-goals.md)

Chapter 1. An Introduction to Package Management

[Next](s1-intro-to-rpm-lets-get-started.md)

-----

<div class="sect1">

# <span id="s1-intro-to-rpm-whats-in-package">What's in a package?</span>

With all the magical things we've claimed that package management
software in general (and RPM in particular) can do, you'd think there
was a tiny computer guru bundled in every package. However, the reality
is not that magical. Here's a quick overview of the more important parts
of an RPM package [<span class="footnote">\[1\]</span>](#FTN.AEN376) .

<div class="sect2">

## <span id="s2-intro-to-rpm-package-labels">RPM's Package Labels</span>

Every package built for RPM has to have a specific set of information
that uniquely identifies it. We call this information a package label.
Here are two sample package labels:

  - `nls-1.0-1`

  - `perl-5.001m-4`

While these labels look like they have very little in common, in fact
they all follow RPM's package labeling convention. There are three
different components in every package label. Let's look at each one in
order:

<div class="sect3">

### <span id="s3-intro-to-rpm-software-name">Component \#1: The Software's Name</span>

Every package label begins with the name of the software. The name may
be derived from the name of the application packaged, or it may be a
name describing a group of related programs bundled together by the
package builder. The software names in the packages listed above are:
`nls` and `perl`. As you can see, the software name is separated from
the rest of the package label by a dash.

</div>

<div class="sect3">

### <span id="s3-intro-to-rpm-software-version">Component \#2: The Software's Version</span>

Next in the package label is an identifier that describes the version of
the software being packaged. If the package builder bundled a number of
related programs together, the software version is probably a number of
their own choosing. However, if the package consists of one major
application, the software version normally comes directly from the
application's developer. The actual version specification is quite
flexible, as can be seen in the examples above. The versions shown are:
`1.0` and `5.001m`. A dash separates the software version from the
remainder of the package label.

</div>

<div class="sect3">

### <span id="s3-intro-to-rpm-package-release">Component \#3: The Package's Release</span>

The package release is the most unambiguous part of a package label. It
is a number chosen by the package builder. It reflects the number of
times the package has been rebuilt using the same version software.
Normally, the rebuilds are due to bugs uncovered after the package has
been in use for a while. By tradition, the package release starts at
`1`. The package releases in the example above are: `1` and `4`.

</div>

</div>

<div class="sect2">

## <span id="s2-intro-to-rpm-labels-names">Labels And Names: Similar, But Distinct</span>

Package labels are used internally by RPM. For example, if you ask RPM
to list every installed package, it will respond with a list of package
labels. When a package file is created, part of the filename consists of
the package label. There is no technical requirement for this, but it
does make it easier to keep track of things.

However, a package file may be renamed, and the new filename won't
confuse RPM in the least. That's because the package label is contained
within the file. For a fairly technical view of the inside of a package
file, refer to [Appendix A](ch-rpm-file-format.md).

</div>

<div class="sect2">

## <span id="s2-intro-to-rpm-package-wide-information">Package-wide Information</span>

Some of the information contained in a package is general in nature.
This information includes such items as:

  - The date and time the package was built.

  - A description of the package's contents.

  - The total size of all the files installed by the package.

  - Information that allows the package to be grouped with similar
    packages.

  - A digital "signature" that can be used to verify the authenticity
    and integrity of the package.
    [<span class="footnote">\[2\]</span>](#FTN.AEN434)

</div>

<div class="sect2">

## <span id="s2-intro-to-rpm-per-file-information">Per-file Information</span>

Each package also contains information about every file contained in the
package. The information includes:

  - The name of every file and where it is to be installed.

  - Each file's permissions.

  - Each file's owner and group specifications.

  - The MD5 checksum of each file.
    [<span class="footnote">\[3\]</span>](#FTN.AEN452)

  - The file's contents.

</div>

</div>

### Notes

|                                                                                     |                                                                                                                                                                                             |
| ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-intro-to-rpm-whats-in-package.md#AEN376) | See [Appendix A](ch-rpm-file-format.md) for complete details on the contents of a .rpm file.                                                                                              |
| [<span class="footnote">\[2\]</span>](s1-intro-to-rpm-whats-in-package.md#AEN434) | For more information on RPM's signature checking capability, refer to [the Section called ***rpm -K** — What Does it Do?* in Chapter 7](ch-rpm-checksig.md#s1-rpm-checksig-what-it-does). |
| [<span class="footnote">\[3\]</span>](s1-intro-to-rpm-whats-in-package.md#AEN452) | We'll discuss MD5 checksums in greater detail in [the Section called *MD5 Checksum* in Chapter 6](ch-rpm-verify.md#s3-rpm-verify-md5-checksum).                                           |

<div class="NAVFOOTER">

-----

|                                               |                            |                                               |
| :-------------------------------------------- | :------------------------: | --------------------------------------------: |
| [Prev](s1-intro-to-rpm-rpm-design-goals.md) |     [Home](index.md)     | [Next](s1-intro-to-rpm-lets-get-started.md) |
| RPM Design Goals                              | [Up](ch-intro-to-rpm.md) |                             Let's Get Started |

</div>
