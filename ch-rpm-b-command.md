<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-build-when-things-go-wrong.html)

[Next](s1-rpm-b-command-other-build-commands.html)

-----

<div class="chapter">

# <span id="ch-rpm-b-command"></span>Chapter 12. **rpmbuild** Command Reference

<div class="table">

<span id="tb-rpm-b-command-"></span>

**Table 12-1. **rpmbuild** Command Syntax**

**rpmbuild** **-b`<stage>` `options` `file1.spec` … `fileN.spec`**

</div>

</div>

`<stage>`

Page

**p**

Execute **%prep**

[the Section called ***rpmbuild** **-bp** — Execute
**%prep***](ch-rpm-b-command.html#s2-rpm-b-command-bp-option)

**c**

Execute **%prep**, **%build**

[the Section called ***rpmbuild** **-bc** — Execute **%prep**,
**%build***](ch-rpm-b-command.html#s2-rpm-b-command-bc-option)

**i**

Execute **%prep**, **%build**, **%install**, **%check**

[the Section called ***rpmbuild** **-bi** — Execute **%prep**,
**%build**, **%install**,
**%check***](ch-rpm-b-command.html#s2-rpm-b-command-bi-option)

**b**

Execute **%prep**, **%build**, **%install**, **%check**, package (bin)

[the Section called ***rpmbuild** **-bb** — Execute **%prep**,
**%build**, **%install**, **%check**, package
(bin)*](ch-rpm-b-command.html#s2-rpm-b-command-bb-option)

**a**

Execute **%prep**, **%build**, **%install**, **%check**, package (bin,
src)

[the Section called ***rpmbuild** **-ba** — Execute **%prep**,
**%build**, **%install**, **%check**, package (bin,
src)*](ch-rpm-b-command.html#s2-rpm-b-command-ba-option)

**l**

Check **%files** list

[the Section called ***rpmbuild** **-bl** — Check **%files**
list*](ch-rpm-b-command.html#s2-rpm-b-command-bl-option)

Parameters

`file1.spec` … `fileN.spec`

One or more `.spec` files

Build-specific Options

Page

**--short-circuit**

Force build to start at particular stage (**-bc**, **-bi** only)

[the Section called ***--short-circuit** — Force build to start at
particular
stage*](ch-rpm-b-command.html#s2-rpm-b-command-short-circuit-option)

**--test**

Create, save build scripts for review

[the Section called ***--test** — Create, Save Build Scripts For
Review*](ch-rpm-b-command.html#s2-rpm-b-command-test-option)

**--clean**

Clean up after build

[the Section called ***--clean** — Clean up after
build*](ch-rpm-b-command.html#s2-rpm-b-command-clean-option)

**--sign**

Add a digital signature to the package

[the Section called ***--sign** — Add a Digital Signature to the
Package*](ch-rpm-b-command.html#s2-rpm-b-command-sign-option)

**--buildroot `<root>`**

Execute **%install** using **`<root>`** as the root

[the Section called ***--buildroot `<path>`** — Execute **%install**
using **`<path>`** as the
root*](ch-rpm-b-command.html#s2-rpm-b-command-buildroot-option)

**--buildarch `<arch>`**

Perform build for the **`<arch>`** architecture

[the Section called ***--buildarch `<arch>`** — Perform Build For the
**`<arch>`**
Architecture*](ch-rpm-b-command.html#s2-rpm-b-command-buildarch-option)

**--buildos `<os>`**

Perform build for the **`<os>`** operating system

[the Section called ***--buildos `<os>`** — Perform Build For the
**`<os>`** Operating
System*](ch-rpm-b-command.html#s2-rpm-b-command-buildos-option)

**--timecheck `<secs>`**

Print a warning if files are over **`<secs>`** old

[the Section called ***--timecheck `<secs>`** — Print a warning if files
to be packaged are over **`<secs>`**
old*](ch-rpm-b-command.html#s2-rpm-b-command-timecheck-option)

General Options

Page

**-vv**

Display debugging information

[the Section called ***-vv** — Display debugging
information*](ch-rpm-b-command.html#s2-rpm-b-command-vv-option)

**--quiet**

Produce as little output as possible

[the Section called ***--quiet** — Produce as Little Output as
Possible*](ch-rpm-b-command.html#s2-rpm-b-command-quiet-option)

**--rcfile `<rcfile>`**

Set alternate `rpmrc` file to **`<rcfile>`**

[the Section called ***--rcfile `<rcfile>`** — Set alternate `rpmrc`
file to
**`<rcfile>`***](ch-rpm-b-command.html#s2-rpm-b-command-rcfile-option)

<div class="sect1">

# <span id="s1-rpm-b-command-what-it-does">**rpmbuild** — What Does it Do?</span>

When RPM is invoked with the **-b** option, the process of building a
package is started. The rest of the command will determine exactly what
is to be built and how far the build should proceed. In this chapter,
we'll explore every aspect of **rpm -b**.

An RPM build command must have two additional pieces of information,
over and above "**rpmbuild**":

1.  The names of one or more spec files representing software to be
    packaged.

2.  The desired stage at which the build is to stop.

As we discussed in [Chapter 10](ch-rpm-basics.html), the spec file is
one of the inputs to RPM's build process. It contains the information
necessary for RPM to perform the build and package the software.

There are a number of stages that RPM goes through during a build. By
specifying that the build process is to stop at a certain stage, the
package builder can monitor the build's progress, make any changes
necessary, and restart the build. Let's start by looking at the various
stages that can be specified in a build command.

<div class="sect2">

## <span id="s2-rpm-b-command-bp-option">**rpmbuild** **-bp** — Execute **%prep**</span>

The command **rpmbuild** **-bp** directs RPM to execute the very first
step in the build process. In the spec file, this step is labeled
**%prep**. Every command in the **%prep** section will be executed when
the **-bp** option is used.

Here's a simple **%prep** section from the spec file we used in [Chapter
11](ch-rpm-build.html):

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%prep
%setup

          </code></pre></td>
</tr>
</tbody>
</table>

This **%prep** section consists of a single **%setup** macro. When using
**rpm -bp** against this spec file, we can see exactly what **%setup**
does:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -bp cdplayer-1.0.spec
* Package: cdplayer
Executing(%prep):
+ umask 022
+ cd /usr/src/redhat/BUILD
+ cd /usr/src/redhat/BUILD
+ rm -rf cdplayer-1.0
+ gzip -dc /usr/src/redhat/SOURCES/cdplayer-1.0.tgz
+ tar -xvvf -
drwxrwxr-x root/users        0 Aug  4 22:30 1996 cdplayer-1.0/
-rw-r--r-- root/users    17982 Nov 10 01:10 1995 cdplayer-1.0/COPYING
-rw-r--r-- root/users      627 Nov 10 01:10 1995 cdplayer-1.0/ChangeLog
…
-rw-r--r-- root/users     2806 Nov 10 01:10 1995 cdplayer-1.0/volume.c
-rw-r--r-- root/users     1515 Nov 10 01:10 1995 cdplayer-1.0/volume.h
+ [ 0 -ne 0 ]
+ cd cdplayer-1.0
+ cd /usr/src/redhat/BUILD/cdplayer-1.0
+ chown -R root.root .
+ chmod -R a+rX,g-w,o-w .
+ exit 0

# 
          </code></pre></td>
</tr>
</tbody>
</table>

First, RPM confirms that the `cdplayer` package is the subject of this
build. Then it sets the umask and starts executing the **%prep**
section. At this point, the **%setup** macro is doing its thing. It
changes directory into the build area and removes any old copies of
`cdplayer`'s build tree.

Next, **%setup** unzips the sources and uses **tar** to create the build
tree. We've removed the complete listing of files, but be prepared to
see *lots* of output if the software being packaged is large.

Finally, **%setup** changes directory into `cdplayer`'s build tree and
changes ownership and file permissions appropriately. The exit 0
signifies the end of the **%prep** section, and therefore, the end of
the **%setup** macro. Since we used the **-bp** option, RPM stopped at
this point. Let's see what RPM left in the build area:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># cd /usr/src/redhat/BUILD
# ls -l
total 1
drwxr-xr-x   2 root     root         1024 Aug  4 22:30 cdplayer-1.0

#
          </code></pre></td>
</tr>
</tbody>
</table>

There's the top-level directory. Changing directory into `cdplayer-1.0`,
we find the sources are ready to be built:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># cd cdplayer-1.0
# ls -lF
total 216
-rw-r--r--   1 root     root        17982 Nov 10  1995 COPYING
-rw-r--r--   1 root     root          627 Nov 10  1995 ChangeLog
…
-rw-r--r--   1 root     root         2806 Nov 10  1995 volume.c
-rw-r--r--   1 root     root         1515 Nov 10  1995 volume.h

# 
          </code></pre></td>
</tr>
</tbody>
</table>

We can see that **%setup**'s **chown** and **chmod** commands did what
they were supposed to — the files are owned by root, with permissions
set appropriately.

If not stopped by the **-bp** option, the next step in RPM's build
process would be to build the software. RPM can also be stopped at the
end of the **%build** section in the spec file. This is done by using
the **-bc** option:

</div>

<div class="sect2">

## <span id="s2-rpm-b-command-bc-option">**rpmbuild** **-bc** — Execute **%prep**, **%build**</span>

When the **-bc** option is used during a build, RPM stops once the
software has been built. In terms of the spec file, every command in the
**%build** section will be executed. In the following example, we've
removed the output from the **%prep** section to cut down on the
redundant output, but keep in mind that it is executed nonetheless:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -bc cdplayer-1.0.spec
* Package: cdplayer
Executing(%prep):
…
+ exit 0
Executing(%build):
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
+ make
gcc -Wall -O2  -c -I/usr/include/ncurses  cdp.c 
gcc -Wall -O2  -c -I/usr/include/ncurses  color.c 
gcc -Wall -O2  -c -I/usr/include/ncurses  display.c 
gcc -Wall -O2  -c -I/usr/include/ncurses  misc.c 
gcc -Wall -O2  -c -I/usr/include/ncurses  volume.c 
volume.c: In function `mix_set_volume&#39;:
volume.c:67: warning: implicit declaration of function `ioctl&#39;
gcc -Wall -O2  -c -I/usr/include/ncurses  hardware.c 
gcc -Wall -O2  -c -I/usr/include/ncurses  database.c 
gcc -Wall -O2  -c -I/usr/include/ncurses  getline.c 
gcc -o cdp cdp.o color.o display.o misc.o volume.o hardware.o database.o
getline.o  -I/usr/include/ncurses  -L/usr/lib -lncurses
groff -Tascii -man cdp.1 | compress &gt;cdp.1.Z
+ exit 0

# 
          </code></pre></td>
</tr>
</tbody>
</table>

After the command, we see RPM executing the **%prep** section (which
we've removed almost entirely). Next, RPM starts executing the contents
of the **%build** section. In our example spec file, the **%build**
section looks like this:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%build
make 

          </code></pre></td>
</tr>
</tbody>
</table>

We see that prior to the **make** command, RPM changes directory into
`cdplayer`'s top-level directory. RPM then starts the **make**, which
ends with the **groff** command. At this point, the execution of the
**%build** section has been completed. Since the **-bc** option was
used, RPM stops at this point.

The next step in the build process would be to install the newly built
software. This is done in the **%install** (and **%check**) section of
the spec file. RPM can be stopped after the install has taken place by
using the **-bi** option:

</div>

<div class="sect2">

## <span id="s2-rpm-b-command-bi-option">**rpmbuild** **-bi** — Execute **%prep**, **%build**, **%install**, **%check**</span>

By using the **-bi** option, RPM is directed to stop once the software
is completely built and installed, and the test suite has been run on
the build system. Here's what the output of a build using the **-bi**
option looks like:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -bi cdplayer-1.0.spec
* Package: cdplayer
Executing(%prep):
…
+ exit 0
Executing(%build):
…
+ exit 0
Executing(%install):
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
+ make install
chmod 755 cdp
chmod 644 cdp.1.Z
cp cdp /usr/local/bin
ln -s /usr/local/bin/cdp /usr/local/bin/cdplay
cp cdp.1 /usr/local/man/man1
+ exit 0
Executing(%check):
+ umask 022
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
+ make test
All tests run successfully.
+ exit 0
Executing(%doc):
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
+ DOCDIR=//usr/doc/cdplayer-1.0-1
+ rm -rf //usr/doc/cdplayer-1.0-1
+ mkdir -p //usr/doc/cdplayer-1.0-1
+ cp -ar README //usr/doc/cdplayer-1.0-1
+ exit 0

# 
          </code></pre></td>
</tr>
</tbody>
</table>

As before, we've excised most of the previously described sections. In
this example, the **%install** section looks like:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%install
make install

          </code></pre></td>
</tr>
</tbody>
</table>

After the **%prep** and **%build** sections, the **%install** section is
executed. Looking at the output, we see that RPM changes directory into
`cdplayer`'s top-level directory and issues the **make install**
command, the sole command in the **%install** section. The output from
that point until the first exit 0, is from **make install**.

The next part of the output is from the **%check** section, ie. the sole
command **make test**.

The remaining commands are due to the contents of the spec file's
**%files** list. Here's what it looks like:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%files
%doc README
/usr/local/bin/cdp
/usr/local/bin/cdplay
/usr/local/man/man1/cdp.1

          </code></pre></td>
</tr>
</tbody>
</table>

The line responsible is **%doc README**. The **%doc** tag identifies the
file as being documentation. RPM handles documentation files by creating
a directory in `/usr/doc` and placing all documentation in it. The exit
0 at the end signifies the end of the **%install** section. RPM stops
due to the **-bi** option.

The next step at which RPM's build process can be stopped is after the
software's binary package file has been created. This is done using the
**-bb** option:

</div>

<div class="sect2">

## <span id="s2-rpm-b-command-bb-option">**rpmbuild** **-bb** — Execute **%prep**, **%build**, **%install**, **%check**, package (bin)</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -bb cdplayer-1.0.spec
* Package: cdplayer
Executing(%prep):
…
+ exit 0
Executing(%build):
…
+ exit 0
Executing(%install):
…
+ exit 0
Executing(%check):
…
+ exit 0
Executing(%doc):
…
+ exit 0
Binary Packaging: cdplayer-1.0-1
Finding dependencies...
Requires (2): libc.so.5 libncurses.so.2.0
usr/doc/cdplayer-1.0-1
usr/doc/cdplayer-1.0-1/README
usr/local/bin/cdp
usr/local/bin/cdplay
usr/local/man/man1/cdp.1
93 blocks
Generating signature: 0
Wrote: /usr/src/redhat/RPMS/i386/cdplayer-1.0-1.i386.rpm
Executing(%clean):
+ umask 022
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
+ exit 0

# 
        </code></pre></td>
</tr>
</tbody>
</table>

After executing the **%prep**, **%build**, **%install**, and **%check**
sections, and handling any special documentation files, RPM then creates
a binary package file. In the sample output, we see that first RPM
performs automatic dependency checking. It does this by determining
which shared libraries are required by the executable programs contained
in the package. Next, RPM actually archives the files to be packaged,
optionally signs the package file, and outputs the finished product.

The last part of RPM's output looks suspiciously like a section in the
spec file being executed. In our example, there is no **%clean**
section. If there were, however, RPM would have executed any commands in
the section. In the absence of a **%clean** section, RPM simply issues
the usual **cd** commands and exits normally.

</div>

<div class="sect2">

## <span id="s2-rpm-b-command-ba-option">**rpmbuild** **-ba** — Execute **%prep**, **%build**, **%install**, **%check**, package (bin, src)</span>

The **-ba** option directs RPM to perform *all* the stages in building a
package. With this one command, RPM:

  - Unpacks the original sources.

  - Applies patches (if desired).

  - Builds the software.

  - Installs the software.

  - Runs the test suite for the software.

  - Creates the binary package file.

  - Creates the source package file.

That's quite a bit of work for one command\! Here it is, in action:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba cdplayer-1.0.spec
* Package: cdplayer
Executing(%prep):
…
+ exit 0
Executing(%build):
…
+ exit 0
Executing(%install):
…
+ exit 0
Executing(%check):
…
+ exit 0
Executing(%doc):
…
+ exit 0
Binary Packaging: cdplayer-1.0-1
…
Executing(%clean):
…
+ exit 0
Source Packaging: cdplayer-1.0-1
cdplayer-1.0.spec
cdplayer-1.0.tgz
80 blocks
Generating signature: 0
Wrote: /usr/src/redhat/SRPMS/cdplayer-1.0-1.src.rpm

# 
          </code></pre></td>
</tr>
</tbody>
</table>

As in previous examples, RPM executes the **%prep**, **%build**,
**%install**, and **%check** sections, handles any special documentation
files, creates a binary package file, and cleans up after itself.

The final step in the build process is to create a source package file.
As the output shows, it consists of the spec file and the original
sources. A source package may optionally include one or more patch
files, although in our example, `cdplayer` requires none.

At the end of a build using the **-ba** option, the software has been
successfully built and packaged in both binary and source form. But
there are a few more build-time options that we can use. One of them is
the **-bl** option:

</div>

<div class="sect2">

## <span id="s2-rpm-b-command-bl-option">**rpmbuild** **-bl** — Check **%files** list</span>

There's one last letter that may be specified with **rpm -b**, but
unlike the others, which indicate the stage at which the build process
is to stop, this option performs a variety of checks on the **%files**
list in the named spec file. When **l** is added to **rpmbuild**, the
following checks are performed:

  - Expands the spec file's **%files** list and checks that each file
    listed actually exists.

  - Determines what shared libraries the software requires by examining
    every executable file listed.

  - Determines what shared libraries are provided by the package.

Why is it necessary to do all this checking? When would it be useful?
Keep in mind that the **%files** list must be generated manually. By
using the **-bl** option, the following steps are all that's necessary
to create a **%files** list:

  - Writing the **%files** list.

  - Using the **-bl** option to check the **%files** list.

  - Making any necessary changes to the **%files** list.

It may take more than one iteration through these steps, but eventually
the list check will pass. Using the **-bl** option to check the
**%files** list is certainly better than starting a two-hour package
build, only to find out at the very end that the list contains a
misspelled filename.

Here's an example of the **-bl** option in action:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -bl cdplayer-1.0.spec
* Package: cdplayer
File List Check: cdplayer-1.0-1
Finding dependencies...
Requires (2): libc.so.5 libncurses.so.2.0

#
          </code></pre></td>
</tr>
</tbody>
</table>

It's hard to see exactly what RPM is doing from the output, but if we
add **-vv**, we can get a bit more information:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -bl -vv cdplayer-1.0.spec
D: Switched to BASE package
D: Source(0) = sunsite.unc.edu:/pub/Linux/apps/sound/cds/cdplayer-1.0.tgz
D: Switching to part: 12
D: fileFile = 
D: Switched to package: (null)
D: Switching to part: 2
D: fileFile = 
D: Switching to part: 3
D: fileFile = 
D: Switching to part: 4
D: fileFile = 
D: Switching to part: 10
D: fileFile = 
D: Switched to package: (null)
* Package: cdplayer
File List Check: cdplayer-1.0-1
D: ADDING: /usr/doc/cdplayer-1.0-1
D: ADDING: /usr/doc/cdplayer-1.0-1/README
D: ADDING: /usr/local/bin/cdp
D: ADDING: /usr/local/bin/cdplay
D: ADDING: /usr/local/man/man1/cdp.1
D: md5(/usr/doc/cdplayer-1.0-1/README) = 2c149b2fb1a4d65418131a19b242601c
D: md5(/usr/local/bin/cdp) = 0f2a7a2f81812c75fd01c52f456798d6
D: md5(/usr/local/bin/cdplay) = d41d8cd98f00b204e9800998ecf8427e
D: md5(/usr/local/man/man1/cdp.1) = b32cc867ae50e2bdfa4d6780b084adfa
Finding dependencies...
D: Adding require: libncurses.so.2.0
D: Adding require: libc.so.5
Requires (2): libc.so.5 libncurses.so.2.0

# 
          </code></pre></td>
</tr>
</tbody>
</table>

Looking at this more verbose output, it's easy to see there's a great
deal going on. Some of it is not directly pertinent to checking the
**%files** list, however. For example, the output extending from the
first line, to the line reading \* Package: cdplayer, reflects
processing that takes place during actual package building, and can be
ignored.

Following that section is the actual **%files** list check. In this
section, every file named in the **%files** list is checked to make sure
it exists. The phrase, ADDING:, again reflects RPM's package building
roots. When using the **-bl** option, however, RPM is simply making sure
the files exist on the build system. If the **--timecheck** option
(described a bit later, on [the Section called ***--timecheck `<secs>`**
— Print a warning if files to be packaged are over **`<secs>`**
old*](ch-rpm-b-command.html#s2-rpm-b-command-timecheck-option)) is
present, the checks required by that option are performed here, as well.

After the list check, the MD5 checksums of each file are calculated and
displayed. While this information is vital during actual package
building, it is not used when using the **-bl** option.

Finally, RPM determines which shared libraries the listed files require.
In this case, there are only two — `libc.so.5`, and `libncurses.so.2.0`.
While not strictly a part of the list-checking process, displaying
shared library dependencies can be quite helpful at this point. It can
point out possible problems, such as assuming that the target systems
have a certain library installed when, in fact, they do not.

So far, we've only seen what happens when the **%files** list is
correct. Let's see what happens where the list has problems. In this
example, we've added a bogus file to the package's **%files** list:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -bl cdplayer-1.0.spec
* Package: cdplayer
File List Check: cdplayer-1.0-1
File not found: /usr/local/bin/bogus
Build failed.

#
          </code></pre></td>
</tr>
</tbody>
</table>

Reflecting more of its package building roots, **rpm -bl** says that the
"build failed". But the bottom line is that there is no such file as
`/usr/bin/bogus`. In this example we made the name obviously wrong, but
in a more real-world setting, the name will more likely be a misspelling
in the **%files** list. OK, let's correct the **%files** list and try
again:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -bl cdplayer-1.0.spec
* Package: cdplayer
File List Check: cdplayer-1.0-1
File not found: /usr/local/bin/cdplay
Build failed.

#
          </code></pre></td>
</tr>
</tbody>
</table>

Another error\! In this case the file is spelled correctly, but it is
not on the build system, even though it should be. Perhaps it was
deleted accidentally. In any case, let's rebuild the software and try
again:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -bi cdplayer-1.0.spec
* Package: cdplayer
Executing(%prep):
…
+ exit 0
Executing(%build):
…
+ exit 0
Executing(%install):
…
ln -s /usr/local/bin/cdp /usr/local/bin/cdplay
…
+ exit 0
Executing(%check):
…
+ exit 0
Executing(%doc):
…
+ exit 0

# 
# rpmbuild -bl cdplayer-1.0.spec
* Package: cdplayer
File List Check: cdplayer-1.0-1
Finding dependencies...
Requires (2): libc.so.5 libncurses.so.2.0

#
          </code></pre></td>
</tr>
</tbody>
</table>

Done\! The moral to this story is that using **rpm -bl** and fixing the
error it flagged doesn't necessarily mean your **%files** list is ready
for prime-time: Always run it again to make sure\!

</div>

<div class="sect2">

## <span id="s2-rpm-b-command-short-circuit-option">**--short-circuit** — Force build to start at particular stage</span>

Although it sounds dangerous, the **--short-circuit** option can be your
friend. This option is used during the initial development of a package.
Earlier in the chapter, we explored stopping RPM's build process at
different stages. Using **--short-circuit**, we can *start* the build
process at different stages.

One time that **--short-circuit** comes in handy is when you're trying
to get software to build properly. Just think what it would be like —
you're hacking away at the sources, trying a build, getting an error,
and hacking some more to fix that error. Without **--short-circuit**,
you'd have to:

1.  Make your change to the sources.

2.  Use **tar** to create a new source archive.

3.  Start a build with something like **rpmbuild** **-bc**.

4.  See another bug.

5.  Go back to step 1.

Pretty cumbersome\! Since RPM's build process is designed to start with
the sources in their original **tar** file, unless your modifications
end up in that **tar** file, they won't be used in the next build.
[<span class="footnote">\[1\]</span>](#FTN.AEN6552)

But there's another way. Just follow these steps:

1.  Place the original source **tar** file in RPM's `SOURCES` directory.

2.  Create a partial spec file in RPM's `SPECS` directory (Be sure to
    include a valid **Source** line).

3.  Issue an **rpmbuild** **-bp** to properly create the build
    environment.

Now use **--short-circuit** to attempt a compile. Here's an example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -bc --short-circuit cdplayer-1.0.spec
* Package: cdplayer
Executing(%build):
+ umask 022
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
+ make
gcc -Wall -O2  -c -I/usr/include/ncurses  cdp.c 
gcc -Wall -O2  -c -I/usr/include/ncurses  color.c 
gcc -Wall -O2  -c -I/usr/include/ncurses  display.c 
gcc -Wall -O2  -c -I/usr/include/ncurses  misc.c 
gcc -Wall -O2  -c -I/usr/include/ncurses  volume.c 
volume.c: In function `mix_set_volume&#39;:
volume.c:67: warning: implicit declaration of function `ioctl&#39;
gcc -Wall -O2  -c -I/usr/include/ncurses  hardware.c 
gcc -Wall -O2  -c -I/usr/include/ncurses  database.c 
gcc -Wall -O2  -c -I/usr/include/ncurses  getline.c 
gcc -o cdp cdp.o color.o display.o misc.o volume.o
     hardware.o database.o getline.o  -I/usr/include/ncurses
     -L/usr/lib -lncurses
groff -Tascii -man cdp.1 | compress &gt;cdp.1.Z
+ exit 0

# 
          </code></pre></td>
</tr>
</tbody>
</table>

Normally, the **-bc** option instructs RPM to *stop* the build after the
**%build** section of the spec file has been executed. By adding
**--short-circuit**, however, RPM *starts* the build by executing the
**%build** section and stops when everything in **%build** has been
executed.

There is only one other build stage that can be **--short-circuit**'ed,
and that is the install stage. The reason for this restriction is to
make it difficult to bypass RPM's use of pristine sources. If it were
possible to **--short-circuit** to **-bb** or **-ba**, a package builder
might take the "easy" way out and simply hack at the build tree until
the software built successfully, then package the hacked sources. So,
RPM will only **--short-circuit** to **-bc** or **-bi**. Nothing else
will do.

What exactly does an **rpmbuild** **-bi --short-circuit** do, anyway?
Like an **rpmbuild** **-bc --short-circuit**, it starts executing at the
named stage, which in this case is **%install**. Note that the build
environment must be ready to perform an install before attempting to
**--short-circuit** to the **%install** stage. If the software installs
via **make install**, **make** will automatically compile the software
anyway.

And what happens if the build environment isn't ready and a
**--short-circuit** is attempted? Let's see:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -bi --short-circuit cdplayer-1.0.spec
* Package: cdplayer
Executing(%install):
+ umask 022
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
/var/tmp/rpmbu01157aaa: cdplayer-1.0: No such file or directory
Bad exit status

# 
          </code></pre></td>
</tr>
</tbody>
</table>

RPM blindly started executing the **%install** stage, but came to an
abrupt halt when it attempted to change directory into `cdplayer-1.0`,
which didn't exist. After giving a descriptive error message, RPM exited
with a failure status. Except for some minor differences, **rpmbuild**
**-bc** would have failed in the same way.

</div>

<div class="sect2">

## <span id="s2-rpm-b-command-buildarch-option">**--buildarch `<arch>`** — Perform Build For the **`<arch>`** Architecture</span>

The **--buildarch** option is used to override RPM's architecture
detection logic. The option is followed by the desired architecture
name. Here's an example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba --buildarch i486 cdplayer-1.0.spec
* Package: cdplayer
…
Binary Packaging: cdplayer-1.0-1
…
Wrote: /usr/src/redhat/RPMS/i486/cdplayer-1.0-1.i486.rpm
…
Wrote: /usr/src/redhat/SRPMS/cdplayer-1.0-1.src.rpm

#
          </code></pre></td>
</tr>
</tbody>
</table>

We've removed most of RPM's output from this example, but the main thing
we can see from this example is that the package was built for the
`i486` architecture, due to the inclusion of the **--buildarch** option
on the command line. We can also see that RPM wrote the binary package
in the architecture-specific directory, `/usr/src/redhat/RPMS/i486`.
Using RPM's **--queryformat** option confirms the package's
architecture:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmquery -qp --queryformat &#39;%{arch}\n&#39; /usr/src/redhat/RPMS/i486/cdplayer-1.0-1.i486.rpm
i486

#
          </code></pre></td>
</tr>
</tbody>
</table>

For more information on build packages for multiple architectures,
please see [Chapter 19](ch-rpm-multi.html).

</div>

<div class="sect2">

## <span id="s2-rpm-b-command-buildos-option">**--buildos `<os>`** — Perform Build For the **`<os>`** Operating System</span>

The **--buildos** option is used to override RPM's operating system
detection logic. The option is followed by the desired operating system
name. Here's an example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba --buildos osf1 cdplayer-1.0.spec
…
Binary Packaging: cdplayer-1.0-1
…
Wrote: /usr/src/redhat/RPMS/i386/cdplayer-1.0-1.i386.rpm
Source Packaging: cdplayer-1.0-1
…
Wrote: /usr/src/redhat/SRPMS/cdplayer-1.0-1.src.rpm

#
          </code></pre></td>
</tr>
</tbody>
</table>

There's nothing in the build output that explicitly states the build
operating system as been set to **osf1**. Let's see if **--queryformat**
will tell us:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmquery -qp --queryformat &#39;%{os}\n&#39; /usr/src/redhat/RPMS/i386/cdplayer-1.0-1.i386.rpm
osf1

#
          </code></pre></td>
</tr>
</tbody>
</table>

The package was indeed built for the specified operating system. For
more information on building packages for multiple operating systems,
please see [Chapter 19](ch-rpm-multi.html).

</div>

<div class="sect2">

## <span id="s2-rpm-b-command-sign-option">**--sign** — Add a Digital Signature to the Package</span>

The **--sign** option directs RPM to add a digital signature to the
package being built. Currently, this is done using PGP. Here's an
example of **--sign** in action:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba --sign cdplayer-1.0.spec
Enter pass phrase: passphrase (not echoed)
Pass phrase is good.
* Package: cdplayer
…
Binary Packaging: cdplayer-1.0-1
…
Generating signature: 1002
Wrote: /usr/src/redhat/RPMS/i386/cdplayer-1.0-1.i386.rpm
…
Source Packaging: cdplayer-1.0-1
…
Generating signature: 1002
Wrote: /usr/src/redhat/SRPMS/cdplayer-1.0-1.src.rpm

#
          </code></pre></td>
</tr>
</tbody>
</table>

The most obvious effect of adding the **--sign** option to a build
command is that RPM then asks for your private key's passphrase. After
entering the passphrase (which isn't echoed), the build proceeds as
usual. The only other difference between this and a non-signed build is
that the Generating signature: lines have a non-zero value.

Let's check the source and binary packages we've just created and see if
they are, in fact, signed:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmsign --checksig /usr/src/redhat/SRPMS/cdplayer-1.0-1.src.rpm
/usr/src/redhat/SRPMS/cdplayer-1.0-1.src.rpm: size pgp md5 OK

# rpmsign --checksig /usr/src/redhat/RPMS/i386/cdplayer-1.0-1.i386.rpm
/usr/src/redhat/RPMS/i386/cdplayer-1.0-1.i386.rpm: size pgp md5 OK

#
          </code></pre></td>
</tr>
</tbody>
</table>

The fact that there is a **pgp** in **--checksig**'s output indicates
that the packages have been signed.

For more information on signing packages, please see [Chapter
17](ch-rpm-pgp.html). Also, [Appendix G](ch-pgp-intro.html) contains
information on obtaining and installing PGP.

</div>

<div class="sect2">

## <span id="s2-rpm-b-command-test-option">**--test** — Create, Save Build Scripts For Review</span>

There are times when it might be necessary to get a more in-depth view
of a particular build. By using the **--test** option, it's easy. When
**--test** is added to a build command, the scripts RPM would normally
use to actually perform the build, are created and saved for you to
review. Let's see how it works:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba --test cdplayer-1.0.spec
* Package: cdplayer

#
          </code></pre></td>
</tr>
</tbody>
</table>

Unlike a normal build, there's not much output. But the **--test**
option has caused a set of scripts to be written and saved for you. The
question is: Where are they?

If you are using a customized `rpmrc` file, the scripts will be written
to the directory specified by the `rpmrc` entry **tmppath**. If you
haven't changed this setting, RPM, by default, writes the scripts in
`/var/tmp`. Here they are:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -l /var/tmp
total 4
-rw-rw-r--   1 root     root          670 Sep 17 20:35 rpmbu00236aaa
-rw-rw-r--   1 root     root          449 Sep 17 20:35 rpmbu00236baa
-rw-rw-r--   1 root     root          482 Sep 17 20:35 rpmbu00236caa
-rw-rw-r--   1 root     root          552 Sep 17 20:35 rpmbu00236daa

# 
          </code></pre></td>
</tr>
</tbody>
</table>

Each file contains a script that performs a given part of the build.
Here's the first file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#!/bin/sh -e
# Script generated by rpm

RPM_SOURCE_DIR=&quot;/usr/src/redhat/SOURCES&quot;
RPM_BUILD_DIR=&quot;/usr/src/redhat/BUILD&quot;
RPM_DOC_DIR=&quot;/usr/doc&quot;
RPM_OPT_FLAGS=&quot;-O2 -m486 -fno-strength-reduce&quot;
RPM_ARCH=&quot;i386&quot;
RPM_OS=&quot;Linux&quot;
RPM_ROOT_DIR=&quot;/tmp/cdplayer&quot;
RPM_BUILD_ROOT=&quot;/tmp/cdplayer&quot;
RPM_PACKAGE_NAME=&quot;cdplayer&quot;
RPM_PACKAGE_VERSION=&quot;1.0&quot;
RPM_PACKAGE_RELEASE=&quot;1&quot;
set -x

umask 022

echo Executing(%prep)
cd /usr/src/redhat/BUILD

cd /usr/src/redhat/BUILD
rm -rf cdplayer-1.0
gzip -dc /usr/src/redhat/SOURCES/cdplayer-1.0.tgz | tar -xvvf -
if [ $? -ne 0 ]; then
  exit $?
fi
cd cdplayer-1.0
cd /usr/src/redhat/BUILD/cdplayer-1.0
chown -R root.root .
chmod -R a+rX,g-w,o-w .

          </code></pre></td>
</tr>
</tbody>
</table>

As we can see, this script contains the **%prep** section from the spec
file. The script starts off by defining a number of environment
variables and then leads into the **%prep** section. In the spec file
used in this build, the **%prep** section consists of a single
**%setup** macro. In this file, we can see exactly how RPM expands that
macro. The remaining files follow the same basic layout — a section
defining environment variables, followed by the commands to be executed.

Note that the **--test** option will only create script files for each
build stage, as specified in the command line. For example, if the above
command was changed to:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -bp --test cdplayer-1.0.spec
#
          </code></pre></td>
</tr>
</tbody>
</table>

only one script file, containing the **%prep** commands, would be
written. In any case, no matter what RPM build command is used, the
**--test** option can let you see exactly what is going to happen during
a build.

</div>

<div class="sect2">

## <span id="s2-rpm-b-command-clean-option">**--clean** — Clean up after build</span>

The **--clean** option can be used to ensure that the package's build
directory tree is removed at the end of a build. Although it can be used
with any build stage, it doesn't always make much sense to do so:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -bp --clean cdplayer-1.0.spec
* Package: cdplayer
Executing(%prep):
…
+ exit 0
Executing(--clean):
+ cd /usr/src/redhat/BUILD
+ rm -rf cdplayer-1.0
+ exit 0

# 
          </code></pre></td>
</tr>
</tbody>
</table>

In this example, we see a typical **%prep** section being executed. The
line "Executing(--clean):" indicates the start of the **--clean**'s
activity. After changing directory into the build directory, RPM then
issues a recursive delete on the package's top-level directory.

As we noted above, this particular example doesn't make much sense.
We're only executing the **%prep** section, which creates the package's
build tree, and using **--clean**, which removes it\! Using **--clean**
with the **-bc** option isn't very productive either, as the newly built
software remains in the build tree. Once again, there would be no
remnants left after **--clean** has done its thing.

Normally, the **--clean** option is used once the software builds and
can be packaged successfully. It is particularly useful when more than
one package is to be built, since **--clean** ensures that the
filesystem holding the build area will not fill up with build trees from
each package.

Note also that the **--clean** option only removes the files that reside
in the software's build tree. If there are any files that the build
creates outside of this hierarchy, it will be necessary to write a
script for the spec file's **%clean** section.

</div>

<div class="sect2">

## <span id="s2-rpm-b-command-buildroot-option">**--buildroot `<path>`** — Execute **%install** using **`<path>`** as the root</span>

The **--buildroot** option can make two difficult situations much
easier:

  - Performing a build without impacting the build system.

  - Allowing non-root users to build packages.

Let's study the first situation in a bit more detail. Say, for example,
that `sendmail` is to be packaged. In the course of creating a
`sendmail` package, the software must be installed. This would mean that
critical `sendmail` files, such as `sendmail.cf` and `aliases`, would be
overwritten. Mail handling on the build system would almost certainly be
disrupted.

In the second case, it's certainly possible to set permissions such that
non-root users can install software, but highly unlikely that any system
administrator worth their salt would do so. What can be done to make
these situations more tenable?

The **--buildroot** option is used to instruct RPM to use a directory
other than `/` as a "build root". This phrase is a bit misleading, in
that the build root is *not* the root directory under which the software
is built. Rather, it is the root directory for the install phase of the
build. When a build root is not specified, the software being packaged
is installed relative to the build system's root directory "`/`".

However, it's not enough to just specify a build root on the command
line. The spec file for the package must be set up to support a build
root. If you don't make the necessary changes, this is what you'll see:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba --buildroot /tmp/foo cdplayer-1.0.spec
Package can not do build prefixes
Build failed.

#
          </code></pre></td>
</tr>
</tbody>
</table>

[Chapter 16](ch-rpm-anywhere.html) has complete instructions on the
modifications necessary to configure a package to use an alternate build
root, as well as methods to permit users to build packages without root
access. Assuming that the necessary modifications have been made, here
is what the build would look like:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba --buildroot /tmp/foonly cdplayer-1.0.spec
* Package: cdplayer
Executing(%prep):
+ cd /usr/src/redhat/BUILD
…
+ exit 0
Executing(%build):
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
…
+ exit 0
Executing(%install):
+ umask 022
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
+ make ROOT=/tmp/foonly install
install -m 755 -o 0 -g 0 -d /tmp/foonly/usr/local/bin/
install -m 755 -o 0 -g 0 cdp /tmp/foonly/usr/local/bin/cdp
rm -f /tmp/foonly/usr/local/bin/cdplay
ln -s /tmp/foonly/usr/local/bin/cdp /tmp/foonly/usr/local/bin/cdplay
install -m 755 -o 0 -g 0 -d /tmp/foonly/usr/local/man/man1/
install -m 755 -o 0 -g 0 cdp.1 /tmp/foonly/usr/local/man/man1/cdp.1
+ exit 0
Executing(%check):
+ umask 022
…
+ exit 0
Executing(%doc):
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
+ DOCDIR=/tmp/foonly//usr/doc/cdplayer-1.0-1
+ rm -rf /tmp/foonly//usr/doc/cdplayer-1.0-1
+ mkdir -p /tmp/foonly//usr/doc/cdplayer-1.0-1
+ cp -ar README /tmp/foonly//usr/doc/cdplayer-1.0-1
+ exit 0
Binary Packaging: cdplayer-1.0-1
Finding dependencies...
Requires (2): libc.so.5 libncurses.so.2.0
usr/doc/cdplayer-1.0-1
usr/doc/cdplayer-1.0-1/README
usr/local/bin/cdp
usr/local/bin/cdplay
usr/local/man/man1/cdp.1
93 blocks
Generating signature: 0
Wrote: /usr/src/redhat/RPMS/i386/cdplayer-1.0-1.i386.rpm
Executing(%clean):
+ umask 022
+ cd /usr/src/redhat/BUILD
+ cd cdplayer-1.0
+ exit 0
Source Packaging: cdplayer-1.0-1
cdplayer-1.0.spec
cdplayer-1.0.tgz
82 blocks
Generating signature: 0
Wrote: /usr/src/redhat/SRPMS/cdplayer-1.0-1.src.rpm

# 
          </code></pre></td>
</tr>
</tbody>
</table>

As the somewhat edited output shows, the **%prep**, **%build**, and
**%install** sections are executed in RPM's normal build directory.
However, the **--buildroot** option comes into play when the **make
install** is done. As we can see, the `ROOT` variable is set to
`/tmp/foonly`, which was the value following **--buildroot** on the
command line. From that point on, we can see that **make** substituted
the new build root value during the install phase.

The build root is also used when documentation files are installed. The
documentation directory `cdplayer-1.0-1` is created in
`/tmp/foonly/usr/doc`, and the `README` file is placed in it.

The only remaining difference that results from using **--buildroot**,
is that the files to be included in the binary package are not located
relative to the build system's root directory. Instead they are located
relative to the build root `/tmp/foonly`. The resulting binary and
source package files are functionally equivalent to packages built
without the use of **--buildroot**.

<div class="sect3">

### <span id="s3-rpm-b-command-buildroot-warning">Using **--buildroot** Can Bite You\!</span>

Although the **--buildroot** option can solve some problems, using a
build root can actually be dangerous. How? Consider the following
situation:

  - A spec file is configured to have a build root of `/tmp/blather`,
    for instance.

  - In the **%prep** section
    [<span class="footnote">\[2\]</span>](#FTN.AEN6863) , there is an
    **rm -rf $RPM\_BUILD\_ROOT** command to clean out any old installed
    software.

  - You decide to build the software so that it installs relative to
    your system's root directory, so you enter the following command:
    "**rpmbuild** **-ba --buildroot / foo.spec**".

The end result? Since specifying "`/`" as the build root sets
`$RPM_BUILD_ROOT` to "`/`", that innocuous little **rm -rf
`$RPM_BUILD_ROOT`** turns into **rm -rf /**\! A recursive delete,
starting at your system's root directory, might not be a total disaster
if you catch it quickly, but in either case, you'll be testing your
ability to restore from backup… Er, you *do* have backups, don't you?

The moral of this story is to be *very* careful when using
**--buildroot**. A good rule of thumb is to always specify a unique
build root. For example, instead of specifying `/tmp` as a build root
(and possibly losing your system's directory for holding temporary
files), use the path `/tmp/mypackage`, where the directory `mypackage`
is used only by the package you're building.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-b-command-timecheck-option">**--timecheck `<secs>`** — Print a warning if files to be packaged are over **`<secs>`** old</span>

While it's possible to detect many errors in the **%files** list using
**rpmbuild** **-bl**, there is another type of problem that can't be
detected. Consider the following scenario:

  - A package you're building creates the file `/usr/bin/foo`.

  - Because of a problem with the package's makefile, `foo` is never
    copied into `/usr/bin`.

  - An older, incompatible version of `foo`, created several months ago,
    already exists in `/usr/bin`.

  - RPM creates the binary package file.

Is the incompatible `/usr/bin/foo` included in the package? You bet it
is\! If only there was some way for RPM to catch this type of problem…

Well, there is\! By adding **--timecheck**, followed by a number, RPM
will check each file being packaged, to see if the file is more than the
specified number of seconds old. If it is, a warning message is
displayed. The **--timecheck** option works with either the **-ba** or
**-bl** options. Here's an example using **-bl**:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -bl --timecheck 3600 cdplayer-1.0.spec
* Package: cdplayer
File List Check: cdplayer-1.0-1
warning: TIMECHECK failure: /usr/doc/cdplayer-1.0-1/README
Finding dependencies...
Requires (2): libc.so.5 libncurses.so.2.0

# 
          </code></pre></td>
</tr>
</tbody>
</table>

In this example, the file `/usr/doc/cdplayer-1.0-1/README` is more than
3,600 seconds, or one hour, old. If we take a look at the file, we find
that it is: [<span class="footnote">\[3\]</span>](#FTN.AEN6928)

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -al /usr/doc/cdplayer-1.0-1/README
-rw-r--r--   1 root     root         1085 Nov 10  1995 README

#
          </code></pre></td>
</tr>
</tbody>
</table>

In this particular case, the warning from **--timecheck** is no cause
for alarm. Since the `README` file was simply copied from the original
source, which was created November 10th, 1995, its date is unchanged. If
the file had been an executable or a library that was supposedly built
recently, **--timecheck**'s warning should be taken more seriously.

If you'd like to set a default time check value of one hour, you can
include the following line in your `rpmrc` file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>timecheck: 3600

          </code></pre></td>
</tr>
</tbody>
</table>

This value can still be overridden by a value on the command line, if
desired. For more information on the use of `rpmrc` files, see [Appendix
B](ch-rpmrc-file.html).

</div>

<div class="sect2">

## <span id="s2-rpm-b-command-vv-option">**-vv** — Display debugging information</span>

Unlike most other RPM commands, there is no **-v** option for
**rpmbuild**. That's because the command's default is to be verbose.
However, even more information can be obtained by adding **-vv**. Here's
an example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -bp -vv cdplayer-1.0.spec
D: Switched to BASE package
D: Source(0) = sunsite.unc.edu:/pub/Linux/apps/sound/cds/cdplayer-1.0.tgz
D: Switching to part: 12
D: fileFile = 
D: Switched to package: (null)
D: Switching to part: 2
D: fileFile = 
D: Switching to part: 3
D: fileFile = 
D: Switching to part: 4
D: fileFile = 
D: Switching to part: 10
D: fileFile = 
D: Switched to package: (null)
* Package: cdplayer
D: RUNNING: %prep
Executing(%prep):
+ umask 022
+ cd /usr/src/redhat/BUILD
+ cd /usr/src/redhat/BUILD
+ rm -rf cdplayer-1.0
+ gzip -dc /usr/src/redhat/SOURCES/cdplayer-1.0.tgz
+ tar -xvvf -
drwxrwxr-x root/users        0 Aug  4 22:30 1996 cdplayer-1.0/
-rw-r--r-- root/users    17982 Nov 10 01:10 1995 cdplayer-1.0/COPYING
…
-rw-r--r-- root/users     1515 Nov 10 01:10 1995 cdplayer-1.0/volume.h
+ [ 0 -ne 0 ]
+ cd cdplayer-1.0
+ cd /usr/src/redhat/BUILD/cdplayer-1.0
+ chown -R root.root .
+ chmod -R a+rX,g-w,o-w .
+ exit 0

# 
          </code></pre></td>
</tr>
</tbody>
</table>

Most of the output generated by the **-vv** option is preceded by a D:.
In this example, the additional output represents RPM's internal
processing during the start of the build process. Using the **-vv**
option with other build commands will produce different output.

</div>

<div class="sect2">

## <span id="s2-rpm-b-command-quiet-option">**--quiet** — Produce as Little Output as Possible</span>

As we mentioned above, the build command is normally verbose. The
**--quiet** option can be used to cut down on the command's output:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba --quiet cdplayer-1.0.spec
* Package: cdplayer
volume.c: In function `mix_set_volume&#39;:
volume.c:67: warning: implicit declaration of function `ioctl&#39;
90 blocks
82 blocks

#
          </code></pre></td>
</tr>
</tbody>
</table>

This is the entire output from a package build of `cdplayer`. Note that
warning messages (actually, anything sent to stdout) are still printed.

</div>

<div class="sect2">

## <span id="s2-rpm-b-command-rcfile-option">**--rcfile `<rcfile>`** — Set alternate `rpmrc` file to **`<rcfile>`**</span>

The **--rcfile** option is used to specify a file containing default
settings for RPM. Normally, this option is not needed. By default, RPM
uses `/etc/rpmrc` and a file named `.rpmrc` located in your login
directory.

This option would be used if there was a need to switch between several
sets of RPM defaults. Software developers and package builders will
normally be the only people using the **--rcfile** option. For more
information on `rpmrc` files, see [Appendix B](ch-rpmrc-file.html).

</div>

</div>

### Notes

|                                                                      |                                                                                                                                                                                                                                                                                               |
| -------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](ch-rpm-b-command.html#AEN6552) | As we mentioned in [Chapter 10](ch-rpm-basics.html), if the original sources need to be modified, the modifications should be kept as a separate set of patches. However, during development, it makes more sense to not generate patches every time a change to the original source is made. |
| [<span class="footnote">\[2\]</span>](ch-rpm-b-command.html#AEN6863) | Or the **%clean** section, it doesn't matter — the end result is the same.                                                                                                                                                                                                                    |
| [<span class="footnote">\[3\]</span>](ch-rpm-b-command.html#AEN6928) | It should be noted that the package was built *substantially* later than November of 1995\!                                                                                                                                                                                                   |

<div class="NAVFOOTER">

-----

|                                                |                    |                                                    |
| :--------------------------------------------- | :----------------: | -------------------------------------------------: |
| [Prev](s1-rpm-build-when-things-go-wrong.html) | [Home](index.html) | [Next](s1-rpm-b-command-other-build-commands.html) |
| When Things Go Wrong                           |  [Up](p5206.html)  |                       Other Build-related Commands |

</div>
