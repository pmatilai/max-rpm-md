<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-checksig.html)

Chapter 7. Using RPM to Verify Package Files

[Next](s1-rpm-checksig-using-rpm-k.html)

-----

<div class="sect1">

# <span id="s1-rpm-checksig-configuring-pgp">Configuring PGP for **rpm -K**</span>

Once PGP is properly built and installed, the actual configuration for
RPM is trivial. Here's what needs to be done:

  - PGP must be in your path. If PGP's usage message doesn't come up
    when you enter **pgp** at your shell prompt, you'll need to add
    PGP's directory to your path.

  - PGP must be able to find the public keyring file that you want to
    use when checking package file signatures. You can use two methods
    to direct PGP to the public keyring:
    
    1.  Set the `PGPPATH` environment variable to point to the directory
        containing the public keyring file.
    
    2.  Set the **pgp\_path** `rpmrc` file entry to point to the
        directory containing the public keyring file.
        [<span class="footnote">\[1\]</span>](#FTN.AEN4609)

Now we're ready.

</div>

### Notes

|                                                                                     |                                                                                                                                |
| ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| [<span class="footnote">\[1\]</span>](s1-rpm-checksig-configuring-pgp.html#AEN4609) | For more information on `rpmrc` files, `rpmrc` file entries, and how to use them, please see [Appendix B](ch-rpmrc-file.html). |

<div class="NAVFOOTER">

-----

|                                   |                            |                                          |
| :-------------------------------- | :------------------------: | ---------------------------------------: |
| [Prev](ch-rpm-checksig.html)      |     [Home](index.html)     | [Next](s1-rpm-checksig-using-rpm-k.html) |
| Using RPM to Verify Package Files | [Up](ch-rpm-checksig.html) |                         Using **rpm -K** |

</div>
