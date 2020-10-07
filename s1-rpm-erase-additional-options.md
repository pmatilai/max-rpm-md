<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-erase-erasing-package.html)

Chapter 3. Using RPM to Erase Packages

[Next](s1-rpm-erase-and-config-files.html)

-----

<div class="sect1">

# <span id="s1-rpm-erase-additional-options">Additional Options</span>

If you're interested in a complex command with lots of options, **rpm
-e** is *not* the place to look. There just aren't that many different
ways to erase a package\! But there are a few options you should know
about.

<div class="sect2">

## <span id="s2-rpm-erase-test-option">**--test** — Go Through the Process of Erasing the Package, But Do Not Erase It</span>

If you're a bit gun-shy about erasing a package, you can use the
**--test** option first to see what **rpm -e** would do:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -e --test bother
removing these packages would break dependencies:
        bother &gt;= 3.1 is needed by blather-7.9-1

#
          </code></pre></td>
</tr>
</tbody>
</table>

It's pretty easy to see that the `blather` package wouldn't work very
well if `bother` were erased. To be fair, however, RPM wouldn't have
erased the package in this example unless we used the **--nodeps**
option, which we'll discuss shortly.

However, if there are no problems erasing the package, you won't see
very much:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -e --test eject
#
          </code></pre></td>
</tr>
</tbody>
</table>

We know, based on previous experience, that **-v** doesn't give us any
additional output with **rpm -e**. However, we *do* know that **-vv**
works wonders. Let's see what *it* has to say:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -evv --test eject
D: uninstalling record number 286040
D: running preuninstall script (if any)
D: would remove files test = 1
D: /usr/man/man1/eject.1 - would remove
D: /usr/bin/eject - would remove
D: running postuninstall script (if any)
D: would remove database entry

#
          </code></pre></td>
</tr>
</tbody>
</table>

As you can see, the output is similar to that of a regular erase command
using the **-vv** option, with the following exceptions:

  - The "would remove files test = 1" line ends with a non-zero number.
    This is because **--test** has been added. If the command hadn't
    included **--test**, the number would have been 0, and the package
    would have been erased.

  - There is a line for each file that RPM would have removed, each one
    ending with "would remove" instead of "removing".

  - There is only one line at the end, stating: "would remove database
    entry", versus the multi-line output showing the cleanup of the RPM
    database during an actual erase.

By using **--test** in conjunction with **-vv**, it's easy to see
exactly what RPM would do during an actual erase.

</div>

<div class="sect2">

## <span id="s2-rpm-erase-nodeps-option">**--nodeps**: Do Not Check Dependencies Before Erasing Package</span>

It's likely that one day while erasing a package, you'll see something
like this:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -e bother
removing these packages would break dependencies:
        bother &gt;= 3.1 is needed by blather-7.9-1

#
          </code></pre></td>
</tr>
</tbody>
</table>

What happened? The problem is that one or more of the packages installed
on your system require the package you're trying to erase. Without it,
they won't work properly. In our example, the `blather` package won't
work properly unless the `bother` package (and more specifically,
`bother` version 3.1 or later) is installed. Since we're trying to erase
`bother`, RPM aborted the erasure.

Now, 99 times out of 100, this is exactly the right thing for RPM to do.
After all, if the package is needed by other packages, why try to erase
it? As with everything else in life, there are exceptions to the rule.
And that is why there is a **--nodeps** option.

Adding the **--nodeps** options to an erase command directs RPM to
ignore any dependency-related problems, and to erase the package. Going
back to our example above, let's add the **--nodeps** option to the
command line and see what happens:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -e --nodeps bother
#
          </code></pre></td>
</tr>
</tbody>
</table>

The package was erased without a peep. Whether the `blather` package
will work properly is another matter. In general, it's not a good idea
to use **--nodeps** to get around dependency problems. The package
builders included the dependency requirements for a reason, and it's
best not to second-guess them.

</div>

<div class="sect2">

## <span id="s2-rpm-erase-noscripts-option">**--noscripts** — Do *Not* Execute Pre- and Post-uninstall Scripts</span>

In [the Section called *Getting More Information With
**-vv***](s1-rpm-erase-erasing-package.html#s2-rpm-erase-vv-option), we
used the **-vv** option to see what RPM was actually doing when it
erased a package. We noted that there were two scripts, a pre-uninstall
and a post-uninstall, that were used to execute commands required during
the process of erasing a package.

The **--noscripts** option prevents these scripts from being executed
during an erase. *This is a very dangerous thing to do\!* The
**--noscripts** option is really meant for package builders to use
during the development of their packages. By preventing the pre- and
post-uninstall scripts from running, a package builder can keep a buggy
package from bringing down their development system. Once the bugs are
found and eliminated, there's very little need to prevent these scripts
from running; in fact, doing so can *cause* problems\!

</div>

<div class="sect2">

## <span id="s2-rpm-erase-rcfile-option">**--rcfile `<rcfile>`** — Read **`<rcfile>`** For RPM Defaults</span>

The **--rcfile** option is used to specify a file containing default
settings for RPM. Normally, this option is not needed. By default, RPM
uses `/etc/rpmrc` and a file named `.rpmrc` located in your login
directory.

This option would be used if there was a need to switch between several
sets of RPM defaults. Software developers and package builders will
normally be the only people using the **--rcfile** option. For more
information on `rpmrc` files, see [Appendix B](ch-rpmrc-file.html).

</div>

<div class="sect2">

## <span id="s2-rpm-erase-root-option">**--root `<path>`** — Use **`<path>`** As the Root</span>

Adding **--root `<path>`** to an install command forces RPM to assume
that the directory specified by **`<path>`** is actually the "root"
directory. The **--root** option affects every aspect of the install
process, so pre- and post-install scripts are run with **`<path>`** as
their root directory (using `chroot(2)`, if you must know). In addition,
RPM expects its database to reside in the directory specified by the
**dbpath** `rpmrc` file entry, relative to **`<path>`**.
[<span class="footnote">\[1\]</span>](#FTN.AEN1831)

Normally this option is only used during an initial system install, or
when a system has been booted off a "rescue disk" and some packages need
to be re-installed.

</div>

<div class="sect2">

## <span id="s2-rpm-erase-dbpath-option">**--dbpath `<path>`**: Use **`<path>`** To Find RPM Database</span>

In order for RPM to do its handiwork, it needs access to an RPM
database. Normally, this database exists in the directory specified by
the `rpmrc` file entry, **dbpath**. By default, **dbpath** is set to
`/var/lib/rpm`.

Although the **dbpath** entry can be modified in the appropriate `rpmrc`
file, the **--dbpath** option is probably a better choice when the
database path needs to be changed temporarily. An example of a time the
**--dbpath** option would come in handy is when it's necessary to
examine an RPM database copied from another system. Granted, it's not a
common occurrence, but it's difficult to handle any other way.

</div>

</div>

### Notes

|                                                                                     |                                                                                     |
| ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-erase-additional-options.html#AEN1831) | For more information on `rpmrc` file entries, see [Appendix B](ch-rpmrc-file.html). |

<div class="NAVFOOTER">

-----

|                                           |                         |                                            |
| :---------------------------------------- | :---------------------: | -----------------------------------------: |
| [Prev](s1-rpm-erase-erasing-package.html) |   [Home](index.html)    | [Next](s1-rpm-erase-and-config-files.html) |
| Erasing a Package                         | [Up](ch-rpm-erase.html) |                **rpm -e** and Config files |

</div>
