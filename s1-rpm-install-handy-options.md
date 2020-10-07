<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-install-performing-install.html)

Chapter 2. Using RPM to Install Packages

[Next](s1-rpm-install-additional-options.html)

-----

<div class="sect1">

# <span id="s1-rpm-install-handy-options">Two handy options</span>

There are two options to **rpm -i** that work so well, and are so
useful, you might think they should be RPM's default behavior. They
aren't, but using them only requires that you type an extra two
characters:

<div class="sect2">

## <span id="s2-rpm-install-more-feedback">Getting a bit more feedback with **-v**</span>

Even though **rpm -i** is doing many things, it's not very exciting, is
it? When performing installs, RPM is pretty quiet, unless something goes
wrong. However, we can ask for a bit more output by adding **-v** to the
command:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -iv eject-1.2-2.i386.rpm
Installing eject-1.2-2.i386.rpm
#
          </code></pre></td>
</tr>
</tbody>
</table>

By adding **-v**, RPM displayed a simple status line. Using **-v** is a
good idea, particularly if you're going to use a single command to
install more than one package:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -iv *.rpm
Installing eject-1.2-2.i386.rpm
Installing iBCS-1.2-3.i386.rpm
Installing logrotate-1.0-1.i386.rpm

#
          </code></pre></td>
</tr>
</tbody>
</table>

In this case, there were three .rpm files in the directory. By using a
simple wildcard, it's as easy to install one package as it is to install
one hundred\!

</div>

<div class="sect2">

## <span id="s2-rpm-install-install-h">**-h**: Perfect for the Impatient</span>

Sometimes a package can be quite large. Other than watching the disk
activity light flash, there's no assurance that RPM is working, and if
it is, how far along it is. If you add **-h**, RPM will print fifty hash
marks ("\#") as the install proceeds:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -ih eject-1.2-2.i386.rpm
##################################################
#
          </code></pre></td>
</tr>
</tbody>
</table>

Once all fifty hash marks are printed, the package is completely
installed. Using **-v** with **-h** results in a very nice display,
particularly when installing more than one package:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -ivh *.rpm
eject          ##################################################
iBCS           ##################################################
logrotate      ##################################################

#
          </code></pre></td>
</tr>
</tbody>
</table>

</div>

</div>

<div class="NAVFOOTER">

-----

|                                                |                           |                                                |
| :--------------------------------------------- | :-----------------------: | ---------------------------------------------: |
| [Prev](s1-rpm-install-performing-install.html) |    [Home](index.html)     | [Next](s1-rpm-install-additional-options.html) |
| Performing an Install                          | [Up](ch-rpm-install.html) |               Additional options to **rpm -i** |

</div>
