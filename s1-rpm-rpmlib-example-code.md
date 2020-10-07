<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-rpmlib-functions.md)

Chapter 21. A Guide to the RPM Library API

[Next](p14028.md)

-----

<div class="sect1">

# <span id="s1-rpm-rpmlib-example-code">Example Code</span>

In this section, we'll study example programs that make use of rpmlib to
perform an assortment of commonly-required operations.

<div class="sect2">

## <span id="s2-rpm-rpmlib-example1">Example \#1</span>

In this example, we'll use a number of rpmlib's header manipulation
functions.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;errno.h&gt;
#include &lt;fcntl.h&gt;
#include &lt;stdio.h&gt;
#include &lt;unistd.h&gt;
#include &lt;string.h&gt;

#include &lt;rpm/rpmlib.h&gt;

          </code></pre></td>
</tr>
</tbody>
</table>

Here we've included `rpmlib.h`, which is necessary for all programs that
use rpmlib.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>void main(int argc, char ** argv)
{
  HeaderIterator iter;
    Header h, sig;
    int_32 itertag, type, count;
    void **p = NULL;
    char *blather;
    char * name;

    int fd, stat;

          </code></pre></td>
</tr>
</tbody>
</table>

Here we've defined the program's storage. Note in particular the
`HeaderIterator`, `Header`, and `int_32` datatypes.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>    if (argc == 1) {
        fd = 0;
    } else {
        fd = open(argv[1], O_RDONLY, 0644);
    }

    if (fd &lt; 0) {
        perror(&quot;open&quot;);
        exit(1);
    }

          </code></pre></td>
</tr>
</tbody>
</table>

Standard stuff here. The first argument is supposed to be an RPM package
file. It is opened here. If there is no argument on the command line,
the program will use stdin instead.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>    stat = rpmReadPackageInfo(fd, &amp;sig, &amp;h);
    if (stat) {
      fprintf(stderr,
              &quot;rpmReadPackageInfo error status: %d\n%s\n&quot;,
              stat, strerror(errno));
        exit(stat);
    }

          </code></pre></td>
</tr>
</tbody>
</table>

Here things start to get interesting\! The signature and headers are
read from package file that was just opened. If you noticed above, we've
defined `sig` and `h` to be of type `Header`. That means we can use
rpmlib's header-related functions on them. After a little bit of error
checking, and it's time to move on…

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>    headerGetEntry(h, RPMTAG_NAME, &amp;type, (void **) &amp;name, &amp;count);

    if (headerIsEntry(h, RPMTAG_PREIN)) {
        printf(&quot;There is a preinstall script for %s\n&quot;, name);
    }

    if (headerIsEntry(h, RPMTAG_POSTIN)) {
        printf(&quot;There is a postinstall script for %s\n&quot;, name);
    }

          </code></pre></td>
</tr>
</tbody>
</table>

Now that we have the package's header, we get the package name
(specified by the <span class="symbol">RPMTAG\_NAME</span> above). Next,
we see if the package has pre-install
(<span class="symbol">RPMTAG\_PREIN</span>) or post-install
(<span class="symbol">RPMTAG\_POSTIN</span>) scripts. If there are, we
print out a message, along with the package name.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>    printf(&quot;Dumping signatures...\n&quot;);
    headerDump(sig, stdout, 1);

    rpmFreeSignature(sig);

          </code></pre></td>
</tr>
</tbody>
</table>

Turning to the other `Header` structure we've read, we print out the
package's signatures in human-readable form. When we're done, we free
the block of signatures.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>    printf(&quot;Iterating through the header...\n&quot;);

    iter = headerInitIterator(h);

          </code></pre></td>
</tr>
</tbody>
</table>

Here we set up an iterator for the package's header. This will allow us
to step through each entry in the header.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>    while (headerNextIterator(iter, &amp;itertag, &amp;type, p, &amp;count)) {
      switch (itertag) {
      case RPMTAG_SUMMARY:
        blather = *p;
        printf(&quot;The Summary: %s\n&quot;, blather);
        break;
      case RPMTAG_FILENAMES:
        printf(&quot;There are %d files in this package\n&quot;, count);
        break;
      }

          </code></pre></td>
</tr>
</tbody>
</table>

This loop uses `headerNextIterator()` to return each entry's tag, type,
data, and size. By using a **switch** statement on the tag, we can
perform different operations on each type of entry in the header.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>    }

    headerFreeIterator(iter);

    headerFree(h);

}

          </code></pre></td>
</tr>
</tbody>
</table>

This is the housecleaning section of the program. First we free the
iterator that we've been using, and finally the header itself. Running
this program on a package gives us the following output:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ./dump amanda-client-2.3.0-2.i386.rpm
There is a postinstall script for amanda-client
Dumping signatures...
Entry count: 2
Data count : 20

             CT  TAG                  TYPE           OFSET      COUNT
Entry      : 000 (1000)NAME           INT32_TYPE     0x00000000 00000001
       Data: 000 0x00029f5d (171869)
Entry      : 001 (1003)SERIAL         BIN_TYPE       0x00000004 00000016
       Data: 000 27 01 f9 97 d8 2c 36 40 
       Data: 008 c6 4a 91 45 32 13 d1 62 
