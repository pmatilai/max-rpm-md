<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-rw-build.html)

Chapter 20. Real-World Package Building

[Next](s1-rpm-rw-build-initial-build-with-rpm.html)

-----

<div class="sect1">

# <span id="s1-rpm-rw-build-build-without-rpm">Initial Building Without RPM</span>

Since amanda can be built on numerous platforms, there needs to be some
initial customization when first building the software. Since
customization implies that mistakes will be made, we'll start off by
building amanda without any involvement on the part of RPM.

But before we can build amanda, we have to get it and unpack it, first.

<div class="sect2">

## <span id="s2-rpm-rw-build-test-build-area">Setting Up A Test Build Area</span>

As we mentioned above, the home FTP site for amanda is `ftp.cs.umd.edu`.
The sources are in `/pub/amanda`.

After getting the sources, it's necessary to unpack them. We'll unpack
them into RPM's `SOURCES` directory, so that we can keep all our work in
one place:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># tar zxvf amanda-2.3.0.tar.gz
amanda-2.3.0/
amanda-2.3.0/COPYRIGHT
amanda-2.3.0/Makefile
amanda-2.3.0/README
…
amanda-2.3.0/man/amtape.8
amanda-2.3.0/tools/
amanda-2.3.0/tools/munge
…

          </code></pre></td>
</tr>
</tbody>
</table>

As we saw, the sources unpacked into a directory called `amanda-2.3.0`.
Let's rename that directory to `amanda-2.3.0-orig`, and unpack the
sources again:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls
total 177
drwxr-xr-x  11 adm      games        1024 May 19  1996 amanda-2.3.0/
-rw-r--r--   1 root     root       178646 Nov 20 10:42 amanda-2.3.0.tar.gz

# mv amanda-2.3.0 amanda-2.3.0-orig
# tar zxvf amanda-2.3.0.tar.gz
amanda-2.3.0/
amanda-2.3.0/COPYRIGHT
amanda-2.3.0/Makefile
amanda-2.3.0/README
…
amanda-2.3.0/man/amtape.8
amanda-2.3.0/tools/
amanda-2.3.0/tools/munge

# ls
total 178
drwxr-xr-x  11 adm      games        1024 May 19  1996 amanda-2.3.0/
drwxr-xr-x  11 adm      games        1024 May 19  1996 amanda-2.3.0-orig/
-rw-r--r--   1 root     root       178646 Nov 20 10:42 amanda-2.3.0.tar.gz

#
          </code></pre></td>
</tr>
</tbody>
</table>

Now why did we do that? The reason lies in the fact that we will
undoubtedly need to make changes to the original sources in order to get
amanda to build on Linux. We'll do all our hacking in the `amanda-2.3.0`
directory, and leave the `amanda-2.3.0-orig` untouched.

Since one of RPM's design features is to build packages from the
original, unmodified sources, that means the changes we'll make will
need to be kept as a set of patches. The `amanda-2.3.0-orig` directory
will let us issue a simple recursive **diff** command to create our
patches when the time comes.

Now that our sources are unpacked, it's time to work on building the
software.

</div>

<div class="sect2">

## <span id="s2-rpm-rw-build-getting-to-build">Getting Software to build</span>

Looking at the `docs/INSTALL` file, we find that the steps required to
get amanda configured and ready to build are actually fairly simple. The
first step is to modify `tools/munge` to point to **cpp**, the C
preprocessor.

Amanda uses CPP to create makefiles containing the appropriate
configuration information. This approach is a bit unusual, but not
unheard of. In `munge`, we find the following section:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># Customize CPP to point to your system&#39;s C preprocessor.

# if cpp is on your path:
CPP=cpp

# if cpp is not on your path, try one of these:
# CPP=/lib/cpp                  # traditional
# CPP=/usr/lib/cpp              # also traditional
# CPP=/usr/ccs/lib/cpp          # Solaris 2.x

          </code></pre></td>
</tr>
</tbody>
</table>

Since **cpp** exists in `/lib` on Red Hat Linux, we need to change this
part of `munge` to:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># Customize CPP to point to your system&#39;s C preprocessor.

# if cpp is on your path:
#CPP=cpp

# if cpp is not on your path, try one of these:
CPP=/lib/cpp                    # traditional
# CPP=/usr/lib/cpp              # also traditional
# CPP=/usr/ccs/lib/cpp          # Solaris 2.x

          </code></pre></td>
</tr>
</tbody>
</table>

Next, we need to take a look in `config/` and create two files:

1.  `config.h` — contains platform-specific configuration information

2.  `options.h` — contains site-specific configuration information

There are a number of example `config.h` files for a number of different
platforms. There is a Linux-specific version, so we copy that file to
`config.h` and review it. After a few changes to reflect our Red Hat
Linux Linux environment, it's ready. Now let's turn our attention to
`options.h`.

In the case of `options.h`, there's only one example file called
`options.h-vanilla`. As the name implies, this is a basic file that
contains a series of **\#define**s that configure amanda for a typical
environment. We'll need to make a few changes:

  - Define the paths to common utility programs.

  - Keep the programs from being named with the suffix `-2.3.0`.

  - Define the directories where the programs should be installed.

While the first change is pretty much standard fare for anyone used to
building software, the last two changes are really due to RPM. With RPM,
there's no need to name the programs with a version-specific name, as
RPM can easily upgrade to a new version and even downgrade back, if the
new version doesn't work as well. The default paths amanda uses
segregate the files so that they can be easily maintained. With RPM,
there's no need to do this, since every file installed by RPM gets
written into the database. In addition, Red Hat Linux systems adhere to
the File System Standard, so any package destined for Red Hat systems
should really be FSSTND-compliant, too. Fortunately for us, amanda was
written to make these types of changes easy. But even if we had to hack
an installation script, RPM would pick up the changes as part of its
patch handling.

