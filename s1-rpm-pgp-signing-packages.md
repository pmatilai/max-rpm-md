<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-pgp-getting-ready.html)

Chapter 17. Adding PGP Signatures to a Package

[Next](ch-rpm-subpack.html)

-----

<div class="sect1">

# <span id="s1-rpm-pgp-signing-packages">Signing Packages</span>

There are three different ways to sign a package:

1.  Signing a package at build-time.

2.  Replacing the signature on an already-existing package.

3.  Adding a signature to an already-existing package.

Lets take a look at each one, starting with build-time signing.

<div class="sect2">

## <span id="s2-rpm-pgp-sign-option">**--sign** — Sign a Package At Build-Time</span>

The **--sign** option is used to sign a package as it is being built.
When this option is added to an RPM build command, RPM will ask for your
PGP pass phrase. If the pass phrase is correct, the build will proceed.
If not, the build stops immediately.

Here's an example of **--sign** in action:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba --sign blather-7.9.spec
            Enter pass phrase: &lt;passphrase&gt; (Not echoed)

Pass phrase is good.
* Package: blather
…
Binary Packaging: blather-7.9-1
Finding dependencies...
…
Generating signature: 1002
Wrote: /usr/src/redhat/RPMS/i386/blather-7.9-1.i386.rpm
…
Source Packaging: blather-7.9-1
…
Generating signature: 1002
Wrote: /usr/src/redhat/SRPMS/blather-7.9-1.src.rpm

# 
          </code></pre></td>
</tr>
</tbody>
</table>

