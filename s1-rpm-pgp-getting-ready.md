<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-pgp.md)

Chapter 17. Adding PGP Signatures to a Package

[Next](s1-rpm-pgp-signing-packages.md)

-----

<div class="sect1">

# <span id="s1-rpm-pgp-getting-ready">Getting Ready to Sign</span>

OK, we've convinced you that signing packages is a good idea. Now we've
got to make sure PGP and RPM are up to the task. As you might imagine,
there are two parts to this process: one for PGP, and one for RPM. Let's
get PGP ready first.

<div class="sect2">

## <span id="s2-rpm-pgp-creating-key-pair">Preparing PGP: Creating a Key Pair</span>

There is really very little to be done to PGP, assuming it's been
installed properly. The only thing required is to generate a key pair.
As mentioned in our mini-primer on PGP, the key pair consists of a
secret key and a public key. In terms of signing packages, you will use
your secret key to do the actual signing. Anyone interested in checking
your signature will need your public key.

Creating a key pair is quite simple. All that's required is to issue a
**pgp -kg** command, enter some information, and create some random
bits. Here's an example key generating session:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># pgp -kg
Pretty Good Privacy(tm) 2.6.3a - Public-key encryption for the masses.
(c) 1990-96 Philip Zimmermann, Phil&#39;s Pretty Good Software. 1996-03-04
Uses the RSAREF(tm) Toolkit, which is copyright RSA Data Security, Inc.
Distributed by the Massachusetts Institute of Technology.
Export of this software may be restricted by the U.S. government.
Current time: 1996/10/31 00:42 GMT

Pick your RSA key size:
    1)   512 bits- Low commercial grade, fast but less secure
    2)   768 bits- High commercial grade, medium speed, good security
    3)  1024 bits- &quot;Military&quot; grade, slow, highest security


Choose 1, 2, or 3, or enter desired number of bits: 3
Generating an RSA key with a 1024-bit modulus.

You need a user ID for your public key.  The desired form for this
user ID is your name, followed by your E-mail address enclosed in
&lt;angle brackets&gt;, if you have an E-mail address.
For example:  John Q. Smith &lt;12345.6789@compuserve.com&gt;

Enter a user ID for your public key: 

Example Key for RPM Book
You need a pass phrase to protect your RSA secret key.
Your pass phrase can be any sentence or phrase and may have many
words, spaces, punctuation, or any other printable characters.


            Enter pass phrase: &lt;passphrase&gt; (Not echoed)
            Enter same pass phrase again: &lt;passphrase&gt; (Still not echoed)

Note that key generation is a lengthy process.

We need to generate 952 random bits.  This is done by measuring the
time intervals between your keystrokes.  Please enter some random text
on your keyboard until you hear the beep:


(Many random characters were entered)
   0 * -Enough, thank you.
............................................
................................**** ...**** 
Pass phrase is good.  Just a moment....
Key signature certificate added.
Key generation completed.

# 
          </code></pre></td>
</tr>
</tbody>
</table>

Let's review each of the times PGP required information. The first thing
PGP needed to know was the key size we wanted. Depending on your level
of paranoia, simply choose an appropriate key size. In our example, we
chose the "They're out to get me" key size of 1024 bits.

Next, we needed to choose a user ID for the key. The user ID should be
descriptive and should also include sufficient information for someone
to contact you. We entered Example Key for RPM Book, which goes against
our suggestion, but is sufficient for the purposes of our example.

After entering a user ID, we needed to add a pass phrase. The pass
phrase is used to protect your secret key, so it should be something
difficult for someone else to guess. It should also be memorable for
you, because if you forget your pass phrase, you won't be able to use
your secret key\! I entered a couple of words and numbers, put together
in such a way that no one could ever guess I typed rpm2kool4words

Oops…

The pass phrase is entered twice, to ensure that no typing mistakes were
made. PGP also performs some cursory checks on the pass phrase, ensuring
that the phrase is at least somewhat secure.

Finally comes the strangest part of the key-generation process, creating
random bits. This is done by measuring the time between keystrokes. The
secret here is to *not* hold down a key so that it auto-repeats and to
*not* wait several seconds between keystrokes. Simply start typing
anything (even nonsense text) until PGP tells you you've typed enough.

After generating enough random bits, PGP takes a minute or so to create
the key pair. Assuming everything completed successfully, you'll see an
ending message similar to the one above. You'll also find, in a
subdirectory of your login directory called `.pgp`, the following files:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -al ~/.pgp
total 6
drwxr-xr-x   2 root     root         1024 Oct 30 19:44 .
drwxr-xr-x   5 root     root         1024 Oct 30 19:44 ..
-rw-------   1 root     root          176 Oct 30 19:44 pubring.bak
-rw-------   1 root     root          331 Oct 30 19:44 pubring.pgp
-rw-------   1 root     root          408 Oct 30 19:44 randseed.bin
-rw-------   1 root     root          509 Oct 30 19:44 secring.pgp

# 
          </code></pre></td>
</tr>
</tbody>
</table>

For those interested in learning exactly what each file is, feel free to
consult any of the fine books on PGP. For the purposes of signing
packages, all we need to know is where these files are located.

That's it\! Now it's time to configure RPM to use your newly generated
key.

</div>

<div class="sect2">

## <span id="s2-rpm-pgp-preparing-rpm">Preparing RPM</span>

RPM's configuration process is quite straightforward. It consists of
adding a few `rpmrc` entries in a file of your choice. For more
information on rpmrc files in general, please see [Appendix
B](ch-rpmrc-file.md).

The entries that need to be added to an rpmrc file are:

  - **signature**

  - **pgp\_name**

  - **pgp\_path**

Let's check out the entries.

<div class="sect3">

### <span id="s3-rpm-pgp-signature-entry">**signature**</span>

The **signature** entry is used to select the type of signature that RPM
is to use. At the time this book was written, the only legal value is
**pgp**. So you would enter:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>signature: pgp

            </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect3">

### <span id="s3-rpm-pgp-pgp-name-entry">**pgp\_name**</span>

The **pgp\_name** entry gives RPM the user ID of the key it is to sign
packages with. In our key generation example, the user ID of the key we
created was Example Key for RPM Book, so this is what our entry should
look like:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>pgp_name: Example Key for RPM Book

            </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect3">

### <span id="s3-rpm-pgp-pgp-path-entry">**pgp\_path**</span>

The **pgp\_path** entry is used to define the path to the directory
where the keys are kept. This entry is not needed if the environment
variable `PGPPATH` has been defined. In our example, we didn't move them
from PGP's default location, which is in the subdirectory `.pgp`, off
the user's login directory. Since we generated the key as `root`, our
path is `/root/.pgp`. Therefore, our entry would look like this:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>pgp_path: /root/.pgp

            </code></pre></td>
</tr>
</tbody>
</table>

And that's it. Now it's time to sign some packages.

</div>

</div>

</div>

<div class="NAVFOOTER">

-----

|                                    |                       |                                          |
| :--------------------------------- | :-------------------: | ---------------------------------------: |
| [Prev](ch-rpm-pgp.md)            |  [Home](index.md)   | [Next](s1-rpm-pgp-signing-packages.md) |
| Adding PGP Signatures to a Package | [Up](ch-rpm-pgp.md) |                         Signing Packages |

</div>
