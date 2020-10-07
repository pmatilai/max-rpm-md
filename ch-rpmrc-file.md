<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-file-format-file-command.html)

[Next](s1-rpmrc-file-rpmrc-file-locations.html)

-----

<div class="appendix">

# <span id="ch-rpmrc-file"></span>Appendix B. The `rpmrc` File

The `rpmrc` file is used to control RPM's actions. The file's entries
have an effect on nearly every aspect of RPM's operations. Here, we
describe the `rpmrc` files in more detail, as well as the command used
to show how RPM interprets the files.

<div class="sect1">

# <span id="s1-rpmrc-file-showrc-option">Using the **--showrc** Option</span>

As we'll see in a moment, RPM can read more than one `rpmrc` file, and
each file can contain nearly thirty different types of entries. This can
make it difficult to determine what values RPM is actually using.

Luckily, there's an option that can be used to help make sense of it
all. The **--showrc** option displays the value for each of the entries.
The output is divided into two sections:

1.  Architecture and operating system values.

2.  `rpmrc` values.

The architecture and operating system values define the architecture and
operating system that RPM is running on. These values define the
environment for both building and installing packages. They also define
which architectures and operating systems are compatible with each
other.

The `rpmrc` values define many aspects of RPM's operation. These values
range from the path to RPM's database, to the name of the person listed
as having built the package.

Here's an example of **--showrc**'s output:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm --showrc
ARCHITECTURE AND OS:
build arch           : i386
build os             : Linux
install arch         : i486
install os           : Linux
compatible arch list : i486 i386
compatible os list   : Linux
RPMRC VALUES:
builddir             : /usr/src/redhat/BUILD
buildroot            : (not set)
cpiobin              : cpio
dbpath               : /var/lib/rpm
defaultdocdir        : /usr/doc
distribution         : (not set)
excludedocs          : (not set)
ftpport              : (not set)
ftpproxy             : (not set)
messagelevel         : (not set)
netsharedpath        : (not set)
optflags             : -O2 -m486 -fno-strength-reduce
packager             : (not set)
pgp_name             : (not set)
pgp_path             : (not set)
require_distribution : (not set)
require_icon         : (not set)
require_vendor       : (not set)
root                 : (not set)
rpmdir               : /usr/src/redhat/RPMS
signature            : none
sourcedir            : /usr/src/redhat/SOURCES
specdir              : /usr/src/redhat/SPECS
srcrpmdir            : /usr/src/redhat/SRPMS
timecheck            : (not set)
tmppath              : /var/tmp
topdir               : /usr/src/redhat
vendor               : (not set)

#
      </code></pre></td>
</tr>
</tbody>
</table>

As you can see, the **--showrc** option clearly displays the values RPM
will use. **--showrc** can also be used with the **--rcfile** option,
which makes it easy to see the effect of specifying a different `rpmrc`
file.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                                    |                    |                                                 |
| :------------------------------------------------- | :----------------: | ----------------------------------------------: |
| [Prev](s1-rpm-file-format-file-command.html)       | [Home](index.html) | [Next](s1-rpmrc-file-rpmrc-file-locations.html) |
| Identifying RPM files with the **file(1)** command | [Up](p14028.html)  |        Different Places an `rpmrc` File Resides |

</div>
