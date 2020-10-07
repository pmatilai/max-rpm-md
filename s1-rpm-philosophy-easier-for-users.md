<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-philosophy-multi-architecture.md)

Chapter 9. The Philosophy Behind RPM

[Next](s1-rpm-philosophy-summary.md)

-----

<div class="sect1">

# <span id="s1-rpm-philosophy-easier-for-users">Easier For Your Users</span>

While we are primarily concerned with RPM's advantages from the
developer's point of view, it's worth looking at RPM from the user's
standpoint for a moment. After all, if RPM makes life easier for your
users, that can translate into lower support costs.

<div class="sect2">

## <span id="s2-rpm-philosophy-easy-upgrades">Easy Upgrades</span>

Probably the biggest headache for user and developer alike is the
upgrade of an application, or worse yet, an entire operating system\!
RPM can make upgrading a one-step process. With one command, a new
package can be installed, and the remnants of the old package removed.

</div>

<div class="sect2">

## <span id="s2-rpm-philosophy-config-file-handling">Intelligent Configuration File Handling</span>

Configuration files — nearly every application has them. They may go by
different names, but they all control the behavior of their application.
Users normally customize config files to their liking and would be upset
if their customizations were lost during the installation, upgrade, or
removal of a package.

RPM takes special care with a user's config files. By using MD5
checksums, RPM can determine what action is most appropriate with a
config file. If a config file has been modified by the user and has to
be replaced, it is saved. That way a user's modifications are never
lost.

</div>

<div class="sect2">

## <span id="s2-rpm-philosophy-powerful-queries">Powerful Query Capabilities</span>

RPM uses a database to keep track of all files it installs. RPM's
database provides other benefits, such as the wide variety of
information that can be easily retrieved from it. RPM's query command
makes it easy for users to quickly answer a number of questions, such
as:

  - Where did this file come from? Is it part of a package?

  - What does this package do?

  - What packages are installed on my system?

These are just a few examples of the many ways RPM can provide
information about one or more packages on a user's system.

</div>

<div class="sect2">

## <span id="s2-rpm-philosophy-easy-verification">Easy Package Verification</span>

Another way that RPM leverages the information stored in its database,
is by providing an easy way to verify that a package is properly
installed. With this capability, RPM makes it easy to determine, for
example, what packages were damaged by a wildcard delete in `/usr/bin`.
In addition, RPM's verification command can detect changes to file
attributes, such as a file's permissions, ownership, and size.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                                   |                              |                                        |
| :------------------------------------------------ | :--------------------------: | -------------------------------------: |
| [Prev](s1-rpm-philosophy-multi-architecture.md) |      [Home](index.md)      | [Next](s1-rpm-philosophy-summary.md) |
| Multi-architecture/operating system Support       | [Up](ch-rpm-philosophy.md) |                          To Summarize… |

</div>
