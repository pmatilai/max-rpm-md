<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-verify-what-to-verify.html)

Chapter 6. Using RPM to Verify Installed Packages

[Next](ch-rpm-checksig.html)

-----

<div class="sect1">

# <span id="s1-rpm-verify-we-lied">We've Lied to You…</span>

Not really; we just omitted a few details until you've had a chance to
see **rpm -V** in action. Here are the details:

<div class="sect2">

## <span id="s2-rpm-verify-verification-control">RPM Controls What Gets Verified</span>

Depending on the type of file being verified, RPM will not verify every
possible attribute. Here is a table showing the attributes checked for
each of the different file types:

<div class="table">

<span id="tb-rpm-verify-verification-control"></span>

**Table 6-2. Verification Versus File Types**

|                |           |      |              |              |              |                |       |       |                   |
| -------------- | --------- | ---- | ------------ | ------------ | ------------ | -------------- | ----- | ----- | ----------------- |
| File Type      | File Size | Mode | MD5 Checksum | Major Number | Minor Number | Symlink String | Owner | Group | Modification Time |
| Directory File | \-        | X    | \-           | \-           | \-           | \-             | X     | X     | \-                |
| Symbolic Links | \-        | X    | \-           | \-           | \-           | X              | X     | X     | \-                |
| FIFO           | \-        | X    | \-           | \-           | \-           | \-             | X     | X     | \-                |
| Devices        | \-        | X    | \-           | X            | X            | \-             | X     | X     | \-                |
| Regular Files  | X         | X    | X            | \-           | \-           | \-             | X     | X     | X                 |

</div>

<div class="sect3">

### <span id="s3-rpm-verify-package-builder-control">The Package Builder Can Also Control What Gets Verified</span>

When a package builder creates a new package, they can control what
attributes are to be verified on a file-by-file basis. The reasons for
excluding specific attributes from verification can be quite involved,
but here's an example just to give you the flavor:

When a person logs into a system, there are device files associated with
that user's terminal session. In order for the terminal device (called
`tty`) to function properly, the owner and group of the device must
change to that of the person logging in. Therefore, if RPM were to
verify the package that created the `tty` device files, any ttys that
were in use at the time would fail to verify. However, by using the
**%verify** [<span class="footnote">\[1\]</span>](#FTN.AEN4495)
directive, a package builder can save you from trivial verification
failures.

</div>

</div>

</div>

### Notes

|                                                                           |                                                                                                                                                                              |
| ------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-verify-we-lied.html#AEN4495) | See [the Section called *The **%verify** Directive* in Chapter 13](s1-rpm-inside-files-list-directives.html#s3-rpm-inside-flist-verify-directive) for details on **%verify** |

<div class="NAVFOOTER">

-----

|                                           |                          |                                   |
| :---------------------------------------- | :----------------------: | --------------------------------: |
| [Prev](s1-rpm-verify-what-to-verify.html) |    [Home](index.html)    |      [Next](ch-rpm-checksig.html) |
| Selecting What to Verify, and How         | [Up](ch-rpm-verify.html) | Using RPM to Verify Package Files |

</div>
