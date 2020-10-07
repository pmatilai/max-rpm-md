<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-install.html)

Chapter 2. Using RPM to Install Packages

[Next](s1-rpm-install-handy-options.html)

-----

<div class="sect1">

# <span id="s1-rpm-install-performing-install">Performing an Install</span>

Let's have RPM install a package. The only thing necessary is to give
the command (**rpm -i**) followed by the name of the package file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -i eject-1.2-2.i386.rpm
#
        </code></pre></td>
</tr>
</tbody>
</table>

At this point, all the steps outlined above have been performed. The
package is now installed. Note that the file name need not adhere to
RPM's file naming convention:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># mv eject-1.2-2.i386.rpm baz.txt
# rpm -i baz.txt
#
        </code></pre></td>
</tr>
</tbody>
</table>

In this case, we changed the name of the package file
`eject-1.2-2.i386.rpm` to `baz.txt` and then proceeded to install the
package. The result is identical to the previous install, that is, the
`eject-1.2-2` package successfully installed. The name of the package
file, although normally incorporating the package label, is not used by
RPM during the installation process. RPM uses the contents of the
package file, which means that even if the file was placed on a DOS
floppy and the name truncated, the installation would still proceed
normally.

<div class="sect2">

## <span id="s2-rpm-install-urls">URLs — Another Way to Specify Package Files</span>

If you've surfed the World Wide Web, you've no doubt noticed the way web
pages are identified:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>http://www.redhat.com/support/docs/rpm/RPM-HOWTO/RPM-HOWTO.html
          </code></pre></td>
</tr>
</tbody>
</table>

This is called a Uniform Resource Locator, or URL. RPM can also use
URLs, although they look a little bit different. Here's one:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>ftp://ftp.redhat.com/pub/redhat/code/rpm/rpm-2.3-1.i386.rpm
          </code></pre></td>
</tr>
</tbody>
</table>

The ftp: signifies that this URL is a File Transfer Protocol URL. As the
name implies, this type of URL is used to move files around. The section
containing `ftp.redhat.com` specifies the hostname, or the name of the
system where the package file resides.

The remainder of the URL (`/pub/redhat/code/rpm/rpm-2.3-1.i386.rpm`)
specifies the path to the package file, followed by the package file
itself.

RPM's use of URLs gives us the ability to install a package located on
the other side of the world, with a single command:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -i ftp://ftp.gnomovision.com/pub/rpms/foobar-1.0-1.i386.rpm
#
          </code></pre></td>
</tr>
</tbody>
</table>

This command would use anonymous FTP to obtain the `foobar` version 1.0
package file and install it on your system. Of course, anonymous FTP (no
username and password required) is not always available. Therefore, the
URL may also contain a username and password preceding the hostname:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>ftp://smith:mypass@ftp.gnomovision.com/pub/rpms/foobar-1.0-1.i386.rpm
          </code></pre></td>
</tr>
</tbody>
</table>

However, entering a password where it can be seen by anyone looking at
your screen is a bad idea. So try this format:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>ftp://smith@ftp.gnomovision.com/pub/rpms/foobar-1.0-1.i386.rpm
          </code></pre></td>
</tr>
</tbody>
</table>

RPM will prompt you for your password, and you'll be in business:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -i ftp://smith@ftp.gnomovision.com/pub/rpms/apmd-2.4-1.i386.rpm
Password for smith@ftp.gnomovision.com: mypass (not echoed)
#
          </code></pre></td>
</tr>
</tbody>
</table>

After entering a valid password, RPM installs the package.

On some systems, the FTP daemon doesn't run on the standard port 21.
Normally this is done for the sake of enhanced security. Fortunately,
there is a way to specify a non-standard port in a URL:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>ftp://ftp.gnomovision.com:1024/pub/rpms/foobar-1.0-1.i386.rpm
          </code></pre></td>
</tr>
</tbody>
</table>

This URL will direct the FTP request to port 1024. The **--ftpport**
option is another way to specify the port. This option is discussed
later, in [the Section called ***--ftpport `<port>`**: Use **`<port>`**
In FTP-based
Installs*](s1-rpm-install-additional-options.html#s2-rpm-install-ftpport).

</div>

<div class="sect2">

## <span id="s2-rpm-install-warning-message">A warning message you might never see</span>

Depending on circumstances, the following message might be rare or very
common. While performing an ordinary install, RPM prints a warning
message:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -i cdp-0.33-100.i386.rpm
warning: /etc/cdp-config saved as /etc/cdp-config.rpmorig
#
          </code></pre></td>
</tr>
</tbody>
</table>

What does it mean? It has to do with RPM's handling of config files. In
the example above, RPM found a file (`/etc/cdp-config`) that didn't
belong to any RPM-installed package. Since the `cdp-0.33-100` package
contains a file of the same name that is to be installed in the same
directory, there is a problem.

RPM solves this the best way it can. It performs two steps:

1.  It renames the original file to `cdp-config.rpmorig`.

2.  It installs the new `cdp-config` file that came with the package.

Continuing our example, if we look in `/etc`, we see that this is
exactly what has happened:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -al /etc/cdp*
-rw-r--r--   1 root     root      119 Jun 23 16:00 /etc/cdp-config
-rw-rw-r--   1 root     root       56 Jun 14 21:44 /etc/cdp-config.rpmorig

#
          </code></pre></td>
</tr>
</tbody>
</table>

This is the best possible solution to a tricky problem. The package is
installed with a config file that is known to work. After all, the
original file may be for an older, incompatible version of the software.
However, the original file is saved so that it can be studied by the
system administrator, who can decide whether the original file should be
put back into service or not.

</div>

</div>

<div class="NAVFOOTER">

-----

|                               |                           |                                           |
| :---------------------------- | :-----------------------: | ----------------------------------------: |
| [Prev](ch-rpm-install.html)   |    [Home](index.html)     | [Next](s1-rpm-install-handy-options.html) |
| Using RPM to Install Packages | [Up](ch-rpm-install.html) |                         Two handy options |

</div>
