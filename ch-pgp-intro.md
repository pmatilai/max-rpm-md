<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-resources-gpl.md)

[Next](s1-pgp-intro-installing-pgp.md)

-----

<div class="appendix">

# <span id="ch-pgp-intro"></span>Appendix G. An Introduction to PGP

Assuming you're not the curious type and haven't flipped your way back
here, you are probably here looking for some information on the program
known as Pretty Good Privacy, or PGP.

<div class="sect1">

# <span id="s1-pgp-intro-pgp-overview">PGP — Privacy for Regular People</span>

PGP, or "Pretty Good Privacy", is a program that is intended to help
make electronic mail more secure. It does this by using sophisticated
techniques known as *public key encryption*.

If you find yourself wondering what electronic mail and making
information unreadable by spies has to do with RPM, you have a good
point. However, although PGP's claim to fame is the handling of e-mail
in total privacy, it has some other tricks up its sleeve.

<div class="sect2">

## <span id="s2-pgp-intro-pgp-keys">Keys your Locksmith Wouldn't Understand</span>

As we mentioned above, PGP uses public key encryption to do some of its
magic. You might guess from the name that this type of encryption
involves keys of some sort. But, as you might imagine, these are not
keys that you can copy down at the local hardware store. They are
numbers — really *large* numbers. Here's what a key might look like
[<span class="footnote">\[1\]</span>](#FTN.AEN18469) :

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>-----BEGIN PGP PUBLIC KEY BLOCK-----
Version: 2.6.2

mQCNAzEpXjUAAAEEAKG4/V9oUSiDc9wIge6Bmg6erDGCLzmFyioAho8kDIJSrcmi
F9qTdPq+fj726pgW1iSb0Y7syZn9Y2lgQm5HkPODfNi8eWyTFSxbr8ygosLRClTP
xqHVhtInGrfZNLoSpv1LdWOme0yOpOQJnghdOMzKXpgf5g84vaUg6PHLopv5AAUR
tCpSZWQgSGF0IFNvZnR3YXJlLCBJbmMuIDxyZWRoYXRAcmVkaGF0LmNvbT6JAFUD
BRAxc0xcKO2uixUx6ZEBAQOfAfsGwmueeH3WcjngsAoZyremvyV3Q8C1YmY1EZC9
SWkQxdRKe7n2PY/WiA82Mvc+op1XGTkmqByvxM9Ax/dXh+peiQCVAwUQMXL7xiIS
axFDcvLNAQH5PAP/TdAOyVcuDkXfOPjN/TIjqKRPRt7k6Fm/ameRvzSqB0fMVHEE
5iZKi55Ep1AkBJ3wX257hvduZ/9juKSJjQNuW/FxcHazPU+7yLZmf27xIq7E0ihW
8zz9JNFWSA9+8vlCMBYwdP1a+DzVdwjbJcnOu3/Z/aCY2lYi9U45PzmtU8iJAJUD
BRAxU9GUGXO+IyM0cSUBAbWfA/9+lVfqcpFYkJIV4HuV5niVv7LW4ywxW/SftqCM
lXDXdJdoDbrvLtVYIGWeGwJ6bES6CoQiQjiW7/WaC3BY9ZITQE4hWOPQADzOnZPQ
fdkIIxuIUAUnU/YarasqvxCs5v/TygfWUTPLPSP+MqGqJcDF2UHXCiNAHrItse9M
h7etkYkAdQMFEDEp61/Nq6IpInoskQEB538C+wSIaCNNDOGxlxS5E2tClXRwMYf0
ymuKXs/srvIUjOO7xuIH4K7qcSSdI4eUwuXy6w5tWWR3xZ/XiygcLtKMi2IZIq0j
wmFq7MEk+Xp8MN7Icawkqj1/1p0p4EwKKkIU64kAlQMFEDEp6pZEcVNogr/H7QEB
jp4D/iblfiCzVTA5QhGeWOj1rRxWzohMvnngn29IJgdnN3zuQXB1/lbVV3zYciRH
NyvpynfcTcgORHNpAIxXDaZ7sd48/v7hHLarcR5kxuY0T75XOTGOKTOlFvb4XmcY
HZR2wSWSBteKezB5uK47A6uhwtvPokV0Owk9xPmBV+LPXkW4
=pnqV
-----END PGP PUBLIC KEY BLOCK-----

        </code></pre></td>
</tr>
</tbody>
</table>

PGP uses two different types of keys: public and private. The public
key, as its name suggests, can be shared with anyone. The key shown
above is, in fact, a public key. The private key, as *its* name
suggests, should be kept a secret. PGP creates keys in pairs — one
private and one public. A key pair must remain a pair; if one is lost,
the other by itself is useless. Why? Because the two keys have an
interesting property that can be exploited in two ways:

  - A message encrypted by a given *public* key can only be decrypted
    with the corresponding *private* key.

  - A message encrypted by a given *private* key can only be decrypted
    with the corresponding *public* key.

In the case of sending messages in total privacy, the key pairs are used
in the first manner. It allows two people to exchange private messages
without first exchanging any "secret codes". The only requirement is
that each know the other's public key.

However, for RPM, the *second* method is the important one. Let's say a
company needs to send you a document, and you'd like to make sure it
really did come from them. If the company first encrypted the file with
their private key and sent it to you, you would have an encrypted file
you couldn't read.

Or could you? If you have the company's *public* key, you should be able
to decrypt it. In fact, if you can't, you can be sure that the message
you received did *not* come from them
[<span class="footnote">\[2\]</span>](#FTN.AEN18491) \!

It is this feature that is used by RPM. By using PGP's public key
encryption, it is possible to not only prove that a package file came
from a certain person or persons, but also that it was not changed
somewhere along the line.

</div>

<div class="sect2">

## <span id="s2-pgp-intro-rpm-packages-encrypted">Are RPM Packages Encrypted?</span>

In a word, no. Rather than being encrypted, RPM package files possess a
*digital signature*. This is a way of using encryption to attach a
signature (again, basically a large number) to some information, such
that:

  - The signature cannot be separated from the information. Any attempt
    to verify the signature against any other information will fail.

  - The signature can only be produced by one private key.

In the case of RPM, the information being signed is the contents of the
`.rpm` file itself.

A digital signature is just like a regular signature. It doesn't obscure
the contents of the document being signed, it just provides a method of
determining the authenticity of a document. Here is an example of a
digital signature turned into printable text:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>-----BEGIN PGP SIGNATURE-----
Version: 2.6.3a
Charset: noconv

iQCVAwUBMXVGMFIa2NdXHZJZAQFe4AQAz0FZrHdH8o+zkIvcI/4ABg4gfE7cG0xE
Z2J9GVWD2zi4tG+s1+IWEY6Ae17kx925JKrzF4Ti2upAwTN2Pnb/x0G8WJQVKQzP
mZcD+XNnAaYCqFz8iIuAFVLchYeWj1Pqxxq0weGCtjQIrpzrmGxV7xXzK0jus+6V
rML3TxQSwdA=
=T9Mc
-----END PGP SIGNATURE-----

        </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect2">

## <span id="s2-pgp-intro-all-rpms-signed">Do All RPM Packages Have Digital Signatures?</span>

Again, no. In a perfect world, every .rpm file would be signed. However,
RPM has no formal requirement that this be the case. There is also no
requirement that you do anything special with a signed .rpm file. Think
of it as an extra feature that you can take advantage of, or not — it's
strictly your choice.

</div>

<div class="sect2">

## <span id="s2-pgp-intro-so-much-so-little">So Much to Cover, So Little Time</span>

PGP has a wealth of features, 99% of which we will not cover in this
book. For more information on the basics of encryption, *Applied
Cryptography*, by Bruce Schneier, contains a wealth of information on
the subject. For more details on PGP specifically, O'Reilly's *PGP:
Pretty Good Privacy* by Simson Garfinkel is an excellent reference.

If you'd rather surf the 'Net, use your favorite World Wide Web index to
hunt for "crypto" or "PGP", and you'll be in business.

</div>

</div>

</div>

### Notes

|                                                                   |                                                                                                                                                                                                           |
| ----------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](ch-pgp-intro.md#AEN18469) | When we say that keys are *numbers*, we aren't lying even though the example key doesn't look like a number. It has been processed so that it can be concisely displayed using only printable characters. |
| [<span class="footnote">\[2\]</span>](ch-pgp-intro.md#AEN18491) | Or at least that it didn't make it to you unchanged.                                                                                                                                                      |

<div class="NAVFOOTER">

-----

|                                   |                    |                                          |
| :-------------------------------- | :----------------: | ---------------------------------------: |
| [Prev](s1-rpm-resources-gpl.md) | [Home](index.md) | [Next](s1-pgp-intro-installing-pgp.md) |
| GNU GENERAL PUBLIC LICENSE        | [Up](p14028.md)  |             Installing PGP for RPM's Use |

</div>
