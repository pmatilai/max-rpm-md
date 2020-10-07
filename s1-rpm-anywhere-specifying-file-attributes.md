<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-anywhere-different-build-area.html)

Chapter 16. Making a Package That Can Build Anywhere

[Next](ch-rpm-pgp.html)

-----

<div class="sect1">

# <span id="s1-rpm-anywhere-specifying-file-attributes">Specifying File Attributes</span>

In cases where the package builder cannot create the files to be
packaged with the proper ownership and permissions, the **%attr** macro
can be used to make things right.

<div class="sect2">

## <span id="s2-rpm-anywhere-attr-macro">**%attr** — How Does It Work?</span>

The **%attr** macro has the following format:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%attr(&lt;mode&gt;, &lt;user&gt;, &lt;group&gt;) &lt;file&gt;

          </code></pre></td>
</tr>
</tbody>
</table>

  - The `<mode>` is represented in traditional numeric fashion.

  - The `<user>` is specified by the login name of the user. Numeric
    UIDs are *not* used, for reasons we'll explore in a moment.

  - The `<group>` is specified by the group's name, as entered in
    `/etc/group`. Numeric GIDs are *not* used, either. Yes, we'll be
    discussing that, too\!

  - `<file>` represents the file. Shell-style globbing is supported.

There are a couple other wrinkles to using the **%attr** macro. If a
particular file attribute doesn't need to be specified, that attribute
can be replaced with a dash "**-**" and **%attr** will not change it.
Say, for instance, that a package's files are installed with the
permissions correctly set, as they almost always are. Instead of having
to go to the trouble of entering the permissions for each and every
file, each file can have the same **%attr**:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%attr(-, root, root)

          </code></pre></td>
</tr>
</tbody>
</table>

This works for user and group specifications, as well.

The other wrinkle is that, although we've been showing the three file
attributes separated by commas, in reality they could be separated by
spaces as well. Whichever delimiter you choose, it pays to be consistent
throughout a spec file.

Let's fix up `cdplayer` with a liberal sprinkling of **%attr**s. Here's
what the **%files** list looks like after we've had our way with it:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%files
%attr(-, root, root) %doc README
%attr(4755, root, root) /usr/local/bin/cdp
%attr(-, root, root) /usr/local/bin/cdplay
%attr(-, root, rot) /usr/local/man/man1/cdp.1

          </code></pre></td>
</tr>
</tbody>
</table>

A couple points are worth noting here. The line for `README` shows that
multiple macros can be used on a line — in this case, one to set file
attributes, and one to mark the file as being documentation. The
**%attr** for `/usr/local/bin/cdp` declares the file to be setuid root.
If it sends a shiver down your spine to know that anybody can create a
package that will run setuid root when installed on your system — Good\!
Just because RPM makes it easy to install software doesn't mean that you
should blindly install every package you find.

A single RPM command can quickly point out areas of potential problems
and should be issued on any package file whose creators you don't trust:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>% rpm -qlvp ../RPMS/i386/cdplayer-1.0-1.i386.rpm
drwxr-xr-x-  root  root   1024 Sep 13 20:16 /usr/doc/cdplayer-1.0-1
-rw-r--r---  root  root   1085 Nov 10 01:10 /usr/doc/cdplayer-1.0-1/README
-rwsr-xr-x-  root  root  40739 Sep 13 21:32 /usr/local/bin/cdp
lrwxrwxrwx-  root  root     47 Sep 13 21:32 /usr/local/bin/cdplay -&gt; ./cdp
-rwxr-xr-x-  root   rot   4550 Sep 13 21:32 /usr/local/man/man1/cdp.1

% 
          </code></pre></td>
</tr>
</tbody>
</table>

Sure enough — there's that setuid root file. In this case we trust the
package builder, so let's install it:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -ivh cdplayer-1.0-1.i386.rpm
cdplayer       ##################################################
group rot does not exist - using root

#
          </code></pre></td>
</tr>
</tbody>
</table>

What's this about group "rot"? Looking back at the **rpm -qlvp** output,
it looks like `/usr/local/man/man1/cdp.1` has a bogus group. Looking
back even further, it's there in the **%attr** for that file. Must have
been a typo. We could pretend that RPM used advanced artificial
intelligence technology to come to the same conclusion as we did and
made the appropriate change, but in reality, RPM simply used the only
group identifier it could count on — root. RPM will do the same thing if
it can't resolve a user specification.

Let's look at some of the files the package installed, including that
worrisome setuid root file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls /usr/local/bin
total 558
-rwsr-xr-x   1 root     root        40739 Sep 13 21:32 cdp*
lrwxrwxrwx   1 root     root           47 Sep 13 21:36 cdplay -&gt; ./cdp*

# 
          </code></pre></td>
</tr>
</tbody>
</table>

RPM did just what it was supposed to — It gave the files the attributes
specified by the **%attr** macros.

</div>

<div class="sect2">

## <span id="s2-rpm-anywhere-betcha">Betcha Thought We Forgot…</span>

At the start of this section, we mentioned that the **%attr** macro
wouldn't accept numeric uids or gids, and we promised to explain why.
The reason is simply that, even if a package requires a certain user or
group to own the package's files, the user may not have the same uid/gid
from system to system. There — wasn't that simple?

In the next chapter, we'll discuss how to make your packaged software
safe against modification by unscrupulous people. The name of the game
is Pretty Good Privacy, and you'll see how signing packages with PGP is
easier than you think\!

</div>

</div>

<div class="NAVFOOTER">

-----

|                                                   |                            |                                    |
| :------------------------------------------------ | :------------------------: | ---------------------------------: |
| [Prev](s1-rpm-anywhere-different-build-area.html) |     [Home](index.html)     |            [Next](ch-rpm-pgp.html) |
| Having RPM Use a Different Build Area             | [Up](ch-rpm-anywhere.html) | Adding PGP Signatures to a Package |

</div>
