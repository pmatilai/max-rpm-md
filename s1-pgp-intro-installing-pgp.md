<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-pgp-intro.md)

Appendix G. An Introduction to PGP

[Next](i18625.md)

-----

<div class="sect1">

# <span id="s1-pgp-intro-installing-pgp">Installing PGP for RPM's Use</span>

To use RPM's PGP-related capabilities, you'll need to have PGP installed
on your system. If it's installed already, you should be able to flip to
the chapters on verifying package signatures and signing packages and be
in business in a matter of minutes. Otherwise, read on for a thumbnail
sketch of what's required to install PGP.

<div class="sect2">

## <span id="s2-pgp-intro-obtaining-pgp">Obtaining PGP</span>

The first step in being able to verify .rpm files is to get a copy of
PGP. Unfortunately, this is not quite as simple as it might sound. The
reason is that PGP is very controversial stuff.

Why the controversy? It centers on PGP's primary mission — to provide a
means of communicating with others in complete privacy. As we've
discussed, PGP uses encryption to provide this privacy. Good encryption.
*Very* good encryption. Encryption so good, it appears some of the
world's governments consider PGP a threat to their national security.

<div class="sect3">

### <span id="s3-pgp-intro-know-laws">Know Your Laws\!</span>

Various countries have differing stances on the use of "strong
encryption" products such as PGP. In some countries, possession of
encryption software is strictly forbidden. Other countries attempt to
control the flow of encryption technology into (or out of) their
country. It is *vital* you know your country's laws, lest you find
yourself in prison, or possibly in front of a firing squad\!

</div>

<div class="sect3">

### <span id="s3-pgp-intro-patent-issues">Patent/Licensing Issues Surrounding PGP</span>

Over and above PGP's legal status, there are other aspects to PGP that
people living in the U.S. and Canada should keep in mind:

  - PGP is free — for non-commercial use only. If you are going to use
    PGP for business purposes, you should look into getting a commercial
    copy. PGP is marketed in the United States by:
    
    Pretty Good Privacy, Inc.
    
      
                      <span class="street"> 2121 S. El Camino Real
    </span>  
                      <span class="otheraddr"> Suite 902 </span>  
                      <span class="city">San
    Mateo</span>, <span class="state">CA</span> <span class="postcode">94403</span>  
                      <span class="phone">(415) 572-0430</span>  
                      <span class="fax">(415) 572-1932</span>  
                      <span class="otheraddr"> http://www.pgp.com/
    </span>  
                    

  - Part of the software that comprises PGP is protected by several
    United States patents. Versions of PGP approved for use in the U.S.
    contain a licensed version of this software, known as RSAREF. RSAREF
    includes a patent license that allows the use of the software in
    noncommercial settings only. Commercial use of the technology
    contained in RSAREF requires a separate license. This is one reason
    why there are restrictions on the commercial use of PGP in the
    United States and Canada.
    
    While people outside the U.S. and Canada can use RSAREF-based PGP,
    they will probably choose the so-called "international" version.
    This version replaces RSAREF with software known as MPILIB. MPILIB
    is, in general, faster than RSAREF, but it cannot legally be used in
    the United States or Canada.

