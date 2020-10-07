<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-checksig-configuring-pgp.html)

Chapter 7. Using RPM to Verify Package Files

[Next](ch-rpm-miscellania.html)

-----

<div class="sect1">

# <span id="s1-rpm-checksig-using-rpm-k">Using **rpm -K**</span>

After all the preliminaries with PGP, it's time to get down to business.
First, we need to get the package builder's public key and add it to the
public keyring file used by RPM. You'll need to do this once for each
package builder whose packages you'll want to check. This is what you'll
need to do:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># pgp -ka RPM-PGP-KEY ./pubring.pgp
Pretty Good Privacy(tm) 2.6.3a - Public-key encryption for the masses.
(c) 1990-96 Philip Zimmermann, Phil&#39;s Pretty Good Software. 1996-03-04
Uses the RSAREF(tm) Toolkit, which is copyright RSA Data Security, Inc.
Distributed by the Massachusetts Institute of Technology.
Export of this software may be restricted by the U.S. government.
Current time: 1996/06/01 22:50 GMT

Looking for new keys...
pub  1024/CBA29BF9 1996/02/20  Red Hat Software, Inc. &lt;redhat@redhat.com&gt;

Checking signatures...

Keyfile contains:
   1 new key(s)

One or more of the new keys are not fully certified.

Do you want to certify any of these keys yourself (y/N)? n
        </code></pre></td>
</tr>
</tbody>
</table>

Here we've added Red Hat's public key, since we're going to check some
package files produced by them. The file `RPM-PGP-KEY` contains the key.
At the end, PGP asks us if we want to certify the new key. We've
answered "no" since it isn't necessary to certify keys to verify package
files.

Next, we'll verify a package file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -K rpm-2.3-1.i386.rpm
rpm-2.3-1.i386.rpm: size pgp md5 OK

#
        </code></pre></td>
</tr>
</tbody>
</table>

While the output might seem somewhat anti-climactic, we can now be
nearly 100% certain this package:

1.  was produced by Red Hat.

2.  is unchanged from their original copy.

The output from this command shows that there are actually three
distinct features of the package file that are checked by the **-K**
option:

1.  The size message indicates that the size of the packaged files has
    not changed.

2.  The pgp message indicates that the digital signature contained in
    the package file is a valid signature of the package file contents,
    and was produced by the organization that originally signed the
    package.

3.  The md5 message indicates that a checksum contained in the package
    file and calculated when the package was built, matches a checksum
    calculated by RPM during verification. Because the two checksums
    match, it is unlikely that the package has been modified.

The OK means that each of these tests were successful. If any had
failed, the name would have been printed in parentheses. A bit later in
the chapter, we'll see what happens when there are verification
problems.

<div class="sect2">

## <span id="s2-rpm-checksig-v-option">**-v** — Display Additional Information</span>

Adding **v** to a verification command will produce more interesting
output:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Kv rpm-2.3-1.i386.rpm
rpm-2.3-1.i386.rpm:
Header+Archive size OK: 278686 bytes
Good signature from user &quot;Red Hat Software, Inc. &lt;redhat@redhat.com&gt;&quot;.
Signature made 1996/12/24 18:37 GMT using 1024-bit key, key ID CBA29BF9

WARNING:  Because this public key is not certified with a trusted
signature, it is not known with high confidence that this public key
actually belongs to: &quot;Red Hat Software, Inc. &lt;redhat@redhat.com&gt;&quot;.
MD5 sum OK: 8873682c5e036a307dee87d990e75349

#
          </code></pre></td>
</tr>
</tbody>
</table>

With a bit of digging, we can see that each of the three tests was
performed, and each passed. The reason for that dire-sounding warning is
that PGP is meant to operate without a central authority managing key
distribution. PGP certifies keys based on *webs of trust*. For example,
if an acquaintance of yours creates a public key, you can certify it by
attaching your digital signature to it. Then anyone that knows and
trusts you can also trust your acquaintance's public key.