Once the pass phrase is entered, there's very little that is different
from a normal build. The only obvious difference is the Generating
signature message in both the binary and source packaging sections. The
number following the message indicates that the signature added was
created using PGP. [<span class="footnote">\[1\]</span>](#FTN.AEN10608)

Notice, that since RPM only signs the source and binary package files,
only the **-bb**, and **-ba** options make any sense when used with
**--sign**. This is due to the fact that only the **-bb** and **-ba**
options create package files.

If we issue a quick signature check using RPM's **--checksig** option,
we can see that there is, in fact, a PGP signature present:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm --checksig blather-7.9-1.i386.rpm
blather-7.9-1.i386.rpm: size pgp md5 OK

#
          </code></pre></td>
</tr>
</tbody>
</table>

It's clear to see that, in addition to the usual size and MD5
signatures, the package has a PGP signature.

<div class="sect3">

### <span id="s3-rpm-pgp-multiple-builds">Multiple Builds? No Problem\!</span>

You might be wondering how the **--sign** option would work if more than
one package is to be built. Do you have to enter the pass phrase for
every single package you build? The answer is no, as long as you build
the packages with a single RPM command. Here's an example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba --sign b*.spec
              Enter pass phrase: &lt;passphrase&gt; (Not echoed)

Pass phrase is good.
* Package: blather
…
Binary Packaging: blather-7.9-1
…
Generating signature: 1002
Wrote: /usr/src/redhat/RPMS/i386/blather-7.9-1.i386.rpm
…
Source Packaging: blather-7.9-1
…
Generating signature: 1002
Wrote: /usr/src/redhat/SRPMS/blather-7.9-1.src.rpm
…
* Package: bother
…
Binary Packaging: bother-3.5-1
…
Generating signature: 1002
Wrote: /usr/src/redhat/RPMS/i386/bother-3.5-1.i386.rpm
…
Source Packaging: bother-3.5-1
…
Generating signature: 1002
Wrote: /usr/src/redhat/SRPMS/bother-3.5-1.src.rpm

# 
            </code></pre></td>
</tr>
</tbody>
</table>

Using the **--sign** option makes it as easy to sign one package as it
is to sign one hundred. But what happens if you need to change your
public key? Will you need to rebuild every single one of your packages
just to update the signature?

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-pgp-resign-option">**--resign** — Replace a Package's Signature(s)</span>

As we mentioned at the end of the previous section, from time to time it
may be necessary to change your public key. Certainly this would be
necessary if your key's security was compromised, but other, more
mundane situations might require this.

Fortunately, RPM has an option that permits you to replace the signature
on an already-built package, with a new one. The option is called
**--resign**, and here's an example of its use:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm --resign blather-7.9-1.i386.rpm
            Enter pass phrase: &lt;passphrase&gt; (Not echoed)

Pass phrase is good.
blather-7.9-1.i386.rpm:

#
          </code></pre></td>
</tr>
</tbody>
</table>

While the output is not as exciting as a package build, the **--resign**
option can be a life-saver if you need to change a package's signature,
and you don't want to rebuild.

As you might have guessed, the **--resign** option works properly on
multiple package files:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm --resign b*.rpm
            Enter pass phrase: &lt;passphrase&gt; (Not echoed)

Pass phrase is good.
blather-7.9-1.i386.rpm:
bother-3.5-1.i386.rpm:

#
          </code></pre></td>
</tr>
</tbody>
</table>

<div class="sect3">

### <span id="s3-rpm-pgp-resign-limitations">There Are Limits, However…</span>

Unfortunately, older package files cannot be re-signed. The package file
must be in version 3 format, at least. If you attempt to resign a
package that is too old, here's what you'll see:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm --resign blah.rpm
              Enter pass phrase: &lt;passphrase&gt; (Not echoed)

Pass phrase is good.
blah.rpm:
blah.rpm: Can&#39;t re-sign v2.0 RPM

#
            </code></pre></td>
</tr>
</tbody>
</table>

Not sure what version your package files are at? Just use the **file**
command to check:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># file blather-7.9-1.i386.rpm
blather-7.9-1.i386.rpm: RPM v3 bin i386 blather-7.9-1

#
            </code></pre></td>
</tr>
</tbody>
</table>

The "v3" in **file**'s output indicates the package file format.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-pgp-addsign-option">**--addsign** — Add a Signature To a Package</span>

The **--addsign** option, as the name suggests, is used to add another
signature to the package. It's pretty easy to see why someone would want
to have a package that had been signed by the package builders. But what
reason would there be for *adding* a signature to a package?

One reason to have more than one signature on a package would be to
provide a means of documenting the path of ownership from the package
builder to the end-user.

As an example, the division of a company creates a package and signs it
with the division's key. The company's headquarters then checks the
package's signature and adds the corporate signature to the package, in
essence stating that the signed package received by them is authentic.

Continuing the example, the doubly-signed package makes its way to a
retailer. The retailer checks the package's signatures and, when they
check out, adds their signature to the package.

The package now makes its way to a company that wishes to deploy the
package. After checking every signature on the package, they know that
it is an authentic copy, unchanged since it was first created. Depending
on the deploying company's internal controls, they may choose to add
their own signature, thereby reassuring their employees that the package
has received their corporate "blessing".

After this lengthy example, the actual output from the **--addsign**
option is a bit anti-climactic:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm --addsign blather-7.9-1.i386.rpm
            Enter pass phrase: &lt;passphrase&gt; (Not echoed)

Pass phrase is good.
blather-7.9-1.i386.rpm:

#
          </code></pre></td>
</tr>
</tbody>
</table>

If we check the signatures of this package, we'll be able to see the
multiple signatures:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm --checksig blather-7.9-1.i386.rpm
blather-7.9-1.i386.rpm: size pgp pgp md5 OK

#
          </code></pre></td>
</tr>
</tbody>
</table>

The two pgp's in **--checksig**'s output clearly shows that the package
has been signed twice.

<div class="sect3">

### <span id="s3-rpm-pgp-addsign-caveats">A Few Caveats</span>

As with the **--resign** option, the **--addsign** option cannot do its
magic on pre-V3 package files:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm --addsign blah.rpm
              Enter pass phrase: &lt;passphrase&gt; (Not echoed)

Pass phrase is good.
blah.rpm:
blah.rpm: Can&#39;t re-sign v2.0 RPM

#
            </code></pre></td>
</tr>
</tbody>
</table>

OK, the error message may not be 100% accurate, but you get the idea.

Another thing to be aware of is that the **--addsign** option does not
check for multiple identical signatures. Although it doesn't make much
sense to do so, RPM will happily let you add the same signature as many
times as you'd like:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm --addsig blather-7.9-1.i386.rpm
              Enter pass phrase: &lt;passphrase&gt; (Not echoed)

Pass phrase is good.
blather-7.9-1.i386.rpm:

# rpm --addsig blather-7.9-1.i386.rpm
              Enter pass phrase: &lt;passphrase&gt; (Not echoed)

Pass phrase is good.
blather-7.9-1.i386.rpm:

# rpm --addsig blather-7.9-1.i386.rpm
              Enter pass phrase: &lt;passphrase&gt; (Not echoed)

Pass phrase is good.
blather-7.9-1.i386.rpm:

# rpm --checksig blather-7.9-1.i386.rpm
blather-7.9-1.i386.rpm: size pgp pgp pgp pgp md5 OK

#
            </code></pre></td>
</tr>
</tbody>
</table>

As we can see from **--checksig**'s output, the package now has four
identical signatures. Maybe this is the digital equivalent of pressing
down extra hard while writing your name…

</div>

</div>

</div>

### Notes

|                                                                                  |                                                                                                                               |
| -------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-pgp-signing-packages.html#AEN10608) | The list of possible signature types can be found in the RPM sources, specifically `signature.h` in RPM's `lib` subdirectory. |

<div class="NAVFOOTER">

-----

|                                       |                       |                             |
| :------------------------------------ | :-------------------: | --------------------------: |
| [Prev](s1-rpm-pgp-getting-ready.html) |  [Home](index.html)   | [Next](ch-rpm-subpack.html) |
| Getting Ready to Sign                 | [Up](ch-rpm-pgp.html) |        Creating Subpackages |

</div>
