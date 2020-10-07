<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-rpmlib.md)

Chapter 21. A Guide to the RPM Library API

[Next](s1-rpm-rpmlib-example-code.md)

-----

<div class="sect1">

# <span id="s1-rpm-rpmlib-functions">rpmlib Functions</span>

There are more than sixty different functions in rpmlib. The tasks they
perform range from low-level database record traversal, to high-level
package manipulation. We've grouped the functions into different
categories for easy reference.

<div class="sect2">

## <span id="s2-rpm-rpmlib-error-handling">Error Handling</span>

The functions in this section perform rpmlib's basic error handling. All
error handling centers on the use of specific status codes. The status
codes are defined in `rpmlib.h` and are of the form
<span class="symbol">RPMERR\_`xxx`</span>, where
<span class="symbol">`xxx`</span> is the name of the error.

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmerrorcode">Return Error Code — `rpmErrorCode()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

int rpmErrorCode(void);

          </code></pre></td>
</tr>
</tbody>
</table>

This function returns the error code set by the last rpmlib function
that failed. Should only be used in an error callback function defined
by `rpmErrorSetCallBack()`.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmerrorstring">Return Error String — `rpmErrorString()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

char *rpmErrorString(void);

          </code></pre></td>
</tr>
</tbody>
</table>

This function returns the error string set by the last rpmlib function
that failed. Should only be used in an error callback function defined
by `rpmErrorSetCallBack()`.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmerrorsetcallback">Set Error CallBack Function — `rpmErrorSetCallback()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

rpmErrorCallBackType rpmErrorSetCallback(rpmErrorCallBackType);

          </code></pre></td>
</tr>
</tbody>
</table>

This function sets the current error callback function to the error
callback function passed to it. The previous error callback function is
returned.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-rpmlib-getting-package-information">Getting Package Information</span>

The following functions are used to obtain information about a package
file.

