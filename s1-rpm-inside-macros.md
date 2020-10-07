<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-inside-scripts.html)

Chapter 13. Inside the Spec File

[Next](s1-rpm-inside-files-list.html)

-----

<div class="sect1">

# <span id="s1-rpm-inside-macros">Macros: Helpful Shorthand for Package Builders</span>

RPM does not support macros in the sense of ad-hoc sequences of commands
being defined as a macro and executed by simply referring to the macro
name.

However, there are two parts of RPM's build process that are fairly
constant from one package to another, and they are the unpacking and
patching of sources. Because of this, RPM makes two macros available to
simplify these tasks:

1.  The **%setup** macro, which is used to unpack the original sources.

2.  The **%patch** macro, which is used to apply patches to the original
    sources.

These macros are used exclusively in the **%prep** script; it wouldn't
make sense to use them anywhere else. The use of these macros is not
mandatory — It is certainly possible to write a **%prep** script without
them. But in the vast majority of cases they make life easier for the
package builder.

<div class="sect2">

## <span id="s2-rpm-inside-setup-macro">The **%setup** Macro</span>

As we mentioned above, the **%setup** macro is used to unpack the
original sources, in preparation for the build. In its simplest form,
the macro is used with no options and gets the name of the source
archive from the **source** tag specified earlier in the spec file.
Let's look at an example. The `cdplayer` package has the following
**source** tag:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Source: ftp://ftp.gnomovision.com/pub/cdplayer/cdplayer-1.0.tgz

          </code></pre></td>
</tr>
</tbody>
</table>

and the following **%prep** script:

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

In this simple case, the **%setup** macro expands into the following
commands:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>cd /usr/src/redhat/BUILD
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

As we can see, the **%setup** macro starts by changing directory into
RPM's build area and removing any `cdplayer` build trees from previous
builds. It then uses **gzip** to uncompress the original source (whose
name was taken from the **source** tag), and pipes the result to **tar**
for unpacking. The return status of the unpacking is tested. If
successful, the macro continues.

At this point, the original sources have been unpacked. The **%setup**
macro continues by changing directory into `cdplayer`'s top-level
directory. The two **cd** commands are an artifact of **%setup**'s macro
expansion. Finally, **%setup** makes sure every file in the build tree
is owned by root and has appropriate permissions set.

But that's just the simplest way that **%setup** can be used. There are
a number of other options that can be added to accommodate different
situations. Let's look at them.

<div class="sect3">

### <span id="s3-rpm-inside-setup-n-option">**-n `<name>`** — Set Name of Build Directory</span>

In our example above, the **%setup** macro simply uncompressed and
unpacked the sources. In this case, the **tar** file containing the
original sources was created such that the top-level directory was
included in the tar file. The name of the top-level directory was also
identical to that of the **tar** file, which was in
**`<name>`-`<version>`** format.

However, this is not always the case. Quite often, the original sources
unpack into a directory whose name is different than the original
**tar** file. Since RPM assumes the directory will be called
**`<name>`-`<version>`**, when the directory is called something else,
it's necessary to use **%setup**'s **-n** option. Here's an example:

Assume, for a moment, that the `cdplayer` sources, when unpacked, create
a top-level directory named `cd-player`. In this case, our **%setup**
line would look like this:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%setup -n cd-player

            </code></pre></td>
</tr>
</tbody>
</table>

and the resulting commands would look like this:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>cd /usr/src/redhat/BUILD
rm -rf cd-player
gzip -dc /usr/src/redhat/SOURCES/cdplayer-1.0.tgz | tar -xvvf -
if [ $? -ne 0 ]; then
  exit $?
fi
cd cd-player
cd /usr/src/redhat/BUILD/cd-player
chown -R root.root .
chmod -R a+rX,g-w,o-w .

            </code></pre></td>
</tr>
</tbody>
</table>

