<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](p108.md)

[Next](s1-intro-to-rpm-package-management-how.md)

-----

<div class="chapter">

# <span id="ch-intro-to-rpm"></span>Chapter 1. An Introduction to Package Management

<div class="sect1">

# <span id="s1-intro-to-rpm-what-are-packages">What are Packages, and Why Manage Them?</span>

To answer that question, let's go back to the basics for a moment.
Computers process information. In order for this to happen, there are
some prerequisites:

  - A computer (*Obviously\!*).

  - Some information to process (Also obvious\!).

  - A program to do the processing (Still pretty obvious\!).

Unless these three things come together very little is going to happen,
information processing-wise. But each of these items have their own
requirements that need to be satisfied before things can get exciting.

Take the computer, for example. While it needs things like electricity
and a cool, dry place to operate, it also needs access to the other two
items — information and programs — in order to do its thing. The way to
get information and programs into a computer is to place them in the
computer's mass storage. These days, mass storage invariably means a
disk drive. Putting information and programs on the disk drive means
that they are stored as files. So much for the computer's part in this.

OK, let's look at the information. Does information have any particular
needs? Well, it needs sufficient space on the disk drive, but more
importantly, it needs to be in the proper format for the program that
will be processing it. That's it for information.

Finally, we have the program. What does it need? Like the information,
it needs sufficient disk space on the disk drive. But there are many
other things that it may need:

  - It may need information to process, in the correct format, named
    properly, and in the appropriate area on a disk drive somewhere.

  - It may need one or more configuration files. These are files that
    control the program's behavior and permit some level of
    customization. Like the information, these files must be in the
    proper format, named properly, and in the appropriate area on a
    disk. We'll be referring to them by their other name — *config*
    files — throughout the book.

  - It may need work areas on a disk, named properly, and located in the
    appropriate area.

  - It may even need other programs, each with their *own* requirements.

  - Although not strictly required by the program itself, the program
    may come with one or more files containing documentation. These
    files can be very handy for the humans trying to get the program to
    do their bidding\!

As you can imagine, this can get pretty complicated. It's not so bad
once everything is set up properly, but how do things get set up
properly in the first place? There are two possibilities:

1.  After reading the documentation that comes with the program you'd
    like to use, you copy the various programs, configuration files, and
    information onto your computer, making sure they are all named
    correctly, are located in the proper place, and that there is
    sufficient disk space to hold them all. You make the appropriate
    changes to the configuration file(s). Finally, you run any setup
    programs that are necessary, giving them whatever information they
    require to do their job.

2.  You let the computer do it.

If it seems like the first choice isn't so bad, consider how many files
you'll need to keep track of. On a typical Linux system, it's not
unusual to have over 20,000 different files. That's a *lot* of
documentation reading, file copying, and configuring\! And what happens
when you want a newer version of a program? More of the same\!

Some people think the second alternative is easier. RPM was made for
them.

<div class="sect2">

## <span id="s2-intro-to-rpm-enter-the-package">Enter the Package</span>

When you consider that computers are very good at keeping track of large
amounts of data, the idea of giving your computer the job of riding herd
over 20,000 files seems like a good one. And that's exactly what package
management software does. But what *is* a "package"?

A package in the computer sense is very similar to a package in the
physical sense. Both are methods of keeping related objects together in
the same place. Both need to be opened before the contents can be used.
Both can have a "packing slip" taped to the side, identifying the
contents.

Normally, package management systems take all the various files
containing programs, data, documentation, and configuration information,
and place them in one specially formatted file — a package file. In the
case of RPM, the package file is sometimes called a "package", a ".rpm
file", or even an "RPM". All mean the same thing — a package containing
software meant to be installed using RPM.

What types of software are normally found in a package? There are no
hard and fast rules, but normally a package's contents consist of one of
the following types of software:

  - A collection of one or more programs that perform a single
    well-defined task. This is normally what people think of as an
    "application". Word processors and programming languages would fit
    into this category.

  - A specific part of an operating system. Examples might be system
    initialization scripts, a particular command shell, or the software
    required to support a web server, for example.

<div class="sect3">

### <span id="s3-intro-to-rpm-advantages-of-package">Advantages of a Package</span>

One of the most obvious benefits to having a package is that the package
is one easily manageable chunk. If you move it from one place to
another, there's no risk of any part getting left behind. But although
this is the most obvious advantage, it's not the biggest one.

The biggest advantage is that the package can contain the *knowledge*
about what it takes to install itself on your computer. And if the
package contains the steps required to install itself, the package can
also contain the steps required to uninstall itself. What used to be a
painful manual process is now a straightforward procedure. What used to
be a mass of 20,000 files becomes a couple hundred packages.

</div>

</div>

<div class="sect2">

## <span id="s2-intro-to-rpm-manage-your-packages">Manage Your Packages, or They Will Manage You</span>

A couple *hundred*? Even though the use of packages has decreased the
complexity of managing a system by an order of magnitude, it hasn't yet
gotten to the level of being a "no-brainer". It's still necessary to
keep track of what packages are installed on your system. And if there
are some packages that require other packages in order to install or
operate correctly, these should be tracked as well.

<div class="sect3">

### <span id="s3-intro-to-rpm-packages-lead-active-lives">Packages Lead Active Lives</span>

If you start looking at a computer system as a collection of packages,
you'll find that a distinct set of operations will take place on those
packages time and time again:

  - New packages are installed. Maybe it's a spreadsheet you'll use to
    keep track of expenses, or the latest shoot-em-up game, but in
    either case it's new and you want it.

  - Old packages are replaced with newer versions. Whoever wrote the
    word processor you use daily, comes out with a new version. You'll
    probably want to install the new version and remove the old one.

  - Packages are removed entirely. Perhaps that over-hyped strategy game
    just didn't cut it. You have better things to do with that disk
    space, so get rid of it\!

With this much activity going on, it's easy to lose track of things.
What types of package information should be available to keep you
informed?

</div>

<div class="sect3">

### <span id="s3-intro-to-rpm-keeping-track-of-packages">Keeping Track of Packages</span>

Just as there are certain operations that are performed on packages,
there are also certain types of information that will make it easier to
make sense of the packages installed on your system:

  - Certainly you'd like to be able to see what packages are installed.
    It's easy to forget if that fax program you tried a few months ago
    is still installed or not.

  - It would be nice to be able to get more detailed information on a
    specific package. This might consist of anything from the date the
    package was installed, to a list of files it installed on your
    system.

  - Being able to access this information in a variety of ways can be
    helpful, too. Instead of simply finding out what files a package
    installed, it might be handy to be able to name a particular file
    and find out which package installed it.

  - If this amount of detail is possible, then it should be possible to
    see if the way a package is presently installed varies from the way
    it was originally installed. It's not at all unusual to make a
    mistake and delete one file — or a hundred. Being able to tell if
    one or more packages are missing files could greatly simplify the
    process of getting an ailing system back on its feet again.

  - Files containing configuration information can be a real headache.
    If it were possible to pay extra attention to these files and make
    sure any changes made to them weren't lost, life would certainly be
    a lot easier.

</div>

</div>

</div>

</div>

<div class="NAVFOOTER">

-----

|                                                                             |                    |                                                     |
| :-------------------------------------------------------------------------- | :----------------: | --------------------------------------------------: |
| [Prev](p108.md)                                                           | [Home](index.md) | [Next](s1-intro-to-rpm-package-management-how.md) |
| RPM and Computer Users — How to Use RPM to Effectively Manage Your Computer |  [Up](p108.md)   |                   Package Management: How to Do It? |

</div>