Iterating through the header...
The Summary: Client-side Amanda package
There are 11 files in this package

# 
          </code></pre></td>
</tr>
</tbody>
</table>

</div>

<div class="sect2">

## <span id="s2-rpm-rpmlib-example2">Example \#2</span>

This example delves a bit more into the database-related side of rpmlib.
After initializing rpmlib's variables by reading the appropriate `rpmrc`
files, the code traverses the database records, looking for a specific
package. That package's header is then dumped in its entirety.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;errno.h&gt;
#include &lt;fcntl.h&gt;
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;
#include &lt;unistd.h&gt;
#include &lt;stdlib.h&gt;

#include &lt;rpm/rpmlib.h&gt;

          </code></pre></td>
</tr>
</tbody>
</table>

As before, this is the normal way of including all of rpmlib's
definitions.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>void main(int argc, char ** argv)
{
    Header h;
    int offset;
    int dspBlockNum = 0;                /* default to all */
    int blockNum = 0;
    int_32 type, count;
    char * name;
    rpmdb db;

          </code></pre></td>
</tr>
</tbody>
</table>

Here are the data declarations. Note the declaration of `db`: this is
how we will be accessing the RPM database.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>    printf(&quot;The database path is: %s\n&quot;,
        rpmGetVar(RPMVAR_DBPATH) ? rpmGetVar(RPM_DBPATH) : &quot;(none)&quot;);

    rpmReadConfigFiles(NULL, NULL, NULL, 0);

    printf(&quot;The database path is: %s\n&quot;,
        rpmGetVar(RPMVAR_DBPATH) ? rpmGetVar(RPM_DBPATH) : &quot;(none)&quot;);

          </code></pre></td>
</tr>
</tbody>
</table>

Before opening the RPM database, it's necessary to know where the
database resides. This information is stored in `rpmrc` files, which are
read by `rpmReadConfigFiles()`. To show that this function is really
doing its job, we retrieve the RPM database path before and after the
`rpmrc` files are read. Note that we test the return value of
`rpmGetVar(RPM_DBPATH)` and, if it is null, we insert (none) in the
`printf()` output. This prevents possible core dumps if no database path
has been set, and besides, it's more user-friendly.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>    if (rpmdbOpen(&quot;&quot;, &amp;db, O_RDONLY, 0644) != 0) {
        fprintf(stderr, &quot;cannot open /var/lib/rpm/packages.rpm\n&quot;);
        exit(1);
    }

          </code></pre></td>
</tr>
</tbody>
</table>

Here we're opening the RPM database, and doing some cursory error
checking to make sure we should continue.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>    offset = rpmdbFirstRecNum(db);

          </code></pre></td>
</tr>
</tbody>
</table>

We get the offset of the first database record…

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>    while (offset) {

        h = rpmdbGetRecord(db, offset);
        if (!h) {
                fprintf(stderr, &quot;headerRead failed\n&quot;);
        exit(1);
                }

          </code></pre></td>
</tr>
</tbody>
</table>

Here we start a **while** loop based on the record offset. As long as
there is a non-zero offset (meaning that there is still an available
record), we get the record. If there's a problem getting the record, we
exit.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>            headerGetEntry(h, RPMTAG_NAME, &amp;type, (void **) &amp;name, &amp;count);
            if (strcmp(name, argv[1]) == 0)
              headerDump(h, stdout, 1);

          </code></pre></td>
</tr>
</tbody>
</table>

Next, we get the package name entry from the record, and compare it with
the name of the package we're interested in. If it matches, we dump the
contents of the entire record.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>        headerFree(h);
    
        offset = rpmdbNextRecNum(db, offset);
    }

          </code></pre></td>
</tr>
</tbody>
</table>

At the end of the loop, we free the record, and get the offset to the
next record.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>
    rpmdbClose(db);
}

          </code></pre></td>
</tr>
</tbody>
</table>

At the end, we close the database, and exit.

Here's the program's output, edited for brevity. Notice that the
database path changes from (null) to `/var/lib/rpm` after the `rpmrc`
files are read.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ./showdb amanda-client
The database path is: (null)
The database path is: /var/lib/rpm
Entry count: 37
Data count : 5219

             CT  TAG                  TYPE               OFSET      COUNT
Entry      : 000 (1000)NAME        STRING_TYPE        0x00000000 00000001
       Data: 000 amanda-client
Entry      : 001 (1001)VERSION     STRING_TYPE        0x0000000e 00000001
       Data: 000 2.3.0
Entry      : 002 (1002)RELEASE     STRING_TYPE        0x00000014 00000001
       Data: 000 7
Entry      : 003 (1004)SUMMARY     STRING_TYPE        0x00000016 00000001
       Data: 000 Client-side Amanda package
