<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-multi.md)

Chapter 19. Building Packages for Multiple Architectures and Operating
Systems

[Next](s1-rpm-multi-build-install-detection.md)

-----

<div class="sect1">

# <span id="s1-rpm-multi-multi-platform-easier">What Does RPM Do To Make Multi-Platform Packaging Easier?</span>

As we mentioned above, RPM supports multi-platform package building
through a set of tags, `rpmrc` file entries, and conditionals. None of
these tools are difficult to use. In fact, the hardest part of
multi-platform package building is figuring out how the software needs
to be changed to support different platforms.

Let's take a look at each multi-platform tool RPM provides.

<div class="sect2">

## <span id="s2-rpm-multi-auto-detection-build">Automatic Detection of Build Platform</span>

The first thing necessary for easy multi-platform package building is to
identify which platform the package is to be built for. Except in the
fairly esoteric case of cross-compilation, the build platform is the
platform on which the package is built. RPM does this for you
automatically, although it can be overridden at build-time.

</div>

<div class="sect2">

## <span id="s2-rpm-multi-auto-detection-install">Automatic Detection of Install Platform</span>

The other important platform in package building is the platform on
which the package is to be installed. Here again, RPM does this for you,
though it's possible to override this when the package is installed.

But there is more to multi-platform package building than simply being
able to determine the platform during package building and installation.
The next component in multi-platform package building is a set of
platform-dependent tags.

</div>

<div class="sect2">

## <span id="s2-rpm-multi-platform-tags">Platform-Dependent Tags</span>

RPM uses a number of tags that control which platforms can build a
package. These tags make it easier for the package builder to build
multiple packages automatically, since the tags will keep RPM from
attempting to build packages that are incompatible with the build
platform.

</div>

<div class="sect2">

## <span id="s2-rpm-multi-platform-conditionals">Platform-Dependent Conditionals</span>

While the platform-dependent tags provide a crude level of
multi-platform control (i.e., the package will be built or not,
depending on the tags and the build platform), RPM's platform-dependent
conditionals provide a much finer level of control. By using these
conditionals, it's possible to excise those parts of the spec file that
are specific to another platform and replace them with one or more lines
that are compatible with the build platform.

Now that we have a basic idea of RPM's multi-platform support features,
let's take a more in-depth look at each one.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                                                    |                         |                                                   |
| :----------------------------------------------------------------- | :---------------------: | ------------------------------------------------: |
| [Prev](ch-rpm-multi.md)                                          |   [Home](index.md)    | [Next](s1-rpm-multi-build-install-detection.md) |
| Building Packages for Multiple Architectures and Operating Systems | [Up](ch-rpm-multi.md) |              Build and Install Platform Detection |

</div>
