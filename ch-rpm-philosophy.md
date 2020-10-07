<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](p5206.html)

[Next](s1-rpm-philosophy-easy-builds.html)

-----

<div class="chapter">

# <span id="ch-rpm-philosophy"></span>Chapter 9. The Philosophy Behind RPM

As we saw in the first half of this book, RPM can make life much easier
for the user. With automated installs, upgrades, and erasures, RPM can
take a lot of the guesswork out of keeping a computer system up-to-date.

But what about people that sling code for a living? Does RPM have
anything to offer them? The answer is *yes*\! One of the best things
about RPM is that although it was designed to make life easier for
users, it was written by people that would be using it to build *many*
packages. So the design philosophy of RPM has a definite bias toward
making life easier for developers. Here are some of the reasons you
should consider building packages with RPM:

<div class="sect1">

# <span id="s1-rpm-philosophy-pristine-sources">Pristine Sources</span>

While many developers might use RPM to package their own software, just
as many, if not more, are going to be packaging software that they have
not written. Because of this, there are some aspects to RPM's design
that are geared toward "third-party" package builders. One such aspect
is RPM's use of "pristine" sources.

When a third-party package builder decides to package someone else's
software, they often get the software from the Net, normally as a
**tar** file compressed with something like GNU zip. That's probably
about the only generalization we can make when talking about software
that is eligible for packaging. Once we look inside the **tar** file,
there are a world of possible differences:

  - The application could be available in pure source form, in pure
    binary form, or some combination of both.

  - The application might have been written to be built using **make**,
    **imake**, or a script included with the sources. Or, it might have
    to be built entirely by hand.

  - The application might need to be configured prior to use. Maybe it
    uses GNU **configure**, a custom configuration script, or one or
    more files that need to be edited to reflect the target environment.

  - The application might have been written to reside in specific
    directories, and those directories do not exist, or are not
    appropriate on the target system.

  - The application might not even support the target environment,
    requiring all manner of changes to port it to the target
    environment.

We could go on, but you probably get the idea. It's a rare application
that comes off the Net ready to package, and the changes required vary
widely. What to do?

This is where the concept of pristine sources comes in. RPM has been
designed to use the sources as they come from the application's
developer, no matter how it has been packaged and configured. The main
benefit is that the changes you as a package builder need to make,
remain separate from the original sources, in a distinct collection of
patches.

This may not sound like much of an advantage, but consider how this
would work if a new version of the application came out. If the new
version had a few localized bug fixes, it's entirely possible the
original patches could be applied, and a new package built, with a
single RPM command. Even if the patches didn't apply cleanly, it would
at least give an indication as to what might need to be done to get the
new version built and packaged.

If your users sometimes customize packages, having pristine sources
makes it easier for them, too. They can see what patches you've created
and can easily add their own.

Another benefit to using pristine sources is that it makes keeping track
of multiple versions of a package simple. Instead of keeping patched
sources around, or battling a revision control system, it's only
necessary to keep:

  - The original sources in their tar file.

  - A copy of the patches you applied to get the application to build.

  - A file used by RPM to control the package building process.

With these three items, it's possible to easily build the package at any
time. Keeping track of multiple versions only entails keeping track of
each version of these three components, rather than hundreds or
thousands of patched source files.

In fact, it gets better than that. RPM can also build a source package
containing these three components. The source package, named using RPM's
standard naming convention, keeps everything you need to recreate a
specific version of a package, in one uniquely named file. Keeping track
of multiple versions of multiple packages is simply a matter of keeping
the appropriate source packages around. Everything else can be built
from them.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                                                           |                    |                                            |
| :------------------------------------------------------------------------ | :----------------: | -----------------------------------------: |
| [Prev](p5206.html)                                                        | [Home](index.html) | [Next](s1-rpm-philosophy-easy-builds.html) |
| RPM and Developers — How to Distribute Your Software More Easily With RPM |  [Up](p5206.html)  |                                Easy Builds |

</div>
