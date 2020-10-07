<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-anywhere-specifying-file-attributes.html)

[Next](s1-rpm-pgp-getting-ready.html)

-----

<div class="chapter">

# <span id="ch-rpm-pgp"></span>Chapter 17. Adding PGP Signatures to a Package

In this chapter, we'll explore the steps required to add a digital
signature to a package, using the software known as Pretty Good Privacy,
or PGP. If you've used PGP before, you probably know everything you'll
need to start signing packages in short order.

On the other hand, if you feel you need a bit more information on PGP
before starting, please refer to [Appendix G](ch-pgp-intro.html) for a
brief introduction. Once you feel comfortable with PGP, come on back and
learn how easy signing packages is…

<div class="sect1">

# <span id="s1-rpm-pgp-why-sign">Why Sign a Package?</span>

The reason for signing a package is to provide authentication. With a
signed package, it's possible for your user community to verify that the
package they have was in your possession at some time and has not been
changed since then. That "not changed" part is also a good reason to
sign your packages, as digital signatures are a very robust way to guard
against any modifications to the package.

Of course, as with anything else in life, adding a digital signature to
a package isn't an ironclad guarantee that everything is right with the
package, but it's about as sure a thing as humans can make it.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                                         |                    |                                       |
| :------------------------------------------------------ | :----------------: | ------------------------------------: |
| [Prev](s1-rpm-anywhere-specifying-file-attributes.html) | [Home](index.html) | [Next](s1-rpm-pgp-getting-ready.html) |
| Specifying File Attributes                              |  [Up](p5206.html)  |                 Getting Ready to Sign |

</div>