To summarize, if you are using PGP for commercial purposes in the U.S.
or Canada, you'll need to purchase it. Otherwise, people living in the
U.S. or Canada should use a version of PGP incorporating RSAREF. People
in other countries can use any version of PGP they desire, though
they'll probably choose the MPILIB-based "international" version
[<span class="footnote">\[1\]</span>](#FTN.AEN18564) .

</div>

<div class="sect3">

### <span id="s3-pgp-intro-rsaref-based-pgp">Getting RSAREF-based PGP</span>

The official source for the latest version of PGP based on RSAREF is the
Massachusetts Institute of Technology. Due to the restrictions on the
export of encryption technology, the process is somewhat convoluted. The
easiest way to obtain PGP from the official MIT archive is to use the
World Wide Web. Point your web browser at:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>http://web.mit.edu/network/pgp.md

          </code></pre></td>
</tr>
</tbody>
</table>

Simply follow the steps, and you'll have the necessary software on your
system in no time.

There is a more cumbersome method that doesn't use the Web. It involves
first using anonymous ftp to obtain several files of instructions and
license agreements. You will then be directed to use telnet to obtain
the name of a temporary ftp directory containing the PGP software.
Finally, you can use anonymous ftp to retrieve the software. To start
this process, ftp to:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>ftp://net-dist.mit.edu

          </code></pre></td>
</tr>
</tbody>
</table>

Then change directory to:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>/pub/PGP

          </code></pre></td>
</tr>
</tbody>
</table>

Obtain a copy of the file `README` and follow the instructions in it
*exactly*.

If all this seems like too much trouble, there is another alternative.
You can find copies of PGP on just about any BBS, FTP, or Web site
advertising freely available software. Be aware, however, that "Floyd's
Storm Door and BBS Company" may not be as trustworthy a place as MIT to
obtain encryption software. It's really a question of how paranoid you
are.

</div>

<div class="sect3">

### <span id="s3-pgp-intro-pgp-outside-us">Outside the United States and Canada</span>

For people living in other countries, it is much easier to find PGP
(depending on the legality of encryption software, of course). Try any
of the places you'd normally look for free software. Keep in mind,
however, that you shouldn't download PGP from any sites in the U.S.
Doing so is considered an "export" of munitions, and can get the people
responsible for the site in deep trouble. Wherever you eventually get
PGP from, since the patents that complicate matters for the U.S. do not
apply abroad, you'll probably end up with the "international" versions
of PGP.

</div>

</div>

<div class="sect2">

## <span id="s2-pgp-intro-building-pgp">Building PGP</span>

Building PGP is mostly a matter of following instructions. However,
users of ELF-based Linux distributions (Such as Red Hat Linux) will find
that PGP will not build. The problem, according to the PGP FAQ, is that
two files do not properly handle the C preprocessor directives that
affect support for ELF. The changes are to two files: `80386.S` and
`zmatch.S`. Near the beginning of each, you'll find either a
**\#ifndef** or a **\#ifdef** for `SYSV`. If you find:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#ifndef SYSV

        </code></pre></td>
</tr>
</tbody>
</table>

It should be changed to read:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#if !defined(SYSV) &amp;&amp; !defined(__ELF__)

        </code></pre></td>
</tr>
</tbody>
</table>

If you find:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#ifdef SYSV

        </code></pre></td>
</tr>
</tbody>
</table>

It should be changed to read:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#if defined(SYSV) || defined(__ELF__)

        </code></pre></td>
</tr>
</tbody>
</table>

After making these changes, PGP should build with no problems.

</div>

<div class="sect2">

## <span id="s2-pgp-intro-ready-to-go">Ready to Go\!</span>

After building and installing PGP, you're ready to start using RPM's
package signature capabilities. If your primary interest is in checking
the signatures on packages built by someone else, [Chapter
7](ch-rpm-checksig.md) will tell you everything you need to know.

On the other hand, if you are a package builder and would like to start
signing packages, [Chapter 17](ch-rpm-pgp.md) will have you signing
packages in no time.

</div>

</div>

### Notes

|                                                                                  |                                                                                                           |
| -------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-pgp-intro-installing-pgp.md#AEN18564) | Note that there are no commercial restrictions regarding PGP in countries other than the U.S. and Canada. |

<div class="NAVFOOTER">

-----

|                           |                         |                     |
| :------------------------ | :---------------------: | ------------------: |
| [Prev](ch-pgp-intro.md) |   [Home](index.md)    | [Next](i18625.md) |
| An Introduction to PGP    | [Up](ch-pgp-intro.md) |               Index |

</div>
