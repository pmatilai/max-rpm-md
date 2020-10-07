<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-reloc-prefix-tag.md)

Chapter 15. Making a Relocatable Package

[Next](s1-rpm-reloc-building-relocatable.md)

-----

<div class="sect1">

# <span id="s1-rpm-reloc-wrinkles">Relocatable Wrinkles: Things to Consider</span>

While it's certainly no problem to add a **prefix** tag line to a spec
file, it's necessary to consider a few other issues:

  - Every file in the **%files** list must start with the path specified
    on the **prefix** tag line.

  - The software must be written such that it can operate properly if
    relocated. Absolute symlinks are a prime example of this.

  - Other software must not rely on the relocatable package being
    installed in any particular location.

Let's cover each of these issues, starting with the **%files** list.

<div class="sect2">

## <span id="s2-rpm-reloc-files-list-restrictions">**%files** List Restrictions</span>

As mentioned above, each file in the **%files** list must start with the
relocation prefix. If this isn't done, the build will fail:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmbuild -ba cdplayer-1.0.spec
* Package: cdplayer
+ umask 022
+ echo Executing: %prep
…
Binary Packaging: cdplayer-1.0-1
Package Prefix = usr/local
File doesn&#39;t match prefix (usr/local): /usr/doc/cdplayer-1.0-1
File not found: /usr/doc/cdplayer-1.0-1
Build failed.

# 
          </code></pre></td>
</tr>
</tbody>
</table>

In our example, the build proceeded normally until the time came to
create the binary package file. At that point RPM detected the problem.
The error message says it all: The **prefix** line in the spec file
(`/usr/local`) was not present in the first part of the file's
(`/usr/doc/cdplayer-1.0-1`) path. This stopped the build in its tracks.

The fact that every file in a relocatable package must be installed
under the directory specified in the **prefix** line, raises some
issues. For example, what about a program that reads a configuration
file normally kept in `/etc`?

This question leads right into our next section.

</div>

<div class="sect2">

## <span id="s2-rpm-reloc-must-be-relocatable">Relocatable Packages Must Contain Relocatable Software</span>

While this section's title seems pretty obvious, it's not always easy to
tell if a particular piece of software can be relocated. Let's take a
look at the question raised at the end of the previous section. If a
program has been written to read its configuration file from `/etc`,
there are three possible approaches to making that program relocatable:

1.  Set the prefix to `/etc` and package everything under `/etc`.

2.  Package everything somewhere other than `/etc` and leave out the
    config file entirely.

3.  Modify the program.

The first approach would certainly work from a purely technical
standpoint, but not many people would be happy with a program that
installed itself in `/etc`. So this approach isn't viable.

The second approach might be more appropriate, but it forces users to
complete the install by having them create the config file themselves.
If RPM's goal is to make software easier to install and remove, this is
not a viable approach, either\!

The final approach might be the best. Once the program is installed,
when the rewritten software is first run, it could see that no
configuration file existed in `/etc`, and create one.

However, even though this would work, when the time came to erase the
package, the config file would be left behind. RPM had never installed
it, so RPM couldn't get rid of it. There's also the fact that this
approach is probably more labor intensive than most package builders
would like.

None of these approaches are very appealing, are they? Some software
just doesn't relocate very well. In general, any of the following things
are warning signs that relocation is going to be a problem:

  - The software contains one or more files that must be installed in
    specific directories

  - The software refers to system files using relative paths (Which is
    really just another way of saying the software must be installed in
    a particular directory)

If these kinds of issues crop up, then making the software relocatable
is going to be tough. And there's still one issue left to consider.

</div>

<div class="sect2">

## <span id="s2-rpm-reloc-referenced">The Relocatable Software Is Referenced By Other Software</span>

Even assuming the software is written so that it can be put in a
relocatable package, there still might be a problem. And that problem
centers not on the relocatable software itself, but on other programs
that reference the relocatable software.

For example, there are times when a package needs to execute other
programs. This might include backup software that needs to send mail, or
a communications program that needs to compress files. If these
underlying programs were relocatable, and not installed where other
packages expect them, then they would be of little use.

Granted, this isn't a common problem, but it can happen. And for the
package builder interested in building relocatable packages, it's an
issue that needs to be explored. Unfortunately, this type of problem can
be the hardest to find.

If, however, a software product has been found to be relocatable, the
mechanics of actually building a relocatable package are pretty
straightforward. Let's give it a try.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                        |                         |                                                |
| :------------------------------------- | :---------------------: | ---------------------------------------------: |
| [Prev](s1-rpm-reloc-prefix-tag.md)   |   [Home](index.md)    | [Next](s1-rpm-reloc-building-relocatable.md) |
| The **prefix** tag: Relocation Central | [Up](ch-rpm-reloc.md) |                 Building a Relocatable Package |

</div>
