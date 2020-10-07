<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-verify-we-lied.html)

[Next](s1-rpm-checksig-configuring-pgp.html)

-----

<div class="chapter">

# <span id="ch-rpm-checksig"></span>Chapter 7. Using RPM to Verify Package Files

<div class="table">

<span id="tb-rpm-checksig-command-syntax"></span>

**Table 7-1. **rpm -K** Command Syntax**

**rpm -K** (or **--checksig**) `options` `file1.rpm` … `fileN`.rpm

</div>

</div>

Parameters

`file1.rpm` … `fileN`.rpm

One or more RPM package files (URLs OK)

Checksig-specific Options

Page

**--nopgp**

Do not verify PGP signatures

[the Section called ***--nopgp** — Do Not Verify Any PGP
Signatures*](s1-rpm-checksig-using-rpm-k.html#s2-rpm-checksig-nopgp-option)

General Options

Page

**-v**

Display additional information

[the Section called ***-v** — Display Additional
Information*](s1-rpm-checksig-using-rpm-k.html#s2-rpm-checksig-v-option)

**-vv**

Display debugging information

[the Section called ***-vv** — Display Debugging
Information*](s1-rpm-checksig-using-rpm-k.html#s2-rpm-checksig-vv-option)

**--rcfile `<rcfile>`**

Set alternate rpmrc file to **`<rcfile>`**

[the Section called ***--rcfile `<rcfile>`**: Use **`<rcfile>`** As An
Alternate `rpmrc`
File*](s1-rpm-checksig-using-rpm-k.html#s2-rpm-checksig-rcfile-option)

<div class="sect1">

# <span id="s1-rpm-checksig-what-it-does">**rpm -K** — What Does it Do?</span>

One aspect of RPM is that you can get a package from the Internet, and
easily install it. But what do you know about that package file? Is the
organization listed as being the "vendor" of the package *really* the
organization that built it? Did someone make unauthorized changes to it?
Can you trust that, if installed, it won't mail a copy of your password
file to a system cracker?

Features built into RPM allow you to make sure that the package file
you've just gotten won't cause you problems once it's installed, whether
the package was corrupted by line noise when you downloaded it, or
something more sinister happened to it.

The command **rpm -K** (The option **--checksig** is equivalent)
verifies a package file. Using this command, it is easy to make sure the
file has not been changed in any way. **rpm -K** can also be used to
make sure that the package was actually built by the organization listed
as being the package's vendor. That's all very impressive, but how does
it do that? Well, it just needs help from some "Pretty Good" software.

<div class="sect2">

## <span id="s2-rpm-checksig-pgp-rpms-assistant">Pretty Good Privacy: RPM's Assistant</span>

The "Pretty Good" software we're referring to is known as "Pretty Good
Privacy", or PGP. While all the information on PGP could fill a book (or
several), we've provided a quick introduction to help you get started.

If PGP is new to you, a quick glance through [Appendix
G](ch-pgp-intro.html) should get you well on your way to understanding,
building, and installing PGP. If, on the other hand, you've got PGP
already installed and have sent an encrypted message or two, you're
probably more than ready to continue with this chapter.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                    |                    |                                              |
| :--------------------------------- | :----------------: | -------------------------------------------: |
| [Prev](s1-rpm-verify-we-lied.html) | [Home](index.html) | [Next](s1-rpm-checksig-configuring-pgp.html) |
| We've Lied to You…                 |  [Up](p108.html)   |               Configuring PGP for **rpm -K** |

</div>
