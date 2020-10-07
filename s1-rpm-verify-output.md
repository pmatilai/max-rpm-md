<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-verify.html)

Chapter 6. Using RPM to Verify Installed Packages

[Next](s1-rpm-verify-what-to-verify.html)

-----

<div class="sect1">

# <span id="s1-rpm-verify-output">When Verification Fails — **rpm -V** Output</span>

When verifying a package, RPM produces output *only* if there is a
verification failure. When a file fails verification, the format of the
output is a bit cryptic, but it packs all the information you need into
one line per file. Here is the format:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>SM5DLUGT c &lt;file&gt;

        </code></pre></td>
</tr>
</tbody>
</table>

Where:

  - S is the file size.

  - M is the file's mode.

  - 5 is the MD5 checksum of the file.

  - D is the file's major and minor numbers.

  - L is the file's symbolic link contents.

  - U is owner of the file.

  - G is the file's group.

  - T is the modification time of the file.

  - c appears only if the file is a configuration file. This is handy
    for quickly identifying config files, as they are very likely to
    change, and therefore, very *unlikely* to verify successfully.

  - `<file>` is the file that failed verification. The complete path is
    listed to make it easy to find.

It's unlikely that *every* file attribute will fail to verify, so each
of the eight attribute flags will only appear if there is a problem.
Otherwise, a "." will be printed in that flag's place. Let's look at an
example or two:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>.M5....T   /usr/X11R6/lib/X11/fonts/misc/fonts.dir

        </code></pre></td>
</tr>
</tbody>
</table>

In this case, the mode, MD5 checksum, and modification time for the
specified file have failed to verify. The file is not a config file
(Note the absence of a "c" between the attribute list and the filename).

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>S.5....T c /etc/passwd

        </code></pre></td>
</tr>
</tbody>
</table>

Here, the size, checksum, and modification time of the system password
file have all changed. The "c" indicates that this is a config file.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>missing    /var/spool/at/spool

        </code></pre></td>
</tr>
</tbody>
</table>

This last example illustrates what RPM does when a file, that should be
there, is missing entirely.

<div class="sect2">

## <span id="s2-rpm-verify-verification-failure-messages">Other Verification Failure Messages</span>

When **rpm -V** finds other problems, the output is a bit easier to
understand:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -V blather
Unsatisfied dependencies for blather-7.9-1: bother &gt;= 3.1

#
          </code></pre></td>
</tr>
</tbody>
</table>

It's pretty easy to see that the `blather` package requires at least
version 3.1 of the `bother` package.

The output from a package's verification script is a bit harder to
categorize, as the script's contents, as well as its messages, are
entirely up to the package builder.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                        |                          |                                           |
| :------------------------------------- | :----------------------: | ----------------------------------------: |
| [Prev](ch-rpm-verify.html)             |    [Home](index.html)    | [Next](s1-rpm-verify-what-to-verify.html) |
| Using RPM to Verify Installed Packages | [Up](ch-rpm-verify.html) |         Selecting What to Verify, and How |

</div>