In this case, the key came directly from a mass-produced Red Hat Linux
CDROM. If someone was trying to masquerade as Red Hat then they have
certainly gone through a lot of trouble to do so. In this case, the lack
of a certified public key is not a major problem, given the fact that
the CDROM came directly from the Red Hat offices.
[<span class="footnote">\[1\]</span>](#FTN.AEN4677)

</div>

<div class="sect2">

## <span id="s2-rpm-checksig-when-not-signed">When the Package is Not Signed</span>

As mentioned earlier, not every package you'll run across is going to be
signed. If this is the case, here's what you'll see from RPM:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -K bother-3.5-1.i386.rpm
bother-3.5-1.i386.rpm: size md5 OK

#
          </code></pre></td>
</tr>
</tbody>
</table>

Note the lack of a pgp message. The size and md5 messages indicate that
the package still has size and checksum information that verified
properly. In fact, all recently-produced package files will have these
verification measures built in automatically.

If you happen to run across an older unsigned package, you'll know it
right away:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -K apmd-2.4-1.i386.rpm
apmd-2.4-1.i386.rpm: No signature available

#
          </code></pre></td>
</tr>
</tbody>
</table>

Older package files had only a PGP-based signature; if that was missing,
there was nothing left to verify.

</div>

<div class="sect2">

## <span id="s2-rpm-checksig-missing-public-key">When You Are Missing the Correct Public Key</span>

If you happen to forget to add the right public key to RPM's keyring,
you'll see the following response:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -K rpm-2.3-1.i386.rpm
rpm-2.3-1.i386.rpm: size (PGP) md5 OK (MISSING KEYS)

#
          </code></pre></td>
</tr>
</tbody>
</table>

Here the PGP in parentheses indicates that there's a problem with the
signature, and the message at the end of the line (MISSING KEYS) shows
what the problem is. Basically, RPM asked PGP to verify the package
against a key that PGP didn't have, and PGP complained.

</div>

<div class="sect2">

## <span id="s2-rpm-checksig-verification-failures">When a Package Just Doesn't Verify</span>

Eventually it's going to happen — you go to verify a package, and it
fails. We'll look at an example of a package that fails verification a
bit later. Before we do that, let's *make* a package that won't verify,
to demonstrate how sensitive RPM's verification is.

First, we made a copy of a signed package, `rpm-2.3-1.i386.rpm`, to be
specific. We called the copy `rpm-2.3-1.i386-bogus.rpm`. Next, using
Emacs (in hexl-mode, for all you Emacs buffs), we changed the first
letter of the name of the system that built the original package. The
file `rpm-2.3-1.i386-bogus.rpm` is now truly bogus: it has been changed
from the original file.

Although the change was a small one, it still showed up when the package
file was queried. Here's a listing from the original package:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qip rpm-2.3-1.i386.rpm
Name        : rpm                   Distribution: Red Hat Linux Vanderbilt
Version     : 2.3                         Vendor: Red Hat Software
Release     : 1                       Build Date: Tue Dec 24 09:07:59 1996
Install date: (none)                  Build Host: porky.redhat.com
Group       : Utilities/System        Source RPM: rpm-2.3-1.src.rpm
Size        : 631157
Summary     : Red Hat Package Manager
Description :
RPM is a powerful package manager, which can be used to build, install,
query, verify, update, and uninstall individual software packages. A
package consists of an archive of files, and package information,
including name, version, and description.

# 
          </code></pre></td>
</tr>
</tbody>
</table>

And here's the same listing from the bogus package file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -qip rpm-2.3-1.i386-bogus.rpm
Name        : rpm                   Distribution: Red Hat Linux Vanderbilt
Version     : 2.3                         Vendor: Red Hat Software
Release     : 1                       Build Date: Tue Dec 24 09:07:59 1996
Install date: (none)                  Build Host: qorky.redhat.com
Group       : Utilities/System        Source RPM: rpm-2.3-1.src.rpm
Size        : 631157
Summary     : Red Hat Package Manager
Description :
RPM is a powerful package manager, which can be used to build, install,
query, verify, update, and uninstall individual software packages. A
package consists of an archive of files, and package information,
including name, version, and description.

# 
          </code></pre></td>
</tr>
</tbody>
</table>

Notice that the build host name changed from `porky.redhat.com` to
`qorky.redhat.com`. Using the **cmp** utility to compare the two files,
we find that the difference occurs at byte 1201, which changed from "p"
(octal 160), to "q" (octal 161):

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># cmp -cl rpm-2.3-1.i386.rpm rpm-2.3-1.i386-bogus.rpm
  1201 160 p    161 q

#
          </code></pre></td>
</tr>
</tbody>
</table>

People versed in octal numbers will note that only *one bit* has been
changed in the entire file. That's the smallest possible change you can
make\! Let's see how our bogus friend fares:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -K rpm-2.3-1.i386-bogus.rpm
rpm-2.3-1.i386-bogus.rpm: size PGP MD5 NOT OK

#
          </code></pre></td>
</tr>
</tbody>
</table>

Given that the command's output ends with NOT OK in big capital letters,
it's obvious there's a problem. Since the word size was printed in
lowercase, the bogus package's size was OK, which makes sense — we only
changed the value of one bit without adding or subtracting anything
else.

However, the PGP signature, printed in uppercase, didn't verify. Again,
this makes sense, too. The package that was signed by Red Hat has been
changed. The fact that the package's MD5 checksum also failed to verify
provides further evidence that the bogus package is just that: bogus.

</div>

<div class="sect2">

## <span id="s2-rpm-checksig-nopgp-option">**--nopgp** — Do Not Verify Any PGP Signatures</span>

Perhaps you want to be able to verify packages but, for one reason or
another, you cannot use PGP. Maybe you don't have a trustworthy source
of the necessary public keys, or maybe it's illegal to possess
encryption (like PGP) software in your country. Is it still possible to
verify packages?

Certainly — in fact, we've already done it, in [the Section called *When
You Are Missing the Correct Public
Key*](s1-rpm-checksig-using-rpm-k.html#s2-rpm-checksig-missing-public-key).
You lose the ability to verify the package's origins, as well as some
level of confidence in the package's integrity, but the size and MD5
checksums still give some measure of assurance as to the package's
state.

Of course, when PGP can't be used, the output from a verification always
looks like something's wrong:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -K rpm-2.3-1.i386.rpm
rpm-2.3-1.i386.rpm: size (PGP) md5 OK (MISSING KEYS)

#
          </code></pre></td>
</tr>
</tbody>
</table>

The **--nopgp** option directs RPM to ignore PGP entirely. If we use the
**--nopgp** option on our example above, we find that things look a
whole lot better:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -K --nopgp rpm-2.3-1.i386.rpm
rpm-2.3-1.i386.rpm: size md5 OK

#
          </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect2">

## <span id="s2-rpm-checksig-vv-option">**-vv** — Display Debugging Information</span>

Nine times out of ten, you'll probably never have to use it, but if
you're the curious type, the **-vv** option will give you insights into
how RPM verifies packages. Here's an example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Kvv rpm-2.3-1.i386.rpm
D: New Header signature
D: magic: 8e ad e8 01
D: got  : 8e ad e8 01
D: Signature size: 236
D: Signature pad : 4
D: sigsize         : 240
D: Header + Archive: 278686
D: expected size   : 278686
rpm-2.3-1.i386.rpm:
Header+Archive size OK: 278686 bytes
Good signature from user &quot;Red Hat Software, Inc. &lt;redhat@redhat.com&gt;&quot;.
Signature made 1996/12/24 18:37 GMT using 1024-bit key, key ID CBA29BF9

WARNING:  Because this public key is not certified with a trusted
signature, it is not known with high confidence that this public key
actually belongs to: &quot;Red Hat Software, Inc. &lt;redhat@redhat.com&gt;&quot;.
MD5 sum OK: 8873682c5e036a307dee87d990e75349

# 
          </code></pre></td>
</tr>
</tbody>
</table>

The lines starting with D: represent extra output produced by the
**-vv** option. This output is normally used by software developers in
the course of adding new features to RPM and is subject to change, but
there's no law against looking at it.

Briefly, the output shows that RPM has detected a new-style signature
block, containing size, MD5 checksum, and PGP signature information. The
size of the signature, the size of the package file's header and archive
sections, and the expected size of those sections are all displayed.

</div>

<div class="sect2">

## <span id="s2-rpm-checksig-rcfile-option">**--rcfile `<rcfile>`**: Use **`<rcfile>`** As An Alternate `rpmrc` File</span>

The **--rcfile** option is used to specify a file containing default
settings for RPM. Normally, this option is not needed. By default, RPM
uses `/etc/rpmrc` and a file named `.rpmrc` located in your login
directory.

This option would be used if there was a need to switch between several
sets of RPM defaults. Software developers and package builders will
normally be the only people using the **--rcfile** option. For more
information on `rpmrc` files, see [Appendix B](ch-rpmrc-file.html).

</div>

</div>

### Notes

|                                                                                 |                                                                                                                                                                                                                                                 |
| ------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-checksig-using-rpm-k.html#AEN4677) | Red Hat Software's public key is also available from their website, at <http://www.redhat.com/redhat/contact.html> . The RPM sources also contain the key, and are available from their FTP site at <ftp://ftp.redhat.com/pub/redhat/code/rpm>. |

<div class="NAVFOOTER">

-----

|                                              |                            |                                 |
| :------------------------------------------- | :------------------------: | ------------------------------: |
| [Prev](s1-rpm-checksig-configuring-pgp.html) |     [Home](index.html)     | [Next](ch-rpm-miscellania.html) |
| Configuring PGP for **rpm -K**               | [Up](ch-rpm-checksig.html) |                     Miscellanea |

</div>
