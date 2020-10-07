<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-intro-to-rpm-package-management-how.html)

Chapter 1. An Introduction to Package Management

[Next](s1-intro-to-rpm-whats-in-package.html)

-----

<div class="sect1">

# <span id="s1-intro-to-rpm-rpm-design-goals">RPM Design Goals</span>

The design goals of RPM could best be summed up with the phrase
"something for everyone". While the main reason for the existence of RPM
was to make it easier for Red Hat to build the several hundred packages
that comprised their Linux distribution, it was not the only reason RPM
was created. Let's take a look at the various requirements the Red Hat
team used in their design of RPM:

<div class="sect2">

## <span id="s2-intro-to-rpm-packages-on-off">Make it easy to get packages on and off the system</span>

As we've seen earlier in this chapter, the act of installing a package
can involve many complex steps. Entrusting these steps to a person who
may not have the necessary experience is a strategy for failure. So the
goal for RPM was to make it as easy as possible for *anyone* to install
packages. The same holds true for removing packages. It is a complex and
error-prone operation, and one that RPM should handle for the user.

The other side of this issue is that RPM should give the package builder
almost total control in terms of *how* the package is installed. The
reason for this is simple: if the package builders do their homework,
their package should install and uninstall properly.

</div>

<div class="sect2">

## <span id="s2-intro-to-rpm-verify">Make it easy to verify a package was installed correctly</span>

Because software problems are a fact of life, the ability to verify the
proper installation of a package is vital. If done properly, it should
be possible to catch a variety of problems, including things such as
missing or modified files.

</div>

<div class="sect2">

## <span id="s2-intro-to-rpm-package-builder">Make it easy for the package builder</span>

While we're dedicating an entire book to package management, in reality
it should be a small portion of the package builder's job. Why? They've
got better things to do\! If they are the people that are actually
creating the software to be packaged, *that's* where they should be
spending the majority of their time.

Even if the package builder isn't actually writing software, they still
have better things to do than worry about building packages. For
instance, they may be responsible for building many packages. The less
time spent on building an individual package translates to more packages
that can be built.

</div>

<div class="sect2">

## <span id="s2-intro-to-rpm-original-source">Make it start with the original source code</span>

Delving a bit more into the package builder's world, it was deemed
important that RPM start with the original, unmodified source code. Why
is this so important?

Using the original sources makes it possible to separate the changes
required to build the package from any changes implemented to fix bugs,
add new features, or anything else. This is a good thing for package
builders, since many of them are *not* the original authors of the
programs they package.

This separation makes it easy, months down the road, to know exactly
what changes were made in order to get the package to build. This is
important when a new version of the packaged software becomes available.
Many times it's only necessary to apply the original "package building"
changes to the newer software. At worst, the changes provide a starting
point to determine what sorts of things might need to be changed in the
new version.

</div>

<div class="sect2">

## <span id="s2-intro-to-rpm-different-architectures">Make it work on different computer architectures</span>

One of the tougher things for a package builder to do is to take a
program, make it run on more than one type of computer, and distribute
packages for each. Because RPM makes it easy to take a program's
original source code, add the changes necessary to get it to build, and
produce a package for each architecture in one step, it can be pretty
handy.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                                     |                            |                                               |
| :-------------------------------------------------- | :------------------------: | --------------------------------------------: |
| [Prev](s1-intro-to-rpm-package-management-how.html) |     [Home](index.html)     | [Next](s1-intro-to-rpm-whats-in-package.html) |
| Package Management: How to Do It?                   | [Up](ch-intro-to-rpm.html) |                          What's in a package? |

</div>
