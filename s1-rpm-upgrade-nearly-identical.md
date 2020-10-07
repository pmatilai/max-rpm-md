<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-upgrade-upgrading-a-package.md)

Chapter 4. Using RPM to Upgrade Packages

[Next](ch-rpm-query.md)

-----

<div class="sect1">

# <span id="s1-rpm-upgrade-nearly-identical">They're Nearly Identical…</span>

Given the fact that **rpm -U** can be used as a replacement to **rpm
-i**, it follows that most of the options available for **rpm -U** are
identical to those used with **rpm -i**. Therefore, to keep the
duplication to a minimum, we'll discuss only those options that are
unique to **rpm -U**, or that behave differently from the same option
when used with **rpm -i**. The table on [Table
4-1](ch-rpm-upgrade.md#tb-rpm-upgrade-syntax) at the start of this
chapter shows all valid options to RPM's upgrade command, and indicates
which are identical to those used with **rpm -i**.

<div class="sect2">

## <span id="s2-rpm-upgrade-oldpackage-option">**--oldpackage**: Upgrade To An Older Version</span>

This option might be used a bit more by people that like to stay on the
"bleeding edge" of new versions of software, but eventually, everyone
will probably need to use it. Usually, the situation plays out like
this:

  - You hear about some new software that sounds pretty nifty, so you
    download the `.rpm` file and install it.

  - The software is *great*\! It does everything you ask for, and more.
    You end up using it every day for the next few months.

  - You hear that a new version of your favorite software is available.
    You waste no time in getting the package. You upgrade the software
    by using **rpm -U**. No problem\!

  - Fingers arched in anticipation, you launch the new version. Your
    computer's screen goes blank\!

Looks like a bug in the new version. *Now* what do you do? Hmmm. Maybe
you can just "upgrade" to the older version. Let's try to go back to
release 2 of `cdp-0.33` from release 3:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Uv cdp-0.33-2.i386.rpm
Installing cdp-0.33-2.i386.rpm
package cdp-0.33-3 (which is newer) is already installed
error: cdp-0.33-2.i386.rpm cannot be installed

#
          </code></pre></td>
</tr>
</tbody>
</table>

That didn't work very well. At least it told us just what the problem
was — we were trying to upgrade to an older version of a package that is
already installed. Fortunately, there's a special option for just this
situation: **--oldpackage**. Let's give it a try:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -Uv --oldpackage cdp-0.33-2.i386.rpm
Installing cdp-0.33-2.i386.rpm

#
          </code></pre></td>
</tr>
</tbody>
</table>

By using the **--oldpackage** option, release 3 of `cdp-0.33` is
history, and has been replaced by release 2.

</div>

<div class="sect2">

## <span id="s2-rpm-upgrade-force-option">**--force**: The Big Hammer</span>

Adding **--force** to an upgrade command is a way of saying "Upgrade it
anyway\!" In essence, it adds **--replacepkgs**, **--replacefiles**,
*and* **--oldpackage** to the command. Like a big hammer, **--force** is
an irresistible force
[<span class="footnote">\[1\]</span>](#FTN.AEN2340) that makes things
happen. In fact, the only thing that will prevent a **--force**'ed
upgrade from proceeding is a dependency conflict.

And like a big hammer, it pays to fully understand why you need to use
**--force** before actually using it.

</div>

<div class="sect2">

## <span id="s2-rpm-upgrade-noscripts-option">**--noscripts**: Do Not Execute Install and Uninstall Scripts</span>

The **--noscripts** option prevents a package's pre- and post-install
scripts from being executed. This is no different than the option's
behavior when used with RPM's install command. However, there is an
additional point to consider when the option is used during an upgrade.
The following example uses specially-built packages that display
messages when their scripts are executed by RPM:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -i bother-2.7-1.i386.rpm
This is the bother 2.7 preinstall script
This is the bother 2.7 postinstall script

#
          </code></pre></td>
</tr>
</tbody>
</table>

In this case, a package has been installed. As expected, its scripts are
executed. Next, let's upgrade this package:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -U bother-3.5-1.i386.rpm
This is the bother 3.5 preinstall script
This is the bother 3.5 postinstall script
This is the bother 2.7 preuninstall script
This is the bother 2.7 postuninstall script

#
          </code></pre></td>
</tr>
</tbody>
</table>

This is a textbook example of the sequence of events during an upgrade.
The new version of the package is installed (as shown by the pre- and
post-install scripts being executed). Finally, the previous version of
the package is removed (showing the pre- and post-*un*install scripts
being executed).

There are really no surprises there — it worked just the way it was
meant to. This time, let's use the **--noscripts** option when the time
comes to perform the upgrade:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -i bother-2.7-1.i386.rpm
This is the bother 2.7 preinstall script
This is the bother 2.7 postinstall script

#
          </code></pre></td>
</tr>
</tbody>
</table>

Again, the first package is installed, and its scripts are executed. Now
let's try the upgrade using the **--noscripts** option:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -U --noscripts bother-3.5-1.i386.rpm
This is the bother 2.7 preuninstall script
This is the bother 2.7 postuninstall script

#
          </code></pre></td>
</tr>
</tbody>
</table>

The difference here is that the **--noscripts** option prevented the new
package's scripts from executing. The scripts from the package being
erased were still executed.

</div>

</div>

### Notes

|                                                                                     |               |
| ----------------------------------------------------------------------------------- | ------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-upgrade-nearly-identical.md#AEN2340) | Pun intended. |

<div class="NAVFOOTER">

-----

|                                                 |                           |                                    |
| :---------------------------------------------- | :-----------------------: | ---------------------------------: |
| [Prev](s1-rpm-upgrade-upgrading-a-package.md) |    [Home](index.md)     |          [Next](ch-rpm-query.md) |
| Upgrading a Package                             | [Up](ch-rpm-upgrade.md) | Getting Information About Packages |

</div>