It should be noted that most information is returned in the form of a
`Header` structure. This data structure is widely used throughout
rpmlib. We will discuss more header-related functions in [the Section
called *Header
Manipulation*](s1-rpm-rpmlib-functions.md#s2-rpm-rpmlib-header-manipulation)
and [the Section called *Header Entry
Manipulation*](s1-rpm-rpmlib-functions.md#s2-rpm-rpmlib-header-entry-manipulation).

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmreadpackageinfo">Read Package Information — `rpmReadPackageInfo()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

int rpmReadPackageInfo(int fd,
                       Header * signatures,
                       Header * hdr);

          </code></pre></td>
</tr>
</tbody>
</table>

Given an open package on `fd`, read in the header and signature. This
function operates as expected with both socket and pipe file descriptors
passed as `fd`. Safe on nonseekable `fd`s. When the function returns,
`fd` is left positioned at the start of the package's archive section.

If either `signatures` or `hdr` are <span class="symbol">NULL</span>,
information for the <span class="symbol">NULL</span> parameter will not
be passed back to the caller. Otherwise, they will return the package's
signatures and header, respectively.

This function returns the following status values:

  - <span class="returnvalue">0</span> — Success.

  - <span class="returnvalue">1</span> — Bad magic numbers found in
    package.

  - <span class="returnvalue">2</span> — Other error.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmreadpackageheader">Read Package Header — `rpmReadPackageHeader()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

int rpmReadPackageHeader(int fd,
                         Header * hdr,
                         int * isSource,
                         int * major,
                         int * minor);

          </code></pre></td>
</tr>
</tbody>
</table>

Given an open package on `fd`, read in the header. This function
operates as expected with both socket and pipe file descriptors passed
as `fd`. Safe on nonseekable `fd`s. When the function returns, `fd` is
left positioned at the start of the package's archive section.

If `hdr`, `isSource`, `major`, or `minor` are
<span class="symbol">NULL</span>, information for the
<span class="symbol">NULL</span> parameter(s) will not be passed back to
the caller. Otherwise, they will return the package's header (`hdr`),
information on whether the package is a source package file or not
(`isSource`), and the package format's major and minor revision number
(`major` and `minor`, respectively).

This function returns the following status values:

  - <span class="returnvalue">0</span> — Success.

  - <span class="returnvalue">1</span> — Bad magic numbers found in
    package.

  - <span class="returnvalue">2</span> — Other error.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-rpmlib-variable-manipulation">Variable Manipulation</span>

The following functions are used to get, set, and interpret RPM's
internal variables. Variables are set according to various pieces of
system information, as well as from `rpmrc` files. They control various
aspects of RPM's operation.

The variables have symbolic names in the form
<span class="symbol">RPMVAR\_`xxx`</span>, where
<span class="symbol">`xxx`</span> is the name of the variable. All
variable names are defined in `rpmlib.h`.

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmgetvar">Return Value of RPM Variable — `rpmGetVar()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

char *rpmGetVar(int var);

          </code></pre></td>
</tr>
</tbody>
</table>

This function returns the value of the variable specified in `var`.

On error, the function returns <span class="symbol">NULL</span>.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmgetbooleanvar">Return Boolean Value Of RPM Variable — `rpmGetBooleanVar()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

int rpmGetBooleanVar(int var);

          </code></pre></td>
</tr>
</tbody>
</table>

This function looks up the variable specified in `var` and returns a
<span class="returnvalue">0</span> or <span class="returnvalue">1</span>
depending on the variable's value.

On error, the function returns <span class="returnvalue">0</span>.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmsetvar">Set Value Of RPM Variable — `rpmSetVar()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

void rpmSetVar(int var,
               char *val);

          </code></pre></td>
</tr>
</tbody>
</table>

This function sets the variable specified in `var` to the value passed
in `val`. It is also possible for `val` to be
<span class="symbol">NULL</span>.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-rpmlib-rpmrc-related-info">`rpmrc`-Related Information</span>

The functions in this section are all related to `rpmrc` information —
the `rpmrc` files as well as the variables set from those files. This
information also includes the architecture and operating system
information based on `rpmrc` file entries.

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmreadconfigfiles">Read `rpmrc` Files — `rpmReadConfigFiles()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

int rpmReadConfigFiles(char * file,
                       char * arch,
                       char * os,
                       int building);

          </code></pre></td>
</tr>
</tbody>
</table>

This function reads `rpmrc` files according to the following rules:

  - Always read `/usr/lib/rpmrc`.

  - If `file` is specified, read it.

  - If `file` is not specified, read `/etc/rpmrc` and `~/.rpmrc`.

Every `rpmrc` file entry is used with `rpmSetVar()` to set the
appropriate RPM variable. Part of the normal `rpmrc` file processing
also includes setting the architecture and operating system variables
for the system executing this function. These default settings can be
overridden by entering architecture and/or operating system information
in `arch` and `os`, respectively. This information will still go through
the normal `rpmrc` translation process.

The `building` argument should be set to 1 only if a package is being
built when this function is called. Since most rpmlib-based applications
will probably not duplicate RPM's package building capabilities,
`building` should normally be set to 0.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmgetosname">Return Operating System Name — `rpmGetOsName()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

char *rpmGetOsName(void);

          </code></pre></td>
</tr>
</tbody>
</table>

This function returns the name of the operating system, as determined by
rpmlib's normal `rpmrc` file processing.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmgetarchname">Return Architecture Name — `rpmGetArchName()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

char *rpmGetArchName(void);

          </code></pre></td>
</tr>
</tbody>
</table>

This function returns the name of the architecture, as determined by
rpmlib's normal `rpmrc` file processing.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmshowrc">Print all `rpmrc`-Derived Variables — `rpmShowRC()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

int rpmShowRC(FILE *f);

          </code></pre></td>
</tr>
</tbody>
</table>

This function writes all variable names and their values to the file
`f`. Always returns <span class="returnvalue">0</span>.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmarchscore">Return Architecture Compatibility Score — `rpmArchScore()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

int rpmArchScore(char * arch);

          </code></pre></td>
</tr>
</tbody>
</table>

This function returns the "distance" between the architecture whose name
is specified in `arch`, and the current architecture. Returns
<span class="returnvalue">0</span> if the two architectures are
incompatible. The smaller the number returned, the more compatible the
two architectures are.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmosscore">Return Operating System Compatibility Score — `rpmOsScore()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

int rpmOsScore(char * os);

          </code></pre></td>
</tr>
</tbody>
</table>

This function returns the "distance" between the operating system whose
name is specified in `os`, and the current operating system. Returns
<span class="returnvalue">0</span> if the two operating systems are
incompatible. The smaller the number returned, the more compatible the
two operating systems are.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-rpmlib-rpm-database-manipulation">RPM Database Manipulation</span>

The functions in this section perform the basic operations on the RPM
database. This includes opening and closing the database, as well as
creating the database. A function also exists to rebuild a database that
has been corrupted.

Every function that accesses the RPM database in some fashion makes use
of the `rpmdb` structure. This structure is used as a handle to refer to
a particular RPM database.

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdbopen">Open RPM Database — `rpmdbOpen()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

int rpmdbOpen(char * root,
               rpmdb * dbp,
               int mode,
               int perms);

          </code></pre></td>
</tr>
</tbody>
</table>

This function opens the RPM database located in
<span class="symbol">RPMVAR\_DBPATH</span>, returning the `rpmdb`
structure `dbp`. If `root` is specified, it is prepended to
<span class="symbol">RPMVAR\_DBPATH</span> prior to opening. The `mode`
and `perms` parameters are identical to `open(2)`'s `flags` and `mode`
parameters, respectively.

The function returns <span class="returnvalue">1</span> on error,
<span class="returnvalue">0</span> on success.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdbclose">Close RPM Database — `rpmdbClose()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

void rpmdbClose(rpmdb db);

          </code></pre></td>
</tr>
</tbody>
</table>

This function closes the RPM database specified by the `rpmdb` structure
`db`. The `db` structure is also freed.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdbinit">Create RPM Database — `rpmdbInit()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

int rpmdbInit(char * root,
              int perms);

          </code></pre></td>
</tr>
</tbody>
</table>

This function creates a new RPM database to be located in
<span class="symbol">RPMVAR\_DBPATH</span>. If the database already
exists, it is left unchanged. If `root` is specified, it is prepended to
<span class="symbol">RPMVAR\_DBPATH</span> prior to creation. The
`perms` parameter is identical to `open(2)`'s `mode` parameter.

The function returns <span class="returnvalue">1</span> on error,
<span class="returnvalue">0</span> on success.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdbrebuild">Rebuild RPM Database — `rpmdbRebuild()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

int rpmdbRebuild(char * root);

          </code></pre></td>
</tr>
</tbody>
</table>

This function rebuilds the RPM database located in
<span class="symbol">RPMVAR\_DBPATH</span>. If `root` is specified, it
is prepended to <span class="symbol">RPMVAR\_DBPATH</span> prior to
rebuilding.

The function returns <span class="returnvalue">1</span> on error,
<span class="returnvalue">0</span> on success.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-rpmlib-rpm-database-traversal">RPM Database Traversal</span>

The following functions are used to traverse the RPM database. Also
described in this section is a function to retrieve a database record by
its record number.

It should be noted that database records are returned in the form of a
`Header` structure. This data structure is widely used throughout
rpmlib. We will discuss more header-related functions in [the Section
called *Header
Manipulation*](s1-rpm-rpmlib-functions.md#s2-rpm-rpmlib-header-manipulation)
and [the Section called *Header Entry
Manipulation*](s1-rpm-rpmlib-functions.md#s2-rpm-rpmlib-header-entry-manipulation).

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdbfirstrecnum">Begin RPM Database Traversal — `rpmdbFirstRecNum()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

unsigned int rpmdbFirstRecNum(rpmdb db);

          </code></pre></td>
</tr>
</tbody>
</table>

This function returns the record number of the first record in the
database specified by `db`.

On error, it returns <span class="returnvalue">0</span>.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdbnextrecnum">Traverse To Next RPM Database Record — `rpmdbNextRecNum()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

unsigned int rpmdbNextRecNum(rpmdb db,
                             unsigned int lastOffset);  

          </code></pre></td>
</tr>
</tbody>
</table>

This function returns the record number of the record following the
record number passed in `lastOffset`, in the database specified by `db`.

On error, this function returns <span class="returnvalue">0</span>.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdbgetrecord">Return Record From RPM Database — `rpmdbGetRecord()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

Header rpmdbGetRecord(rpmdb db,
                      unsigned int offset);

          </code></pre></td>
</tr>
</tbody>
</table>

This function returns the record at the record number specified by
`offset` from the database specified by `db`.

This function returns <span class="symbol">NULL</span> on error.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-rpmlib-rpm-database-search">RPM Database Search</span>

The functions in this section search the various parts of the RPM
database. They all return a structure of type `dbiIndexSet`, which
contains the records that match the search term. Here is the definition
of the structure, as found in `<rpm/dbindex.h>`:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>typedef struct {
    dbiIndexRecord * recs;
    int count;
} dbiIndexSet;

          </code></pre></td>
</tr>
</tbody>
</table>

Each `dbiIndexRecord` is also defined in `<rpm/dbindex.h>` as follows:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>typedef struct {
    unsigned int recOffset;
    unsigned int fileNumber;
} dbiIndexRecord;

          </code></pre></td>
</tr>
</tbody>
</table>

The `recOffset` element is the offset of the record from the start of
the database file. The `fileNumber` element is only used by
`rpmdbFindByFile()`.

Keep in mind that the `rpmdbFindxxx` search functions each return
`dbiIndexSet` structures, which must be freed with
`dbiFreeIndexRecord()` when no longer needed.

<div class="sect3">

### <span id="s3-rpm-rpmlib-dbifreeindexrecord">Free Database Index — `dbiFreeIndexRecord()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/dbindex.h&gt;

void dbiFreeIndexRecord(dbiIndexSet set);

          </code></pre></td>
</tr>
</tbody>
</table>

This function frees the database index set specified by `set`.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdbfindbyfile">Search RPM Database By File — `rpmdbFindByFile()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/dbindex.h&gt;

int rpmdbFindByFile(rpmdb db,
                    char * filespec,
                    dbiIndexSet * matches);

          </code></pre></td>
</tr>
</tbody>
</table>

This function searches the RPM database specified by `db` for the
package which owns the file specified by `filespec`. It returns matching
records in `matches`.

This function returns the following status values:

  - <span class="returnvalue">-1</span> — An error occurred reading a
    database record.

  - <span class="returnvalue">0</span> — The search completed normally.

  - <span class="returnvalue">1</span> — The search term was not found.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdbyfindbygroup">Search RPM Database By Group — `rpmdbFindByGroup()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/dbindex.h&gt;

int rpmdbFindByGroup(rpmdb db,
                     char * group,
                     dbiIndexSet * matches);

          </code></pre></td>
</tr>
</tbody>
</table>

This function searches the RPM database specified by `db` for the
packages which are members of the group specified by `group`. It returns
matching records in `matches`.

This function returns the following status values:

  - <span class="returnvalue">-1</span> — An error occurred reading a
    database record.

  - <span class="returnvalue">0</span> — The search completed normally.

  - <span class="returnvalue">1</span> — The search term was not found.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdbfindpackage">Search RPM Database By Package — `rpmdbFindPackage()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/dbindex.h&gt;

int rpmdbFindPackage(rpmdb db,
                     char * name,
                     dbiIndexSet * matches);

          </code></pre></td>
</tr>
</tbody>
</table>

This function searches the RPM database specified by `db` for the
packages with the package name (not label) specified by `name`. It
returns matching records in `matches`.

This function returns the following status values:

  - <span class="returnvalue">-1</span> — An error occurred reading a
    database record.

  - <span class="returnvalue">0</span> — The search completed normally.

  - <span class="returnvalue">1</span> — The search term was not found.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdbfindbyprovides">Search RPM Database By Provides — `rpmdbFindByProvides()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/dbindex.h&gt;

int rpmdbFindByProvides(rpmdb db,
                        char * provides,
                        dbiIndexSet * matches);

          </code></pre></td>
</tr>
</tbody>
</table>

This function searches the RPM database specified by `db` for the
packages which provide the provides information specified by `provides`.
It returns matching records in `matches`.

This function returns the following status values:

  - <span class="returnvalue">-1</span> — An error occurred reading a
    database record.

  - <span class="returnvalue">0</span> — The search completed normally.

  - <span class="returnvalue">1</span> — The search term was not found.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdbfindbyrequiredby">Search RPM Database By Requires — `rpmdbFindByRequiredBy()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/dbindex.h&gt;

int rpmdbFindByRequiredBy(rpmdb db,
                          char * requires,
                          dbiIndexSet * matches);

          </code></pre></td>
</tr>
</tbody>
</table>

This function searches the RPM database specified by `db` for the
packages which require the requires information specified by `requires`.
It returns matching records in `matches`.

This function returns the following status values:

  - <span class="returnvalue">-1</span> — An error occurred reading a
    database record.

  - <span class="returnvalue">0</span> — The search completed normally.

  - <span class="returnvalue">1</span> — The search term was not found.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdbfindbyconflicts">Search RPM Database By Conflicts — `rpmdbFindByConflicts()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/dbindex.h&gt;

int rpmdbFindByConflicts(rpmdb db,
                         char * conflicts,
                         dbiIndexSet * matches);

          </code></pre></td>
</tr>
</tbody>
</table>

This function searches the RPM database specified by `db` for the
packages which conflict with the conflicts information specified by
`conflicts`. It returns matching records in `matches`.

This function returns the following status values:

  - <span class="returnvalue">-1</span> — An error occurred reading a
    database record.

  - <span class="returnvalue">0</span> — The search completed normally.

  - <span class="returnvalue">1</span> — The search term was not found.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-rpmlib-package-manipulation">Package Manipulation</span>

These functions perform the operations most RPM users are familiar with.
Functions that install and erase packages are here, along with a few
related lower-level support functions.

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpminstallsourcepackage">Install Source Package File — `rpmInstallSourcePackage()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

int rpmInstallSourcePackage(char * root,
                            int fd,
                            char ** specFile,
                            rpmNotifyFunction notify,
                            char * labelFormat);

          </code></pre></td>
</tr>
</tbody>
</table>

This function installs the source package file specified by `fd`. If
`root` is not <span class="symbol">NULL</span>, it is prepended to the
variables <span class="symbol">RPMVAR\_SOURCEDIR</span> and
<span class="symbol">RPMVAR\_SPECDIR</span> prior to the actual
installation. If `specFile` is not <span class="symbol">NULL</span>, the
complete path and filename of the just-installed spec file is returned.

The `notify` parameter is used to specify a progress-tracking function
that will be called during the installation. Please refer to [the
Section called *Track Package Installation Progress —
`rpmNotifyFunction()`*](s1-rpm-rpmlib-functions.md#s3-rpm-rpmlib-rpmnotifyfunction)
for more information on this parameter.

The `labelFormat` parameter can be used to specify how the package label
should be formatted. It is used when printing the package label once the
package install is ready to proceed. If `labelformat` is
<span class="symbol">NULL</span>, the package label is not printed.

This function returns the following status values:

  - <span class="returnvalue">0</span> — The source package was
    installed successfully.

  - <span class="returnvalue">1</span> — The source package file
    contained incorrect magic numbers.

  - <span class="returnvalue">2</span> — Another type of error occurred.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpminstallpackage">Install Binary Package File — `rpmInstallPackage()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

int rpmInstallPackage(char * rootdir,
                      rpmdb db,
                      int fd,
                      char * prefix, 
                      int flags,
                      rpmNotifyFunction notify,
                      char * labelFormat,
                      char * netsharedPath);

          </code></pre></td>
</tr>
</tbody>
</table>

This function installs the binary package specified by `fd`. If a path
is specified in `rootdir`, the package will be installed with that path
acting as the root directory. If a path is specified in `prefix`, it
will be used as the prefix for relocatable packages. The RPM database
specified by `db` is updated to reflect the newly installed package.

The `flags` parameter is used to control the installation behavior. The
flags are defined in `rpmlib.h` and take the form
<span class="symbol">RPMINSTALL\_`xxx`</span>, where
<span class="symbol">`xxx`</span> is the name of the flag.

The following flags are currently defined:

  - <span class="symbol">RPMINSTALL\_REPLACEPKG</span> — Install the
    package even if it is already installed.

  - <span class="symbol">RPMINSTALL\_REPLACEFILES</span> — Install the
    package even if it will replace files owned by another package.

  - <span class="symbol">RPMINSTALL\_TEST</span> — Perform all
    install-time checks, but do not actually install the package.

  - <span class="symbol">RPMINSTALL\_UPGRADE</span> — Install the
    package, and remove all older versions of the package.

  - <span class="symbol">RPMINSTALL\_UPGRADETOOLD</span> — Install the
    package, even if the package is an older version of an
    already-installed package.

  - <span class="symbol">RPMINSTALL\_NODOCS</span> — Do not install the
    package's documentation files.

  - <span class="symbol">RPMINSTALL\_NOSCRIPTS</span> — Do not execute
    the package's install- and erase-time (in the case of an upgrade)
    scripts.

  - <span class="symbol">RPMINSTALL\_NOARCH</span> — Do not perform
    architecture compatibility tests.

  - <span class="symbol">RPMINSTALL\_NOOS</span> — Do not perform
    operating system compatibility tests.

The `notify` parameter is used to specify a progress tracking function
that will be called during the installation. Please refer to [the
Section called *Track Package Installation Progress —
`rpmNotifyFunction()`*](s1-rpm-rpmlib-functions.md#s3-rpm-rpmlib-rpmnotifyfunction)
for more information on this parameter.

The `labelFormat` parameter can be used to specify how the package label
should be formatted. This information is used when printing the package
label once the package install is ready to proceed. It is used when
printing the package label once the package install is ready to proceed.
If `labelformat` is <span class="symbol">NULL</span>, the package label
is not printed.

The `netsharedPath` parameter is used to specify that part of the local
filesystem that is shared with other systems. If there is more than one
path that is shared, the paths should be separated with a colon.

This function returns the following status values:

  - <span class="returnvalue">0</span> — The binary package was
    installed successfully.

  - <span class="returnvalue">1</span> — The binary package file
    contained incorrect magic numbers.

  - <span class="returnvalue">2</span> — Another type of error occurred.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmnotifyfunction">Track Package Installation Progress — `rpmNotifyFunction()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

typedef void (*rpmNotifyFunction)(const unsigned long amount,
                                  const unsigned long total);

          </code></pre></td>
</tr>
</tbody>
</table>

A function can be passed to `rpmInstallSourcePackage` or
`rpmInstallPackage` via the `notify` parameter. The function will be
called at regular intervals during the installation, and will have two
parameters passed to it:

1.  `amount` — The number of bytes of the install that have been
    completed so far.

2.  `total` — The total number of bytes that will be installed.

This function permits the creation of a dynamically updating progress
meter during package installation.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmremovepackage">Remove Installed Package — `rpmRemovePackage()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

int rpmRemovePackage(char * root,
                     rpmdb db,
                     unsigned int offset,
                     int flags);

          </code></pre></td>
</tr>
</tbody>
</table>

This function removes the package at record number `offset` in the RPM
database specified by `db`. If `root` is specified, it is used as the
path to a directory that will serve as the root directory while the
package is being removed.

The `flags` parameter is used to control the package removal behavior.
The flags that may be passed are defined in `rpmlib.h`, and are of the
form <span class="symbol">RPMUNINSTALL\_`xxx`</span>, where
<span class="symbol">`xxx`</span> is the name of the flag.

The following flags are currently defined:

  - <span class="symbol">RPMUNINSTALL\_TEST</span> — Perform all
    erase-time checks, but do not actually remove the package.

  - <span class="symbol">RPMUNINSTALL\_NOSCRIPTS</span> — Do not execute
    the package's erase-time scripts.

This function returns the following status values:

  - <span class="returnvalue">0</span> — The package was removed
    successfully.

  - <span class="returnvalue">1</span> — The package removal failed.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-rpmlib-package-file-verification">Package And File Verification</span>

The functions in this section perform the verification operations
necessary to ensure that the files comprising a package have not been
modified since they were installed.

Verification takes place on three distinct levels:

1.  On the file-by-file level.

2.  On a package-wide level, through the use of the **%verifyscript**
    verification script.

3.  On an inter-package level, through RPM's normal dependency
    processing.

Because of this, there are two functions to perform each specific
verification operation.

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmverifyfile">Verify File — `rpmVerifyFile()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

int rpmVerifyFile(char * root,
                  Header h,
                  int filenum,
                  int * result);

          </code></pre></td>
</tr>
</tbody>
</table>

This function verifies the `filenum`'th file from the package whose
header is `h`. If `root` is specified, it is used as the path to a
directory that will serve as the root directory while the file is being
verified. The results of the file verification are returned in `result`,
and consist of a number of flags. Each flag that is set indicates a
verification failure.

The flags are defined in `rpmlib.h`, and are of the form
<span class="symbol">RPMVERIFY\_`xxx`</span>, where
<span class="symbol">`xxx`</span> is the name of the data that failed
verification.

This function returns <span class="returnvalue">0</span> on success, and
<span class="returnvalue">1</span> on failure.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmverifyscript">Execute Package's **%verifyscript** Verification Script — `rpmVerifyScript()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

int rpmVerifyScript(char * root,
                    Header h,
                    int err);

          </code></pre></td>
</tr>
</tbody>
</table>

This function executes the **%verifyscript** verification script for the
package whose header is `h`. `err` must contain a valid file descriptor.
If `rpmIsVerbose()` returns <span class="returnvalue">true</span>, the
**%verifyscript** verification script will direct all status messages to
`err`.

This function returns <span class="returnvalue">0</span> on success,
<span class="returnvalue">1</span> on failure.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-rpmlib-dependency-related-ops">Dependency-Related Operations</span>

The functions in this section are used to perform the various
dependency-related operations supported by rpmlib.

Dependency processing is entirely separate from normal package-based
operations. The package installation and removal functions do not
perform any dependency processing themselves. Therefore, dependency
processing is somewhat different from other aspects of rpmlib's
operation.

Dependency processing centers around the `rpmDependencies` data
structure. The operations that are to be performed against the RPM
database (adding, removing, and upgrading packages) are performed
against this data structure, using functions that are described below.
These functions simply populate the data structure according to the
operation being performed. They do *not* perform the actual operation on
the package. This is an important point to keep in mind.

Once the data structure has been completely populated, a dependency
check function is called to determine if there are any
dependency-related problems. The result is a structure of dependency
conflicts. This structure, `rpmDependencyConflict`, is defined in
`rpmlib.h`.

Note that it is necessary to free both the conflicts structure *and* the
`rpmDependencies` structure when they are no longer needed. However,
`free()` should *not* be used — special functions for this are provided,
and will be discussed in this section.

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdepdependencies">Create a New Dependency Data Structure — `rpmdepDependencies()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

rpmDependencies rpmdepDependencies(rpmdb db);

          </code></pre></td>
</tr>
</tbody>
</table>

This function returns an initialized `rpmDependencies` structure. The
dependency checking to be done will be based on the RPM database
specified in the `db` parameter. If this parameter is
<span class="symbol">NULL</span>, the dependency checking will be done
as if an empty RPM database was being used.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdepaddpackage">Add a Package Install To the Dependency Data Structure — `rpmdepAddPackage()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

void rpmdepAddPackage(rpmDependencies rpmdep,
                      Header h);

          </code></pre></td>
</tr>
</tbody>
</table>

This function adds the installation of the package whose header is `h`,
to the `rpmDependencies` data structure, `rpmdep`.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdepupgradepackage">Add a Package Upgrade To the Dependency Data Structure — `rpmdepUpgradePackage()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

void rpmdepUpgradePackage(rpmDependencies rpmdep,
                          Header h);

          </code></pre></td>
</tr>
</tbody>
</table>

This function adds the upgrading of the package whose header is `h`, to
the `rpmDependencies` data structure, `rpmdep`. It is similar to
`rpmdepAddPackage()`, but older versions of the package are removed.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdepremovepackage">Add a Package Removal To the Dependency Data Structure — `rpmdepRemovePackage()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

void rpmdepRemovePackage(rpmDependencies rpmdep,
                         int dboffset);

          </code></pre></td>
</tr>
</tbody>
</table>

This function adds the removal of the package whose RPM database offset
is `dboffset`, to the `rpmDependencies` data structure, `rpmdep`.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdepavailablepackage">Add an Available Package To the Dependency Data Structure — `rpmdepAvailablePackage()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

void rpmdepAvailablePackage(rpmDependencies rpmdep,
                            Header h,
                            void * key);

          </code></pre></td>
</tr>
</tbody>
</table>

This function adds the package whose header is `h`, to the
`rpmDependencies` structure, `rpmdep`.

The `key` parameter can be anything that uniquely identifies the package
being added. It will be returned as part of the `rpmDependencyConflict`
structure returned by `rpmdepCheck()`, specifically in that structure's
`suggestedPackage` element.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdepcheck">Perform a Dependency Check — `rpmdepCheck()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

int rpmdepCheck(rpmDependencies rpmdep,
                struct rpmDependencyConflict ** conflicts,
                int * numConflicts);

          </code></pre></td>
</tr>
</tbody>
</table>

This function performs a dependency check on the `rpmDependencies`
structure `rpmdep`. It returns an array of size `numConflicts`, pointed
to by `conflicts`.

This function returns <span class="returnvalue">0</span> on success, and
<span class="returnvalue">1</span> on error.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdepfreeconflicts">Free Results of `rpmdepCheck()` — `rpmdepFreeConflicts()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

void rpmdepFreeConflicts(struct rpmDependencyConflict * conflicts,
                         int numConflicts);

          </code></pre></td>
</tr>
</tbody>
</table>

This function frees the dependency conflict information of size
`numConflicts` pointed to by `conflicts`.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmdepdone">Free a Dependency Data Structure — `rpmdepDone()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

void rpmdepDone(rpmDependencies rpmdep);

          </code></pre></td>
</tr>
</tbody>
</table>

This function frees the `rpmDependencies` structure pointed to by
`rpmdep`.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-rpmlib-diagnostic-output-control">Diagnostic Output Control</span>

The functions in this section are used to control the amount of
diagnostic output produced by other rpmlib functions. The rpmlib library
can produce a wealth of diagnostic output, making it easy to see what is
going on at any given time.

There are several different verbosity levels defined in `rpmlib.h`.
Their symbolic names are of the form
<span class="symbol">RPMMESS\_`xxx`</span>, where
<span class="symbol">`xxx`</span> is the name of the verbosity level. It
should be noted that the numeric values of the verbosity levels
*increase* with a *decrease* in verbosity.

Unless otherwise set, the default verbosity level is
<span class="symbol">RPMMESS\_NORMAL</span>.

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmincreaseverbosity">Increase Verbosity Level — `rpmIncreaseVerbosity()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

void rpmIncreaseVerbosity(void);

          </code></pre></td>
</tr>
</tbody>
</table>

This function is used to increase the current verbosity level by one.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmsetverbosity">Set Verbosity Level — `rpmSetVerbosity()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

void rpmSetVerbosity(int level);

          </code></pre></td>
</tr>
</tbody>
</table>

This function is used to set the current verbosity level to `level`.
Note that no range checking is done to `level`.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmgetverbosity">Return Verbosity Level — `rpmGetVerbosity()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

int rpmGetVerbosity(void);

          </code></pre></td>
</tr>
</tbody>
</table>

This function returns the current verbosity level.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmisverbose">Check Verbosity Level — `rpmIsVerbose()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

int rpmIsVerbose(void);

          </code></pre></td>
</tr>
</tbody>
</table>

This function checks the current verbosity level and returns
<span class="returnvalue">1</span> if the current level is set to
<span class="symbol">RPMMESS\_VERBOSE</span> or a level of higher
verbosity. Otherwise, it returns <span class="returnvalue">0</span>.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmisdebug">Check Debug Level — `rpmIsDebug()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

int rpmIsDebug(void);

          </code></pre></td>
</tr>
</tbody>
</table>

This function checks the current verbosity level and returns
<span class="returnvalue">1</span> if the current level is set to
<span class="symbol">RPMMESS\_DEBUG</span>, or a level of higher
verbosity. Otherwise, it returns <span class="returnvalue">0</span>.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-rpmlib-signature-verification">Signature Verification</span>

The functions in this section deal with the verification of package
signatures. A package file may contain more than one type of signature.
For example, a package may contain a signature that contains the
package's size, as well as a signature that contains
cryptographically-derived data that can be used to prove the package's
origin.

Each type of signature has its own tag value. These tag values are
defined in `rpmlib.h` and are of the form
<span class="symbol">RPMSIGTAG\_`xxx`</span>, where
<span class="symbol">`xxx`</span> is the type of signature.

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmverifysignature">Verify A Package File's Signature — `rpmVerifySignature()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;

int rpmVerifySignature(char *file,
                       int_32 sigTag,
                       void *sig,
                       int count,
                       char *result);

          </code></pre></td>
</tr>
</tbody>
</table>

This function verifies the signature of the package pointed to by
`file`. The result of the verification is stored in `result`, in a
format suitable for printing.

The `sigTag` parameter specifies the type of signature to be checked.
The `sig` parameter specifies the signature against which the package is
to be verified. The `count` parameter specifies the size of the
signature; at present, this parameter is only used for PGP-based
signatures.

This function returns the following values:

  - <span class="symbol">RPMSIG\_OK</span> — The signature verified
    correctly.

  - <span class="symbol">RPMSIG\_UNKNOWN</span> — The signature type is
    unknown.

  - <span class="symbol">RPMSIG\_BAD</span> — The signature did not
    verify correctly.

  - <span class="symbol">RPMSIG\_NOKEY</span> — The key required to
    check this signature is not available.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-rpmfreesignature">Free Signature Read By `rpmReadPackageInfo()` — `rpmFreeSignature()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

void rpmFreeSignature(Header h);

          </code></pre></td>
</tr>
</tbody>
</table>

This function frees the signature `h`.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-rpmlib-header-manipulation">Header Manipulation</span>

The header is one of the key data structures in rpmlib. The functions in
this section perform basic manipulations of the header.

The header is actually a data structure. It is not necessary to fully
understand the actual data structure. However, it *is* necessary to
understand the basic concepts on which the header is based.

The header serves as a kind of miniature database. The header can be
searched for specific information, which can be retrieved easily. Like a
database, the information contained in the header can be of varying
sizes.

<div class="sect3">

### <span id="s3-rpm-rpmlib-headerread">Read A Header — `headerRead()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

Header headerRead(int fd,
                  int magicp);

          </code></pre></td>
</tr>
</tbody>
</table>

This function reads a header from file `fd`, converting it from network
byte order to the host system's byte order. If `magicp` is defined to be
<span class="symbol">HEADER\_MAGIC\_YES</span>, `headerRead()` will
expect header magic numbers, and will return an error if they are not
present. Likewise, if `magicp` is defined to be
<span class="symbol">HEADER\_MAGIC\_NO</span>, `headerRead()` will not
check the header's magic numbers, and will return an error if they are
present.

On error, this function returns <span class="symbol">NULL</span>.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-headerwrite">Write A Header — `headerWrite()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

void headerWrite(int fd,
                 Header h,
                 int magicp);

          </code></pre></td>
</tr>
</tbody>
</table>

This function writes the header `h`, to file `fd`, converting it from
host byte order to network byte order. If `magicp` is defined to be
<span class="symbol">HEADER\_MAGIC\_YES</span>, `headerWrite()` will add
the appropriate magic numbers to the header being written. If `magicp`
is defined to be <span class="symbol">HEADER\_MAGIC\_NO</span>,
`headerWrite()` will not include magic numbers.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-headercopy">Copy A Header — `headerCopy()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

Header headerCopy(Header h);

          </code></pre></td>
</tr>
</tbody>
</table>

This function returns a copy of header `h`.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-headersizeof">Calculate A Header's Size — `headerSizeof()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

unsigned int headerSizeof(Header h,
                          int magicp);

          </code></pre></td>
</tr>
</tbody>
</table>

This function returns the number of bytes the header `h` takes up on
disk. Note that in versions of RPM prior to 2.3.3, this function also
changes the location of the data in the header. The result is that
pointers from `headerGetEntry()` will no longer be valid. Therefore, any
pointers acquired before calling `headerSizeof()` should be discarded.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-headernew">Create A New Header — `headerNew()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

Header headerNew(void);

          </code></pre></td>
</tr>
</tbody>
</table>

This function returns a new header.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-headerfree">Deallocate A Header — `headerFree()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

void headerFree(Header h);

          </code></pre></td>
</tr>
</tbody>
</table>

This function deallocates the header specified by `h`.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-headerdump">Print Header Structure In Human-Readable Form — `headerDump()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

void headerDump(Header h,
                FILE *f,
                int flags);

          </code></pre></td>
</tr>
</tbody>
</table>

This function prints the structure of the header `h`, to the file `f`.
If the `flags` parameter is defined to be
<span class="symbol">HEADER\_DUMP\_INLINE</span>, the header's data is
also printed.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-rpmlib-header-entry-manipulation">Header Entry Manipulation</span>

The functions in this section provide the basic operations necessary to
manipulate header entries. The following header entry types are
currently defined:

  - <span class="symbol">RPM\_NULL\_TYPE</span> — This type is not used.

  - <span class="symbol">RPM\_CHAR\_TYPE</span> — The entry contains a
    single character.

  - <span class="symbol">RPM\_INT8\_TYPE</span> — The entry contains an
    eight-bit integer.

  - <span class="symbol">RPM\_INT16\_TYPE</span> — The entry contains a
    sixteen-bit integer.

  - <span class="symbol">RPM\_INT32\_TYPE</span> — The entry contains a
    thirty-two-bit integer.

  - <span class="symbol">RPM\_INT64\_TYPE</span> — The entry contains a
    sixty-four-bit integer.

  - <span class="symbol">RPM\_STRING\_TYPE</span> — The entry contains a
    null-terminated character string.

  - <span class="symbol">RPM\_BIN\_TYPE</span> — The entry contains
    binary data that will not be interpreted by rpmlib.

  - <span class="symbol">RPM\_STRING\_ARRAY\_TYPE</span> — The entry
    contains an array of null-terminated strings.

<div class="sect3">

### <span id="s3-rpm-rpmlib-headergetentry">Get Entry From Header — `headerGetEntry()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

int headerGetEntry(Header h,
                   int_32 tag,
                   int_32 *type,
                   void **p,
                   int_32 *c);

          </code></pre></td>
</tr>
</tbody>
</table>

This function retrieves the entry matching `tag` from header `h`. The
type of the entry is returned in `type`, a pointer to the data is
returned in `p`, and the size of the data is returned in `c`. Both
`type` and `c` may be null, in which case that data will not be
returned. Note that if the entry type is
<span class="symbol">RPM\_STRING\_ARRAY\_TYPE</span>, you must issue a
`free()` on `p` when done with the data.

This function returns <span class="returnvalue">1</span> on success, and
<span class="returnvalue">0</span> on failure.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-headeraddentry">Add Entry To Header — `headerAddEntry()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

int headerAddEntry(Header h,
                   int_32 tag,
                   int_32 type,
                   void *p,
                   int_32 c);

          </code></pre></td>
</tr>
</tbody>
</table>

This function adds a new entry to the header `h`. The entry's tag is
specified by the `tag` parameter, and the entry's type is specified by
the `type` parameter.

The entry's data is pointed to by `p`, and the size of the data is
specified by `c`.

This function always returns <span class="returnvalue">1</span>.

Note: In versions of RPM prior to 2.3.3, `headerAddEntry()` will only
work successfully with headers produced by `headerCopy()` and
`headerNew()`. In particular, `headerAddEntry()` is not supported when
used to add entries to a header produced by `headerRead()`. Later
versions of RPM lift this restriction.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-headerisentry">Determine If Entry Is In Header — `headerIsEntry()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

int headerIsEntry(Header h,
                  int_32 tag);

          </code></pre></td>
</tr>
</tbody>
</table>

This function returns <span class="returnvalue">1</span> if an entry
with tag `tag` is present in header `h`. If the tag is not present, this
function returns <span class="returnvalue">0</span>.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-rpmlib-header-iterator-support">Header Iterator Support</span>

Iterators are used as a means to step from entry to entry, through an
entire header. The functions in this section are used to create, use,
and free iterators.

<div class="sect3">

### <span id="s3-rpm-rpmlib-headerinititerator">Create an Iterator — `headerInitIterator()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

HeaderIterator headerInitIterator(Header h);

          </code></pre></td>
</tr>
</tbody>
</table>

This function returns a newly-created iterator for the header `h`.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-headernextiterator">Step To the Next Entry — `headerNextIterator()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

int headerNextIterator(HeaderIterator iter,
                       int_32 *tag,
                       int_32 *type,
                       void **p,
                       int_32 *c);

          </code></pre></td>
</tr>
</tbody>
</table>

This function steps to the next entry in the header specified when the
iterator `iter` was created with `headerInitIterator()`. The next
entry's tag, type, data, and size are returned in `tag`, `type`, `p`,
and `c`, respectively. Note that if the entry type is
<span class="symbol">RPM\_STRING\_ARRAY\_TYPE</span>, you must issue a
`free()` on `p` when done with the data.

This function returns <span class="returnvalue">1</span> if successful,
and <span class="returnvalue">0</span> if there are no more entries in
the header.

</div>

<div class="sect3">

### <span id="s3-rpm-rpmlib-headerfreeiterator">Free An Iterator — `headerFreeIterator()`</span>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#include &lt;rpm/rpmlib.h&gt;
#include &lt;rpm/header.h&gt;

void headerFreeIterator(HeaderIterator iter);

          </code></pre></td>
</tr>
</tbody>
</table>

This function frees the resources used by the iterator `iter`.

</div>

</div>

</div>

<div class="NAVFOOTER">

-----

|                                |                          |                                         |
| :----------------------------- | :----------------------: | --------------------------------------: |
| [Prev](ch-rpm-rpmlib.md)     |    [Home](index.md)    | [Next](s1-rpm-rpmlib-example-code.md) |
| A Guide to the RPM Library API | [Up](ch-rpm-rpmlib.md) |                            Example Code |

</div>
