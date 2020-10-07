<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-erase-and-config-files.html)

Chapter 3. Using RPM to Erase Packages

[Next](ch-rpm-upgrade.html)

-----

<div class="sect1">

# <span id="s1-rpm-erase-watch-out">Watch Out\!</span>

RPM takes most of the work out of removing software from your system,
and that's great. As with everything else in life, however, there's a
downside. RPM also makes it easy to erase packages that are critical to
your system's continued operation. Here are some examples of packages
*not* to erase:

  - RPM: RPM will happily uninstall itself. No problem — you'll just
    re-install it with **rpm -i**… Oops\!

  - Bash: The Bourne-again Shell may not be the shell you use, but
    certain parts of many Linux systems (like the scripts executed
    during system startup and shutdown) use `/bin/sh`, which is a
    symbolic link to `/bin/bash`. No `/bin/bash`, no `/bin/sh`. No
    `/bin/sh`, no system\!

In many cases, RPM's dependency processing will prevent inadvertent
erasures from causing massive problems. However, if you're not sure, use
**rpm -q** to get more information about the package you'd like to
erase. [<span class="footnote">\[1\]</span>](#FTN.AEN1905)

</div>

### Notes

|                                                                            |                                                                        |
| -------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-erase-watch-out.html#AEN1905) | See [Chapter 5](ch-rpm-query.html) for more information on **rpm -q**. |

<div class="NAVFOOTER">

-----

|                                            |                         |                               |
| :----------------------------------------- | :---------------------: | ----------------------------: |
| [Prev](s1-rpm-erase-and-config-files.html) |   [Home](index.html)    |   [Next](ch-rpm-upgrade.html) |
| **rpm -e** and Config files                | [Up](ch-rpm-erase.html) | Using RPM to Upgrade Packages |

</div>