We'll spare you the usual discovery of typos, incompatibilities, and the
resulting rebuilds. After an undisclosed number of iterations, our
`config.h` and `options.h` files are perfect. Amanda builds:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># make
Making all in common-src
make[1]: Entering directory `/usr/src/redhat/SOURCES/amanda-2.3.0/common-src&#39;
…
make[1]: Leaving directory `/usr/src/redhat/SOURCES/amanda-2.3.0/man&#39;

#
          </code></pre></td>
</tr>
</tbody>
</table>

As we noted, amanda is constructed so that most of the time changes will
only be necessary in `tools/munge`, and the two files in `config`. Our
situation was no different — after all was said and done, that was all
we needed to hack.

</div>

<div class="sect2">

## <span id="s2-rpm-rw-build-installing-testing">Installing and testing</span>

As we all know, just because software builds doesn't mean that it's
ready for prime-time. It's necessary to test it first. In order to test
amanda, we need to install it. Amanda's makefile has an install target,
so let's use that to get started. We'll also get a copy of the output,
because we'll need that later:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># make install
Making install in common-src
…
make[1]: Entering directory `/usr/src/redhat/SOURCES/amanda-2.3.0/client-src&#39;
Installing Amanda client-side programs:
  install -c -o bin amandad /usr/lib/amanda
  install -c -o bin sendsize /usr/lib/amanda
  install -c -o bin calcsize /usr/lib/amanda
  install -c -o bin sendbackup-dump /usr/lib/amanda
  install -c -o bin sendbackup-gnutar /usr/lib/amanda
  install -c -o bin runtar /usr/lib/amanda
  install -c -o bin selfcheck /usr/lib/amanda
Setting permissions for setuid-root client programs:
  (cd /usr/lib/amanda ; chown root calcsize; chmod u+s calcsize)
  (cd /usr/lib/amanda ; chown root runtar; chmod u+s runtar)
…
Making install in server-src
Installing Amanda libexec programs:
  install -c -o bin taper /usr/lib/amanda
  install -c -o bin dumper /usr/lib/amanda
  install -c -o bin driver /usr/lib/amanda
  install -c -o bin planner /usr/lib/amanda
  install -c -o bin reporter /usr/lib/amanda
  install -c -o bin getconf /usr/lib/amanda
Setting permissions for setuid-root libexec programs:
  (cd /usr/lib/amanda ; chown root dumper; chmod u+s dumper)
  (cd /usr/lib/amanda ; chown root planner; chmod u+s planner)
Installing Amanda user programs:
  install -c -o bin amrestore /usr/sbin
  install -c -o bin amadmin /usr/sbin
  install -c -o bin amflush /usr/sbin
  install -c -o bin amlabel /usr/sbin
  install -c -o bin amcheck /usr/sbin
  install -c -o bin amdump /usr/sbin
  install -c -o bin amcleanup /usr/sbin
  install -c -o bin amtape /usr/sbin
Setting permissions for setuid-root user programs:
  (cd /usr/sbin ; chown root amcheck; chmod u+s amcheck)
…
Installing Amanda changer libexec programs:
  install -c -o bin chg-generic /usr/lib/amanda
…
Installing Amanda man pages:
  install -c -o bin amanda.8 /usr/man/man8
  install -c -o bin amadmin.8 /usr/man/man8
  install -c -o bin amcheck.8 /usr/man/man8
  install -c -o bin amcleanup.8 /usr/man/man8
  install -c -o bin amdump.8 /usr/man/man8
  install -c -o bin amflush.8 /usr/man/man8
  install -c -o bin amlabel.8 /usr/man/man8
  install -c -o bin amrestore.8 /usr/man/man8
  install -c -o bin amtape.8 /usr/man/man8
…

# 
          </code></pre></td>
</tr>
</tbody>
</table>

OK, no major problems there. Amanda does require a bit of additional
effort to get everything running, though. Looking at `docs/INSTALL`, we
follow the steps to get amanda running on our test system, as both a
client and a server. As we perform each step, we note it for future
reference:

  - For the client:
    
    1.  Set up a `~/.rhosts` file allowing the server to connect.
    
    2.  Make the disk device files readable by the client.
    
    3.  Make `/etc/dumpdates` readable and writable by the client.
    
    4.  Put an amanda entry in `/etc/services`.
    
    5.  Put an amanda entry in `/etc/inetd.conf`.
    
    6.  Issue a **kill -HUP** on inetd.

  - For the server:
    
    1.  Create a directory to hold the server configuration files.
    
    2.  Modify the provided example configuration files to suit our
        site.
    
    3.  Add crontab entries to run amanda nightly.
    
    4.  Put an amanda entry in `/etc/services`.

Once everything is ready, we run a few tests. Everything performs
flawlessly. [<span class="footnote">\[1\]</span>](#FTN.AEN11972) Looks
like we've got a good build. Let's start getting RPM involved.

</div>

</div>

### Notes

|                                                                                        |                           |
| -------------------------------------------------------------------------------------- | ------------------------- |
| [<span class="footnote">\[1\]</span>](s1-rpm-rw-build-build-without-rpm.html#AEN11972) | Well, eventually it did\! |

<div class="NAVFOOTER">

-----

|                              |                            |                                                     |
| :--------------------------- | :------------------------: | --------------------------------------------------: |
| [Prev](ch-rpm-rw-build.html) |     [Home](index.html)     | [Next](s1-rpm-rw-build-initial-build-with-rpm.html) |
| Real-World Package Building  | [Up](ch-rpm-rw-build.html) |                           Initial Building With RPM |

</div>