The results are identical to using **%setup** with no options, except
for the fact that **%setup** now does a recursive delete on the
directory `cd-player` (instead of `cdplayer-1.0`), and changes directory
into `cd-player` (instead of `cdplayer-1.0`).

Note that all subsequent build-time scripts will change directory into
the directory specified by the **-n** option. This makes **-n**
unsuitable as a means of unpacking sources in directories other than the
top-level build directory. In the upcoming example on [the Section
called *Using **%setup** in a Multi-source Spec
File*](s1-rpm-inside-macros.html#s3-rpm-inside-setup-multi-source),
we'll show a way around this restriction.

A quick word of warning: If the name specified with the **-n** option
doesn't match the name of the directory created when the sources are
unpacked, the build will stop pretty quickly, so it pays to be careful
when using this option.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-setup-c-option">**-c** — Create Directory (and change to it) Before Unpacking</span>

How many times have you grabbed a **tar** file and unpacked it, only to
find that it splattered files all over your current directory? Sometimes
source archives are created without a top-level directory.

As you can see from the examples so far, **%setup** expects the archive
to create its own top-level directory. If this isn't the case, you'll
need to use the **-c** option.

This option simply creates the directory and changes directory into it
before unpacking the sources. Here's what it looks like:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>cd /usr/src/redhat/BUILD
rm -rf cdplayer-1.0
mkdir -p cdplayer-1.0
cd cdplayer-1.0
gzip -dc /usr/src/redhat/SOURCES/cdplayer-1.0.tgz | tar -xvvf -
if [ $? -ne 0 ]; then
  exit $?
fi
cd /usr/src/redhat/BUILD/cdplayer-1.0
chown -R root.root .
chmod -R a+rX,g-w,o-w .

            </code></pre></td>
</tr>
</tbody>
</table>

The only changes from using **%setup** with no options, are the
**mkdir** and **cd** commands, prior to the commands that unpack the
sources. Note that you can use the **-n** option along with **-c**, so
something like **%setup -c -n blather** works as expected.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-setup-d-option">**-D** — Do Not Delete Directory Before Unpacking Sources</span>

The **-D** option keeps the **%setup** macro from deleting the
software's top-level directory. This option is handy when the sources
being unpacked are to be added to an already-existing directory tree.
This would be the case when more than one **%setup** macro is used.
Here's what **%setup** does when the **-D** option is employed:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>cd /usr/src/redhat/BUILD
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

As advertised, the **rm** prior to the **tar** command is gone.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-setup-t-option">**-T** — Do Not Perform Default Archive Unpacking</span>

The **-T** option disables **%setup**'s normal unpacking of the archive
file specified on the **source0** line. Here's what the resulting
commands look like:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>cd /usr/src/redhat/BUILD
rm -rf cdplayer-1.0
cd cdplayer-1.0
cd /usr/src/redhat/BUILD/cdplayer-1.0
chown -R root.root .
chmod -R a+rX,g-w,o-w .

            </code></pre></td>
</tr>
</tbody>
</table>

Doesn't make much sense, does it? There's a method to this madness.
We'll see the **-T** in action in the next section.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-setup-b-option">**-b `<n>`** — Unpack The *n*th Sources Before Changing Directory</span>

The **-b** option is used in conjunction with the **source** tag.
Specifically, it is used to identify which of the numbered **source**
tags in the spec file are to be unpacked.

The **-b** option requires a numeric argument matching an existing
**source** tag. If a numeric argument is not provided, the build will
fail:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba cdplayer-1.0.spec
* Package: cdplayer
Need arg to %setup -b
Build failed.
#
            </code></pre></td>
</tr>
</tbody>
</table>

Remembering that the first **source** tag is implicitly numbered 0,
let's see what happens when the **%setup** line is changed to **%setup
-b 0**:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>cd /usr/src/redhat/BUILD
rm -rf cdplayer-1.0
gzip -dc /usr/src/redhat/SOURCES/cdplayer-1.0.tgz | tar -xvvf -
if [ $? -ne 0 ]; then
  exit $?
fi
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

That's strange. The sources were unpacked twice. It doesn't make sense,
until you realize that this is why there is a **-T** option. Since
**-T** disables the default source file unpacking, and **-b** selects a
particular source file to be unpacked, the two are meant to go together,
like this:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%setup -T -b 0

            </code></pre></td>
</tr>
</tbody>
</table>

Looking at the resulting commands, we find:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>cd /usr/src/redhat/BUILD
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

That's more like it\! Let's go on to the next option.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-setup-a-option">**-a `<n>`** — Unpack The *n*th Sources After Changing Directory</span>

The **-a** option works similarly to the **-b** option, except that the
sources are unpacked *after* changing directory into the top-level build
directory. Like the **-b** option, **-a** requires **-T** in order to
prevent two sets of unpacking commands. Here are the commands that a
**%setup -T -a 0** line would produce:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>cd /usr/src/redhat/BUILD
rm -rf cdplayer-1.0
cd cdplayer-1.0
gzip -dc /usr/src/redhat/SOURCES/cdplayer-1.0.tgz | tar -xvvf -
if [ $? -ne 0 ]; then
  exit $?
fi
cd /usr/src/redhat/BUILD/cdplayer-1.0
chown -R root.root .
chmod -R a+rX,g-w,o-w .

            </code></pre></td>
</tr>
</tbody>
</table>

Note that there is no **mkdir** command to create the top-level
directory prior to issuing a **cd** into it. In our example, adding the
**-c** option will make things right:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>cd /usr/src/redhat/BUILD
rm -rf cdplayer-1.0
mkdir -p cdplayer-1.0
cd cdplayer-1.0
gzip -dc /usr/src/redhat/SOURCES/cdplayer-1.0.tgz | tar -xvvf -
if [ $? -ne 0 ]; then
  exit $?
fi
cd /usr/src/redhat/BUILD/cdplayer-1.0
chown -R root.root .
chmod -R a+rX,g-w,o-w .

            </code></pre></td>
</tr>
</tbody>
</table>

The result is the proper sequence of commands for unpacking a **tar**
file with no top-level directory.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-setup-multi-source">Using **%setup** in a Multi-source Spec File</span>

If all these interrelated options seem like overkill for unpacking a
single source file, you're right. The real reason for the various
options is to make it easier to combine several separate source archives
into a single, build-able entity. Let's see how they work in that type
of environment.

For the purposes of this example, our spec file will have the following
three **source** tags:
[<span class="footnote">\[1\]</span>](#FTN.AEN8439)

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>source: source-zero.tar.gz
source1: source-one.tar.gz
source2: source-two.tar.gz

            </code></pre></td>
</tr>
</tbody>
</table>

To unpack the first source is not hard; all that's required is to use
**%setup** with no options:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%setup

            </code></pre></td>
</tr>
</tbody>
</table>

This produces the following set of commands:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>cd /usr/src/redhat/BUILD
rm -rf cdplayer-1.0
gzip -dc /usr/src/redhat/SOURCES/source-zero.tar.gz | tar -xvvf -
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

If `source-zero.tar.gz` didn't include a top-level directory, we could
have made one by adding the **-c** option:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%setup -c

            </code></pre></td>
</tr>
</tbody>
</table>

which would result in:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>cd /usr/src/redhat/BUILD
rm -rf cdplayer-1.0
mkdir -p cdplayer-1.0
cd cdplayer-1.0
gzip -dc /usr/src/redhat/SOURCES/source-zero.tar.gz | tar -xvvf -
if [ $? -ne 0 ]; then
  exit $?
fi
cd /usr/src/redhat/BUILD/cdplayer-1.0
chown -R root.root .
chmod -R a+rX,g-w,o-w .

            </code></pre></td>
</tr>
</tbody>
</table>

Of course, if the top-level directory did not match the package name,
the **-n** option could have been added:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%setup -n blather

            </code></pre></td>
</tr>
</tbody>
</table>

which results in:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>cd /usr/src/redhat/BUILD
rm -rf blather
gzip -dc /usr/src/redhat/SOURCES/source-zero.tar.gz | tar -xvvf -
if [ $? -ne 0 ]; then
  exit $?
fi
cd blather
cd /usr/src/redhat/BUILD/blather
chown -R root.root .
chmod -R a+rX,g-w,o-w .

            </code></pre></td>
</tr>
</tbody>
</table>

or

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%setup -c -n blather

            </code></pre></td>
</tr>
</tbody>
</table>

This results in:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>cd /usr/src/redhat/BUILD
rm -rf blather
mkdir -p blather
cd blather
gzip -dc /usr/src/redhat/SOURCES/source-zero.tar.gz | tar -xvvf -
if [ $? -ne 0 ]; then
  exit $?
fi
cd /usr/src/redhat/BUILD/blather
chown -R root.root .
chmod -R a+rX,g-w,o-w .

            </code></pre></td>
</tr>
</tbody>
</table>

Now let's add the second source file. Things get a bit more interesting
here. First, we need to identify which **source** tag (and therefore,
which source file) we're talking about. So we need to use either the
**-a** or **-b** option, depending on the characteristics of the source
archive. For this example, let's say that **-a** is the option we want.
Adding that option, plus a "1" to point to the source file specified in
the **source1** tag, we have:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%setup -a 1

            </code></pre></td>
</tr>
</tbody>
</table>

Since we've already seen that using the **-a** or **-b** option results
in duplicate unpacking, we need to disable the default unpacking by
adding the **-T** option:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%setup -T -a 1

            </code></pre></td>
</tr>
</tbody>
</table>

Next, we need to make sure that the top-level directory isn't deleted.
Otherwise, the first source file we just unpacked would be gone. That
means we need to include the **-D** option to prevent that from
happening. Adding this final option, and including the now complete
macro in our **%prep** script, we now have:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%setup
%setup -T -D -a 1

            </code></pre></td>
</tr>
</tbody>
</table>

This will result in the following commands:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>cd /usr/src/redhat/BUILD
rm -rf cdplayer-1.0
gzip -dc /usr/src/redhat/SOURCES/source-zero.tar.gz | tar -xvvf -
if [ $? -ne 0 ]; then
  exit $?
fi
cd cdplayer-1.0
cd /usr/src/redhat/BUILD/cdplayer-1.0
chown -R root.root .
chmod -R a+rX,g-w,o-w .
cd /usr/src/redhat/BUILD
cd cdplayer-1.0
gzip -dc /usr/src/redhat/SOURCES/source-one.tar.gz | tar -xvvf -
if [ $? -ne 0 ]; then
  exit $?
fi
cd /usr/src/redhat/BUILD/cdplayer-1.0
chown -R root.root .
chmod -R a+rX,g-w,o-w .

            </code></pre></td>
</tr>
</tbody>
</table>

So far, so good. Let's include the last source file, but with this one,
we'll say that it needs to be unpacked in a subdirectory of
`cdplayer-1.0` called `database`. Can we use **%setup** in this case?

We could, if `source-two.tgz` created the `database` subdirectory. If
not, then it'll be necessary to do it by hand. For the purposes of our
example, let's say that `source-two.tgz` wasn't created to include the
`database` subdirectory, so we'll have to do it ourselves. Here's our
**%prep** script now:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%setup
%setup -T -D -a 1
mkdir database
cd database
gzip -dc /usr/src/redhat/SOURCES/source-two.tar.gz | tar -xvvf -

            </code></pre></td>
</tr>
</tbody>
</table>

Here's the resulting script:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>cd /usr/src/redhat/BUILD
rm -rf cdplayer-1.0
gzip -dc /usr/src/redhat/SOURCES/source-zero.tar.gz | tar -xvvf -
if [ $? -ne 0 ]; then
  exit $?
fi
cd cdplayer-1.0
cd /usr/src/redhat/BUILD/cdplayer-1.0
chown -R root.root .
chmod -R a+rX,g-w,o-w .
cd /usr/src/redhat/BUILD
cd cdplayer-1.0
gzip -dc /usr/src/redhat/SOURCES/source-one.tar.gz | tar -xvvf -
if [ $? -ne 0 ]; then
  exit $?
fi
mkdir database
cd database
gzip -dc /usr/src/redhat/SOURCES/source-two.tar.gz | tar -xvvf -

            </code></pre></td>
</tr>
</tbody>
</table>

The three commands we added to unpack the last set of sources were added
to the end of the **%prep** script.

The bottom line to using the **%setup** macro is that you can probably
get it to do what you want, but don't be afraid to tinker. And even if
**%setup** can't be used, it's easy enough to add the necessary commands
to do the work manually. Above all, make sure you use the **--test**
option when testing your **%setup** macros, so you can see what commands
they're translating to.

Next, let's look at RPM's second macro, **%patch**.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-inside-patch-macro">The **%patch** Macro</span>

The **%patch** macro, as its name implies, is used to apply patches to
the unpacked sources. In the following examples, our spec file has the
following **patch** tag lines:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>patch0: patch-zero
patch1: patch-one
patch2: patch-two

          </code></pre></td>
</tr>
</tbody>
</table>

At its simplest, the **%patch** macro can be invoked without any
options:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%patch

          </code></pre></td>
</tr>
</tbody>
</table>

Here are the resulting commands:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>echo &quot;Patch #0:&quot;
patch -p0  -s &lt; /usr/src/redhat/SOURCES/patch-zero

          </code></pre></td>
</tr>
</tbody>
</table>

The **%patch** macro nicely displays a message showing that a patch is
being applied, then invokes the **patch** command to actually do the
dirty work. There are two options to the **patch** command:

1.  The **-p** option, which directs **patch** to remove the specified
    number of slashes (and any intervening directories) from the front
    of any filenames specified in the patch file. In this case, nothing
    will be removed.

2.  The **-s** option, which directs **patch** to apply the patch
    without displaying any informational messages. Only errors from
    **patch** will be displayed.

How did the **%patch** macro know which patch to apply? Keep in mind
that, like the **source** tag lines, every **patch** tag is numbered,
starting at zero. The **%patch** macro, by default, applies the patch
file named on the **patch** (or **patch0**) tag line.

<div class="sect3">

### <span id="s3-rpm-inside-which-patch-tag">Specifying Which **patch** Tag to Use</span>

The **%patch** macro actually has two different ways to specify the
**patch** tag line it is to use. The first method is to simply append
the number of the desired **patch** tag to the end of the **%patch**
macro itself. For example, in order to apply the patch specified on the
**patch2** tag line, the following **%patch** macro could be used:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%patch2

            </code></pre></td>
</tr>
</tbody>
</table>

The other approach is to use the **-P** option. This option is followed
by the number of the **patch** tag line desired. Therefore, this line is
identical in function to the previous one:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%patch -P 2

            </code></pre></td>
</tr>
</tbody>
</table>

Note that the **-P** option will *not* apply the file specified on the
**patch0** line, by default. Therefore, if you choose to use the **-P**
option to specify patch numbers, you'll need to use the following format
when applying patch zero:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%patch -P 0

            </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect3">

### <span id="s3-rpm-inside-patch-p-option">**-p `<#>`** — Strip **`<#>`** leading slashes and directories from patch filenames</span>

The **-p** (Note the *lowercase* "p"\!) option is sent directly to the
**patch** command. It is followed by a number, which specifies the
number of leading slashes (and the directories in between) to strip from
any filenames present in the patch file. For more information on this
option, please consult the **patch** man page.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-b-option">**-b `<name>`** — Set the backup file extension to **`<name>`**</span>

When the **patch** command is used to apply a patch, unmodified copies
of the files patched are renamed to end with the extension `.orig`. The
**-b** option is used to change the extension used by **patch**. This is
normally done when multiple patches are to be applied to a given file.
By doing this, copies of the file as it existed prior to each patch, are
readily available.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-e-option">**-E** — Remove Empty Output Files</span>

The **-E** option is passed directly to the **patch** program. When
**patch** is run with the **-E** option, any output files that are empty
after the patches have been applied, are removed.

Now let's take **%patch** on a test-drive, and put it through its paces.

</div>

<div class="sect3">

### <span id="s3-rpm-inside-patch-rw">An example of the **%patch** Macro in Action</span>

Using the example **patch** tag lines we've used throughout this
section, let's put together an example and look at the resulting
commands. In our example, the first patch to be applied needs to have
the root directory stripped. Its **%patch** macro will look like this:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%patch -p1

            </code></pre></td>
</tr>
</tbody>
</table>

The next patch is to be applied to files in the software's `lib`
subdirectory, so we'll need to add a **cd** command to get us there.
We'll also need to strip an additional directory:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>cd lib
%patch -P 1 -p2

            </code></pre></td>
</tr>
</tbody>
</table>

Finally, the last patch is to be applied from the software's top-level
directory, so we need to **cd** back up a level. In addition, this patch
modifies some files that were also patched the first time, so we'll need
to change the backup file extension:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>cd ..
%patch -P 2 -p1 -b .last-patch

            </code></pre></td>
</tr>
</tbody>
</table>

Here's what the **%prep** script (minus any **%setup** macros) looks
like:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>%patch -p1
cd lib
%patch -P 1 -p2
cd ..
%patch -P 2 -p1 -b .last-patch

            </code></pre></td>
</tr>
</tbody>
</table>

And here's what the macros expand to:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>echo &quot;Patch #0:&quot;
patch -p1  -s &lt; /usr/src/redhat/SOURCES/patch-zero
cd lib
echo &quot;Patch #1:&quot;
patch -p2  -s &lt; /usr/src/redhat/SOURCES/patch-one
cd ..
echo &quot;Patch #2:&quot;
patch -p1 -b .last-patch -s &lt; /usr/src/redhat/SOURCES/patch-two

            </code></pre></td>
</tr>
</tbody>
</table>

No surprises here. Note that the **%setup** macro leaves the current
working directory set to the software's top-level directory, so our
**cd** commands with their relative paths will do the right thing. Of
course, we have environment variables available that could be used here,
too.

<div class="sect4">

#### <span id="s4-rpm-inside-patch-compressed">Compressed Patch Files</span>

If a patch file is compressed with **gzip**, RPM will automatically
decompress it before applying the patch. Here's a compressed patch file
as specified in the spec file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>Patch: bother-3.5-hack.patch.gz

              </code></pre></td>
</tr>
</tbody>
</table>

This is part of the script RPM will execute when the **%prep** section
is executed:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>echo Executing: %prep
…
echo &quot;Patch #0:&quot;
gzip -dc /usr/src/redhat/SOURCES/bother-3.5-hack.patch.gz | patch -p1  -s
…

              </code></pre></td>
</tr>
</tbody>
</table>

First, the patch file is decompressed using **gzip**. The output from
**gzip** is then piped into **patch**.

That's about it for RPM's macros. Next, let's take a look at the
**%files** list.

</div>

</div>

</div>

</div>

### Notes

|                                                                          |                                                                        |
| ------------------------------------------------------------------------ | ---------------------------------------------------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-inside-macros.html#AEN8439) | Yes, the **source** tags should include a URL pointing to the sources. |

<div class="NAVFOOTER">

-----

|                                    |                          |                                       |
| :--------------------------------- | :----------------------: | ------------------------------------: |
| [Prev](s1-rpm-inside-scripts.html) |    [Home](index.html)    | [Next](s1-rpm-inside-files-list.html) |
| Scripts: RPM's Workhorse           | [Up](ch-rpm-inside.html) |                   The **%files** List |

</div>
