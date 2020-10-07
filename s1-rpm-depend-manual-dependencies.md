<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-depend-auto-depend.html)

Chapter 14. Adding Dependency Information to a Package

[Next](s1-rpm-depend-summary.html)

-----

<div class="sect1">

# <span id="s1-rpm-depend-manual-dependencies">Manual Dependencies</span>

You might have noticed that we've been using the words "requires" and
"provides" to describe the dependency relationships between packages. As
it turns out, these are the exact words used in spec files to manually
add dependency information. Let's look at the first tag: **Requires**.

<div class="sect2">

## <span id="s2-rpm-depend-requires-tag">The **Requires** Tag</span>

We've been deliberately vague when discussing exactly what it is that a
package requires. Although we've used the word "capabilities", in fact,
manual dependency requirements are always represented in terms of
packages. For example, if package `foo` requires that package `bar` is
installed, it's only necessary to add the following line to `foo`'s spec
file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Requires: bar

          </code></pre></td>
</tr>
</tbody>
</table>

Later, when the `foo` package is being installed, RPM will consider
`foo`'s dependency requirements met if any version of package `bar` is
already installed. [<span class="footnote">\[1\]</span>](#FTN.AEN9473)

If more than one package is required, they can be added to the
**Requires** tag, one after another, separated by commas and/or spaces.
So if package `foo` requires packages `bar` *and* `baz`, the following
line will do the trick:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Requires: bar, baz

          </code></pre></td>
</tr>
</tbody>
</table>

As long as any version of `bar` and `baz` is installed, `foo`'s
dependencies will be met.

<div class="sect3">

### <span id="s3-rpm-depend-version-requirements">Adding Version Requirements</span>

When a package has slightly more stringent needs, it's possible to
require certain versions of a package. All that's necessary is to add
the desired version number, preceded by one of the following comparison
operators:

  - Requires package with a version less than the specified version.

  - Requires package with a version less than or equal to the specified
    version.

  - Requires package with a version equal to the specified version.

  - Requires package with a version equal to or greater than the
    specified version.

  - Requires package with a version greater than the specified version.

Continuing with our example, let's suppose that the required version of
package `bar` actually needs to be at least 2.7, and that the `baz`
package must be version 2.1 — no other version will do. Here's what the
**Requires** tag line would look like:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Requires: bar &gt;= 2.7, baz = 2.1

            </code></pre></td>
</tr>
</tbody>
</table>

We can get even more specific and require a particular *release* of a
package:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Requires: bar &gt;= 2.7-4, baz = 2.1-1

            </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect3">

### <span id="s3-rpm-depend-version-not-enough">When Version Numbers Aren't Enough</span>

You might think that with all these features, RPM's dependency
processing can handle every conceivable situation. You'd be right,
except for the problem of version numbers. RPM needs to be able to
determine which version numbers are more recent than others, in order to
perform its version comparisons.

It's pretty simple to determine that version 1.5 is older than version
1.6. But what about 2.01 and 2.1? Or 7.6a and 7.6? There's no way for
RPM to keep up with all the different version-numbering schemes in use.
But there *is* a solution; two, in fact…

<div class="sect4">

#### <span id="s4-rpm-depend-version-epoch">Solution Number 1: Epoch numbers</span>

When RPM can't decipher a package's version number, it's time to pull
out the **Epoch** tag. This tag is used to help RPM determine version
number ordering. Here's a sample **Epoch** tag line:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Epoch: 42

              </code></pre></td>
</tr>
</tbody>
</table>

This line indicates that the package has an epoch number of 42. What
does the 42 mean? Only that this version of the package is newer than
the same package with an epoch number of 41, but older than the same
package with an epoch number of 43. If you think of epoch numbers as
being nothing more than very simple version numbers, you'll be on the
mark. In other words, **Epoch** is the *most significant component* of a
package's complete version identifier with regards to RPM's version
comparison algorithm.

In order to direct RPM to look at the epoch number instead of the
version number when doing dependency checking, it's necessary to use a
"**:**" before the version in the **Requires** tag line. So if a package
requires package `foo` to have an epoch number equal to 42, the
following tag line would be used:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Requires: foo = 42:

              </code></pre></td>
</tr>
</tbody>
</table>

If the `foo` package needs to have an epoch number greater than or equal
to 42, this line would work:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Requires: foo &gt;= 42:

              </code></pre></td>
</tr>
</tbody>
</table>

If the `foo` package needs to have version with an epoch number 42 and
version 1.0, this line would work:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Requires: foo &gt;= 42:1.0

              </code></pre></td>
</tr>
</tbody>
</table>

You *must* include the epoch in a requires if it exists in the package.

It might seem that using epoch numbers is a lot of extra trouble, and
you're right. But there is an alternative:

</div>

<div class="sect4">

#### <span id="s4-rpm-depend-say-no">Solution Number 2: Just Say No\!</span>

If you have the option between changing the software's version-numbering
scheme, or using epoch numbers in RPM, please consider changing the
version-numbering scheme. Chances are, if RPM can't figure it out, most
of the people using your software can't, either. But in case you aren't
the author of the software you're packaging, and its version numbering
scheme is giving RPM fits, the **epoch** tag can help you out.

</div>

</div>

<div class="sect3">

### <span id="s3-rpm-depend-fine-grained">Fine Grained Dependencies</span>

For the vast majority of dependencies, using the normal **Requires** is
enough. However, there are some special situations where one might want
more fine grained control over them. When multiple packages are being
installed in a transaction, installation order and dependency loops are
such cases. Erasure order of packages within a transaction is the
opposite of their installation order.

A very trivial example of a dependency loop is when package `foo`
requires `bar`, and `bar` requires `foo`. However, when the number of
packages involved in a loop grows, the loops get more and more complex.
The special dependency types in this chapter are at best *hints* for
RPM; as a rule of thumb, it is best to try to *avoid dependency loops*
altogether. However, in some rare cases, they may be desired.

<div class="sect4">

#### <span id="s3-rpm-depend-prereq">The **PreReq** Tag</span>

The **PreReq** tag is the same as **Requires**, originally with one
additional property. Using it used to tell RPM that the package marked
as **PreReq** should be installed before the package containing the
dependency. However, as of RPM version 4.4, this special property is
being phased out, and **PreReq** and **Requires** will soon have no
functional differences.

A plain **Requires** is enough to ensure proper installation order *if
there are no dependency loops* present in the transaction. If dependency
loops are present and cannot be avoided, packagers should strive to
construct them in a way that the order of installation of the the this
way interdependent packages does not matter.

Historically, in dependency loops **PreReq** used to "win" over the
conventional **Requires** when RPM determined the installation order in
a transaction. But as said above, this functionality is being phased
out, and one should no longer assume things will work that way.

</div>

<div class="sect4">

#### <span id="s3-rpm-depend-context-depends">Context Marked Dependencies</span>

Recent versions of RPM support context marked dependencies. This is a
special type of a dependency that applies only in a specified *context*.
Using this feature, one can specify dependencies for pre- and
post(un)install scriptlets, ie. the context of a dependency is the
execution time of the specified scriptlet.

The syntax for specifying these dependencies is:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Requires(X): foo

              </code></pre></td>
</tr>
</tbody>
</table>

Here, `X` can be one of **pre**, **post**, **preun**, or **postun**,
which tells RPM that the package depends on package `foo` for running
the corresponding **%pre**, **%post**, **%preun**, or **%postun**
script.

In practice, RPM enforces the above dependencies *until* the specified
script has been run, not *at* that time. In other words, it will allow
erasing a dependency that was marked for eg. the **%post** script for an
already installed package, but will not allow erasing one that is
required for a **%postun** script for such a package. This is to reduce
confusion; it would be somewhat odd if RPM told one to install a package
in order to get another one erased.

</div>

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-depend-conflicts-tag">The **Conflicts** Tag</span>

The **Conflicts** tag is the logical complement to the **Requires** tag.
It is used to specify which packages conflict with the current package.
RPM will not permit conflicting packages to be installed unless
overridden with the **--nodeps** option.

The **Conflicts** tag has the same format as **Requires**. It accepts a
real or virtual package name and can optionally include version and
release specifications or an epoch number.

</div>

<div class="sect2">

## <span id="s2-rpm-depend-provides-tag">The **Provides** Tag</span>

Now that you've seen how it's possible to require a package using the
**Requires** tag, you're probably expecting that you'll need to use the
**Provides** tag in every single package. After all, RPM has to get
those package names from *somewhere*, right?

While it is true that RPM needs to have the package names available, the
**Provides** tag is normally not required. It would actually be
redundant, because the RPM database already contains the name of every
package installed. There's no need to duplicate that information.

But wait — We said earlier that manual dependency requirements are
*always* represented in terms of packages. If RPM doesn't require the
package builder to use the **Provides** tag to provide the package name,
then what is the **Provides** tag used for?

<div class="sect3">

### <span id="s3-rpm-depend-virtual-packages">Virtual Packages</span>

Enter the virtual package. A virtual package is nothing more than a name
specified with the **Provides** tag. Virtual packages are handy when a
package requires a certain capability, and that capability can be
provided by any one of several packages. Here's an example:

In order to work properly, **sendmail** needs a local delivery agent to
handle mail delivery. There are a number of different local delivery
agents available — **sendmail** will work just fine with any of them.

In this case, it doesn't make sense to force the use of a particular
local delivery agent; as long as one's installed, `sendmail`'s
requirements will have been satisfied. So `sendmail`'s package builder
adds the following line to `sendmail`'s spec file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Requires: lda

            </code></pre></td>
</tr>
</tbody>
</table>

There is no package with that name available, so `sendmail`'s
requirements must be met with a virtual package. The creators of the
various local delivery agents indicate that their packages satisfy the
requirements of the `lda` virtual package by adding the following line
to *their* packages' spec files:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Provides: lda

            </code></pre></td>
</tr>
</tbody>
</table>

(Note that virtual packages may not have version numbers.) Now, when
`sendmail` is installed, as long as there is a package installed that
provides the `lda` virtual package, there will be no problem.

</div>

</div>

</div>

### Notes

|                                                                                       |                                                                                                                                                                                                                                                                                                                      |
| ------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-depend-manual-dependencies.html#AEN9473) | As long as the requiring *and* the providing packages are installed using the same invocation of RPM, the dependency checking will succeed. For example, the command **rpm -ivh \*.rpm** will properly check for dependencies, even if the requiring package ends up being installed *before* the providing package. |

<div class="NAVFOOTER">

-----

|                                        |                          |                                    |
| :------------------------------------- | :----------------------: | ---------------------------------: |
| [Prev](s1-rpm-depend-auto-depend.html) |    [Home](index.html)    | [Next](s1-rpm-depend-summary.html) |
| Automatic Dependencies                 | [Up](ch-rpm-depend.html) |                      To Summarize… |

</div>
