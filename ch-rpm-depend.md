<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-inside-conditionals.html)

[Next](s1-rpm-depend-auto-depend.html)

-----

<div class="chapter">

# <span id="ch-rpm-depend"></span>Chapter 14. Adding Dependency Information to a Package

Since the very first version of RPM hit the streets, one of the side
effects of RPM's ease of use was that it made it easier for people to
break things. Since RPM made it so simple to erase packages, it became
common for people to joyfully erase packages until something broke.

Usually this only bit people once, but even once was too much of a
hassle if it could be prevented. With this in mind, the RPM developers
gave RPM the ability to:

  - Build packages that contain information on the capabilities they
    require.

  - Build packages that contain information on the capabilities they
    provide.

  - Store this "provides" and "requires" information in the RPM
    database.

In addition, they made sure RPM was able to display dependency
information, as well as to warn users if they were attempting to do
something that would break a package's dependency requirements.

With these features in place, it became more difficult for someone to
unknowingly erase a package and wreak havoc on their system.

<div class="sect1">

# <span id="s1-rpm-depend-overview">An Overview of Dependencies</span>

We've already alluded to the underlying concept for RPM's dependency
processing. It is based on two key factors:

  - Packages advertise what capabilities they provide.

  - Packages advertise what capabilities they require.

By simply checking these two types of information, many possible
problems can be avoided. For example, if a package requires a capability
that is not provided by any already-installed package, that package
cannot be installed and expected to work properly.

On the other hand, if a package is to be erased, but its capabilities
are required by other installed packages, then it cannot be erased
without causing other packages to fail.

As you might imagine, it's not quite *that* simple. But adding
dependency information can be easy. In fact, in most cases, it's
automatic\!

</div>

</div>

<div class="NAVFOOTER">

-----

|                                         |                    |                                        |
| :-------------------------------------- | :----------------: | -------------------------------------: |
| [Prev](s1-rpm-inside-conditionals.html) | [Home](index.html) | [Next](s1-rpm-depend-auto-depend.html) |
| Conditionals                            |  [Up](p5206.html)  |                 Automatic Dependencies |

</div>