Entry      : 004 (1005)DESCRIPTION STRING_TYPE        0x00000031 00000001
…
Entry      : 017 (1027)FILENAMES   STRING_ARRAY_TYPE  0x00000df3 00000015
       Data: 000 /usr/doc/amanda-client-2.3.0-7
       Data: 001 /usr/doc/amanda-client-2.3.0-7/COPYRIGHT
       Data: 002 /usr/doc/amanda-client-2.3.0-7/INSTALL
       Data: 003 /usr/doc/amanda-client-2.3.0-7/README
       Data: 004 /usr/doc/amanda-client-2.3.0-7/SYSTEM.NOTES
       Data: 005 /usr/doc/amanda-client-2.3.0-7/WHATS.NEW
       Data: 006 /usr/doc/amanda-client-2.3.0-7/amanda-client.README
…
Entry      : 034 (1049)REQUIRENAME STRING_ARRAY_TYPE  0x0000141c 00000006
       Data: 000 libc.so.5
       Data: 001 libdb.so.2
       Data: 002 grep
       Data: 003 sed
       Data: 004 NetKit-B
       Data: 005 dump
…

#
          </code></pre></td>
</tr>
</tbody>
</table>

As can be seen, everything that you could possibly want to know about an
installed package is available using this method.

</div>

<div class="sect2">

## <span id="s2-rpm-rpmlib-example3">Example \#3</span>

This example is similar in function to the previous one, except that it
uses rpmlib's search functions to find the desired package record:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;errno.h&gt;
#include &lt;fcntl.h&gt;
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;
#include &lt;unistd.h&gt;
#include &lt;stdlib.h&gt;

#include &lt;rpm/rpmlib.h&gt;

          </code></pre></td>
</tr>
</tbody>
</table>

Here we include rpmlib's definitions.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>void main(int argc, char ** argv)
{
    Header h;
    int stat;
    rpmdb db;
    dbiIndexSet matches;

          </code></pre></td>
</tr>
</tbody>
</table>

Here are the storage declarations.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>    if (argc != 2) {
        fprintf(stderr, &quot;showdb2 &lt;search term&gt;\n&quot;);
        exit(1);
    }

    rpmReadConfigFiles(NULL, NULL, NULL, 0);

    if (rpmdbOpen(&quot;&quot;, &amp;db, O_RDONLY, 0644) != 0) {
        fprintf(stderr, &quot;cannot open /var/lib/rpm/packages.rpm\n&quot;);
        exit(1);
    }

          </code></pre></td>
</tr>
</tbody>
</table>

In this section, we do some argument processing, processing the `rpmrc`
files, and open the RPM database.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>    stat = rpmdbFindPackage(db, argv[1], &amp;matches);
    printf(&quot;Status is: %d\n&quot;, stat);
    if (stat == 0) {
      if (matches.count) {
        printf(&quot;Number of matches: %d\n&quot;, matches.count);
        h = rpmdbGetRecord(db, matches.recs[0].recOffset);
        if (h) headerDump(h, stdout, 1);
        headerFree(h);
        dbiFreeIndexRecord(matches);
      }
    }

          </code></pre></td>
</tr>
</tbody>
</table>

In this section we use `rpmdbFindPackage()` to search for the desired
package. After checking for successful status, the count of matching
package records is checked. If there is at least one match, the first
matching record is retrieved, and dumped. Note that there could be more
than one match. Although this example doesn't dump more than the first
matching record, it would be simple to access all matches by stepping
through the `matches.recs` array.

Once we're done with the record, we free it, as well as the list of
matching records.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>    rpmdbClose(db);
}

          </code></pre></td>
</tr>
</tbody>
</table>

The last thing we do before exiting is to close the database. Here's
some sample output from the program. Note the successful status, and the
number of matches printed before the dump:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ./showdb2 rpm
Status is: 0
Number of matches: 1
Entry count: 37
Data count : 2920

             CT  TAG                  TYPE               OFSET      COUNT
Entry      : 000 (1000)NAME        STRING_TYPE        0x00000000 00000001
       Data: 000 rpm
Entry      : 001 (1001)VERSION     STRING_TYPE        0x00000004 00000001
       Data: 000 2.2.9
Entry      : 002 (1002)RELEASE     STRING_TYPE        0x0000000a 00000001
       Data: 000 1
Entry      : 003 (1004)SUMMARY     STRING_TYPE        0x0000000c 00000001
       Data: 000 Red Hat Package Manager
…
Entry      : 034 (1049)REQUIRENAME STRING_ARRAY_TYPE  0x00000b40 00000003
       Data: 000 libz.so.1
       Data: 001 libdb.so.2
       Data: 002 libc.so.5
Entry      : 035 (1050)REQUIREVERSION STRING_ARRAY_TYPE 0x00000b5f 00000003
       Data: 000 
       Data: 001 
       Data: 002 
Entry      : 036 (1064)RPMVERSION  STRING_TYPE        0x00000b62 00000001
       Data: 000 2.2.9

#
          </code></pre></td>
</tr>
</tbody>
</table>

</div>

</div>

<div class="NAVFOOTER">

-----

|                                      |                          |                     |
| :----------------------------------- | :----------------------: | ------------------: |
| [Prev](s1-rpm-rpmlib-functions.md) |    [Home](index.md)    | [Next](p14028.md) |
| rpmlib Functions                     | [Up](ch-rpm-rpmlib.md) |          Appendixes |

</div>
