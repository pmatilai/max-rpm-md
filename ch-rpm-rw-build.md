<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-multi-hints-and-kinks.html)

[Next](s1-rpm-rw-build-build-without-rpm.html)

-----

<div class="chapter">

# <span id="ch-rpm-rw-build"></span>Chapter 20. Real-World Package Building

In [Chapter 11](ch-rpm-build.html), we packaged a fairly simple
application. Since our goal was to introduce package building, we kept
things as simple as possible. However, things aren't always that simple
in the real world.

In this chapter, we'll package a more complex application that will call
on most of RPM's capabilities. We'll start with a general overview of
the application and end with a completed package, just as you would if
you were tasked with packaging an application that you'd not seen
before.

So without further ado, let's meet amanda…

<div class="sect1">

# <span id="s1-rpm-rw-build-amanda-overview">An Overview of Amanda</span>

Amanda is a network backup utility. The name amanda stands for "Advanced
Maryland Automatic Network Disk Archiver". If the word "Maryland" seems
somewhat incongruous, it helps to realize that the program was developed
at the University of Maryland by James Da Silva, and has subsequently
been enhanced by many people around the world.

The sources are available at `ftp.cs.umd.edu`, in directory
`/pub/amanda`. At the time of writing, the latest version of amanda is
version 2.3.0. Therefore, it should come as no surprise that the amanda
source **tar** file is called amanda-2.3.0.tar.gz.

As with most network-centric applications, amanda has a server
component, and a client component. An amanda server controls how the
various client systems are backed up to the server's tape drive. Each
amanda client uses the operating system's native **dump** utility to
perform the actual backup, which is then compressed and sent to the
server. A server can back itself up simply by having the client software
installed and configured, just like any other client system.

The software builds with **make**, and most customization is done in two
`.h` files in the `config` subdirectory. A fair amount of documentation
is available in the `doc` subdirectory. All in all, amanda is a typical
non-trivial application.

Amanda can be built on several Unix-based operating systems. In this
chapter, we'll build and package amanda for Red Hat Linux Linux version
4.0.

</div>

</div>

<div class="NAVFOOTER">

-----

|                                           |                    |                                                |
| :---------------------------------------- | :----------------: | ---------------------------------------------: |
| [Prev](s1-rpm-multi-hints-and-kinks.html) | [Home](index.html) | [Next](s1-rpm-rw-build-build-without-rpm.html) |
| Hints and Kinks                           |  [Up](p5206.html)  |                   Initial Building Without RPM |

</div>
