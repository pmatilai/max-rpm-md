<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-commands-add-signature-mode.html)

Appendix C. Concise RPM Command Reference

[Next](s1-rpm-commands-initialize-database-mode.html)

-----

<div class="sect1">

# <span id="s1-rpm-commands-check-signature-mode">Check Signature Mode</span>

RPM's check signature mode is used to verify a package's signature:

Format: **rpm --checksig `<options>` `<packagefile>`+**

or

Format: **rpm -K `<options>` `<packagefile>`+**

<div class="sect2">

## <span id="s2-rpm-commands-check-signature-options">Options To Check Signature Mode</span>

The following option can be used on any check signature command:

  - **--nopgp** — Skip any PGP signatures (size and MD5 only).

</div>

</div>

<div class="NAVFOOTER">

-----

|                                                 |                            |                                                       |
| :---------------------------------------------- | :------------------------: | ----------------------------------------------------: |
| [Prev](s1-rpm-commands-add-signature-mode.html) |     [Home](index.html)     | [Next](s1-rpm-commands-initialize-database-mode.html) |
| Add Signature Mode                              | [Up](ch-rpm-commands.html) |                              Initialize Database Mode |

</div>
