<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-erase-additional-options.html)

Chapter 3. Using RPM to Erase Packages

[Next](s1-rpm-erase-watch-out.html)

-----

<div class="sect1">

# <span id="s1-rpm-erase-and-config-files">**rpm -e** and Config files</span>

If you've made changes to a configuration file that was originally
installed by RPM, your changes won't be lost if you erase the package.
Say, for example, that we've made changes to `/etc/skel/.bashrc` (a
config file), which was installed as part of the `etcskel` package.
Later, we remove `etcskel`:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -e etcskel
#
        </code></pre></td>
</tr>
</tbody>
</table>

But if we take a look in `/etc/skel`, look what's there:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -al
total 5
drwxr-xr-x   3 root     root         1024 Jun 17 22:01 .
drwxr-xr-x   8 root     root         2048 Jun 17 19:01 ..
-rw-r--r--   1 root     root          152 Jun 17 21:54 .bashrc.rpmsave
drwxr-xr-x   2 root     root         1024 May 13 13:18 .xfm

#
        </code></pre></td>
</tr>
</tbody>
</table>

Sure enough: `.bashrc.rpmsave` is a copy of your modified `.bashrc`
file\! Remember, however, that this feature only works with config
files. Not sure how to determine which files RPM thinks are config
files? [Chapter 5](ch-rpm-query.html) will show you how.

</div>

<div class="NAVFOOTER">

-----

|                                              |                         |                                     |
| :------------------------------------------- | :---------------------: | ----------------------------------: |
| [Prev](s1-rpm-erase-additional-options.html) |   [Home](index.html)    | [Next](s1-rpm-erase-watch-out.html) |
| Additional Options                           | [Up](ch-rpm-erase.html) |                         Watch Out\! |

</div>
