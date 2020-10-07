<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-anywhere.html)

Chapter 16. Making a Package That Can Build Anywhere

[Next](s1-rpm-anywhere-specifying-file-attributes.html)

-----

<div class="sect1">

# <span id="s1-rpm-anywhere-different-build-area">Having RPM Use a Different Build Area</span>

While RPM's build root requires a certain amount of spec file and make
file tweaking in order to get it working properly, directing RPM to
perform the build in a different directory is a snap. The hardest part
is to create the directories RPM will use during the build process.

<div class="sect2">

## <span id="s2-rpm-anywhere-creating-build-area">Setting up a Build Area</span>

RPM's build area consists of five directories in the top-level:

1.  The `BUILD` directory is where the software is unpacked and built.

2.  The `RPMS` directory is where the newly created binary package files
    are written.

3.  The `SOURCES` directory contains the original sources, patches, and
    icon files.

4.  The `SPECS` directory contains the spec files for each package to be
    built.

5.  The `SRPMS` directory is where the newly created source package
    files are written.

The description of the `RPMS` directory above, is missing one key point.
Since the binary package files are specific to an architecture, the
directory actually contains one or more subdirectories, one for each
architecture. It is in these subdirectories that RPM will write the
binary package files.

Let's start by creating the directories. We can even do it with one
command:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>% pwd
/home/ed
% mkdir mybuild\
?  mybuild/BUILD\
?  mybuild/RPMS\
?  mybuild/RPMS/i386\
?  mybuild/SOURCES\
?  mybuild/SPECS\
?  mybuild/SRPMS\
%
          </code></pre></td>
</tr>
</tbody>
</table>

That's all there is to it. You may have noticed that we created a
subdirectory to `RPMS` called `i386` — This is the architecture-specific
subdirectory for Intel x86-based systems, which is our example build
system.

The next step in getting RPM to use a different build area is telling
RPM where the new build area is. And it's almost as easy as creating the
build area itself.

</div>

<div class="sect2">

## <span id="s2-rpm-anywhere-using-new-build-area">Directing RPM to Use the New Build Area</span>

All that's required to get RPM to start using the new build area is to
define an alternate value for **topdir** in an `rpmmacros` file. For the
non-root user, this means putting the following line in a file called
`.rpmmacros`, located in your home directory:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%_topdir &lt;path&gt;

          </code></pre></td>
</tr>
</tbody>
</table>

By replacing `<path>` with the path to the new build area's top-level
directory, RPM will attempt to use it the next time a build is
performed. Using our newly created build area as an example, we'll set
**topdir** to `/home/ed/mybuild`:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%_topdir /home/ed/mybuild

          </code></pre></td>
</tr>
</tbody>
</table>

That's all there is to it. Now it's time to try a build.

</div>

<div class="sect2">

## <span id="s2-rpm-anywhere-performing-build">Performing a Build in a New Build Area</span>

In the following example, a non-root user attempts to build the
`cdplayer` package in a personal build area. If the user has modified
`rpmrc` file entries to change the default build area, the command used
to start the build is just like the one used by a root user. Otherwise,
the **--buildroot** option will need to be used:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>% cd /home/ed/mybuild/SPECS
% rpmbuild -ba --buildroot /home/ed/mybuildroot cdplayer-1.0.spec
* Package: cdplayer
+ umask 022
Executing: %prep
+ cd /home/ed/mybuild/BUILD
+ cd /home/ed/mybuild/BUILD
+ rm -rf cdplayer-1.0
+ gzip -dc /home/ed/mybuild/SOURCES/cdplayer-1.0.tgz
+ tar -xvvf -
drwxrwxr-x root/users        0 Aug 20 20:58 1996 cdplayer-1.0/
-rw-r--r-- root/users    17982 Nov 10 01:10 1995 cdplayer-1.0/COPYING
…
+ cd /home/ed/mybuild/BUILD/cdplayer-1.0
+ chmod -R a+rX,g-w,o-w .
+ exit 0
Executing: %build
+ cd /home/ed/mybuild/BUILD
+ cd cdplayer-1.0
+ make
gcc -Wall -O2  -c -I/usr/include/ncurses  cdp.c 
…
Executing: %install
+ cd /home/ed/mybuild/BUILD
+ make ROOT=/home/ed/mybuildroot/cdplayer install
install -m 755 -o 0 -g 0 -d /home/ed/mybuildroot/cdplayer/usr/local/bin/
install: /home/ed/mybuildroot/cdplayer: Operation not permitted
install: /home/ed/mybuildroot/cdplayer/usr: Operation not permitted
install: /home/ed/mybuildroot/cdplayer/usr/local: Operation not permitted
install: /home/ed/mybuildroot/cdplayer/usr/local/bin: Operation not
permitted
install: /home/ed/mybuildroot/cdplayer/usr/local/bin/: Operation not
permitted
make: *** [install] Error 1
Bad exit status

% 
          </code></pre></td>
</tr>
</tbody>
</table>

Things started off pretty well — The **%prep** section of the spec file
unpacked the sources into the new build area, as did the **%build**
section. The build was proceeding normally in the user-specified build
area, and root access was not required. In the **%install** section,
however, things started to fall apart. What happened?

Take a look at that **install** command. The two options, "**-o 0**" and
"**-g 0**", dictate that the directories to be created in the build root
are to be owned by the root account. Since the user performing this
build did not have root access, the **install** failed, and rightly so.

OK, let's remove the offending options and see where that gets us.
Here's the install section of the make file after our modifications:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>install: cdp cdp.1.Z
    install -m 755 -d $(ROOT)/usr/local/bin/
    install -m 755 cdp $(ROOT)/usr/local/bin/cdp
    rm -f $(ROOT)/usr/local/bin/cdplay
    ln -s ./cdp $(ROOT)/usr/local/bin/cdplay
    install -m 755 -d $(ROOT)/usr/local/man/man1/
    install -m 755 cdp.1 $(ROOT)/usr/local/man/man1/cdp.1

          </code></pre></td>
</tr>
</tbody>
</table>

We'll spare you from having to read through another build, but this time
it completed successfully. Now, let's put our sysadmin hat on and
install the newly built package:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpm -ivh cdplayer-1.0-1.i386.rpm
cdplayer       ##################################################

#
          </code></pre></td>
</tr>
</tbody>
</table>

Well, that was easy enough. Let's take a look at some of the files and
make sure everything looks OK. We know there are some files installed in
`/usr/local/bin`, so let's check those:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -al /usr/local/bin
-rwxr-xr-x   1 ed       ed          40739 Sep 13 20:16 cdp*
lrwxrwxrwx   1 ed       ed             47 Sep 13 20:34 cdplay -&gt; ./cdp*

#
          </code></pre></td>
</tr>
</tbody>
</table>

Looks pretty good… Wait a minute\! What's up with the owner and group?
The answer is simple: User `ed` ran the build, which executed the make
file, which ran **install**, which created the files. Since `ed` created
the files, they are owned by him.

This brings up an interesting point. Software must be installed with
very specific file ownership and permissions. But a non-root user can't
create files that are owned by anyone other than his or herself. What is
a non-root user to do?

</div>

</div>

<div class="NAVFOOTER">

-----

|                                          |                            |                                                         |
| :--------------------------------------- | :------------------------: | ------------------------------------------------------: |
| [Prev](ch-rpm-anywhere.html)             |     [Home](index.html)     | [Next](s1-rpm-anywhere-specifying-file-attributes.html) |
| Making a Package That Can Build Anywhere | [Up](ch-rpm-anywhere.html) |                              Specifying File Attributes |

</div>
