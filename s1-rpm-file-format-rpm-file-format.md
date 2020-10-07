<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](ch-rpm-file-format.md)

Appendix A. Format of the RPM File

[Next](s1-rpm-file-format-rpm-tools.md)

-----

<div class="sect1">

# <span id="s1-rpm-file-format-rpm-file-format">RPM File Format</span>

While the following details concerning the actual format of an RPM
package file were accurate at the time this was written, three points
should be kept in mind:

1.  The file format is subject to change.

2.  If a package file is to be manipulated somehow, you are *strongly*
    urged to use the appropriate rpmlib routines to access the package
    file. Why? See point number 1\!

3.  This appendix describes the most recent version of the RPM file
    format: version 3. The **file(1)** utility can be used to see a
    package's file format version.

With those caveats out of the way, let's take a look inside an RPM file…

<div class="sect2">

## <span id="s2-rpm-file-format-parts-of-rpm-file">Parts of an RPM File</span>

Every RPM package file can be divided into four distinct sections. They
are:

  - The lead.

  - The signature.

  - The header.

  - The archive.

Package files are written to disk in network byte order. If required,
RPM will automatically convert to host byte order when the package file
is read. Let's take a look at each section, starting with the lead.

</div>

<div class="sect2">

## <span id="s2-rpm-file-format-the-lead">The Lead</span>

The lead is the first part of an RPM package file. In previous versions
of RPM, it was used to store information used internally by RPM. Today,
however, the lead's sole purpose is to make it easy to identify an RPM
package file. For example, the **file(1)** command uses the lead.
[<span class="footnote">\[1\]</span>](#FTN.AEN14145) All the information
contained in the lead has been duplicated or superseded by information
contained in the header.
[<span class="footnote">\[2\]</span>](#FTN.AEN14149)

RPM defines a C structure that describes the lead:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>struct rpmlead {
    unsigned char magic[4];
    unsigned char major, minor;
    short type;
    short archnum;
    char name[66];
    short osnum;
    short signature_type;
    char reserved[16];
} ;

        </code></pre></td>
</tr>
</tbody>
</table>

Let's take a look at an actual package file and examine the various
pieces of data that make up the lead. In the following display, the
number to the left of the colon is the byte offset, in hexadecimal, from
the start of the file. The eight groups of four characters show the hex
value of the bytes in the file — two bytes per group of four characters.
Finally, the characters on the right show the ASCII values of the data
bytes. When a data byte's value results in a non-printable character, a
dot (".") is inserted instead. Here are the first thirty-two bytes of a
package file — in this case, the package file `rpm-2.2.1-1.i386.rpm`:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>00000000: edab eedb 0300 0000 0001 7270 6d2d 322e  ..........rpm-2.
00000010: 322e 312d 3100 0000 0000 0000 0000 0000  2.1-1...........

        </code></pre></td>
</tr>
</tbody>
</table>

The first four bytes (`edab eedb`) are the magic values that identify
the file as an RPM package file. Both the **file** command and RPM use
these magic numbers to determine whether a file is legitimate or not.

The next two bytes (`0300`) indicate RPM file format version. In this
case, the file's major version number is 3, and the minor version number
is 0. Versions of RPM later than 2.1 create version 3.0 package files.

The next two bytes (`0000`) determine what type of RPM file the file is.
There are presently two types defined:

  - Binary package file (type = `0000`)

  - Source package file (type = `0001`)

In this case, the file is a binary package file.

The next two bytes (`0001`) are used to store the architecture that the
package was built for. In this case, the number 1 refers to the i386
architecture. [<span class="footnote">\[3\]</span>](#FTN.AEN14176) In
the case of a source package file, these two bytes should be ignored, as
source packages are not built for a specific architecture.

The next sixty-six bytes (starting with `7270 6d2d`) contain the name of
the package. The name must end with a null byte, which leaves sixty-five
bytes for RPM's usual
<span class="symbol">name-version-release</span>-style name. In this
case, we can read the name from the right side of the output:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>rpm-2.2.1-1

        </code></pre></td>
</tr>
</tbody>
</table>

Since the name `rpm-2.2.1-1` is shorter than the sixty-five bytes
allocated for the name, the leftover bytes are filled with nulls.

Skipping past the space allocated for the name, we see two bytes
(`0001`):

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>00000040: 0000 0000 0000 0000 0000 0000 0001 0005  ................
00000050: 0400 0000 24e1 ffbf 6bb3 0008 00e6 ffbf  ....$...k.......

        </code></pre></td>
</tr>
</tbody>
</table>

These bytes represent the operating system for which this package was
built. In this case, 1 equals Linux. As with the architecture-to-number
translations, the operating system and corresponding code numbers can be
found in the file, `/usr/lib/rpmrc`.

The next two bytes (`0005`) indicate the type of signature used in the
file. A type 5 signature is new to version 3 RPM files. The signature
appears next in the file, but we need to discuss an additional detail
before exploring the signature.

</div>

<div class="sect2">

## <span id="s2-rpm-file-format-new-data-structure">Wanted: A New RPM Data Structure</span>

By looking at the C structure that defines the lead, and matching it
with the bytes in an actual package file, it's trivial to extract the
data from the lead. From a programming standpoint, it's also easy to
manipulate data in the lead — It's simply a matter of using the element
names from the structure. But there's a problem. And because of that
problem the lead is no longer used internally by RPM.

<div class="sect3">

### <span id="s3-rpm-file-format-lead-abandoned">The lead: An Abandoned Data Structure</span>

What's the problem, and why is the lead no longer used by RPM? The
answer to these questions is a single word: inflexibility. The technique
of defining a C structure to access data in a file just isn't very
flexible. Let's look at an example.

Flip back to the lead's C structure in [the Section called *The
Lead*](s1-rpm-file-format-rpm-file-format.md#s2-rpm-file-format-the-lead).
Say, for example, that some software comes along, and it has a long
name. A *very* long name. A name so long, in fact, that the 66 bytes
defined in the structure element `name` just couldn't hold it.

What can we do? Well, we could certainly change the structure such that
the `name` element would be 100 bytes long. But once a new version of
RPM is created using this new structure, we have two problems:

1.  Any package file created with the new version of RPM wouldn't be
    able to read older package formats.

2.  Any older version of RPM would be unable to install packages created
    with the newer version of RPM.

Not a very good situation\! Ideally, we would like to somehow eliminate
the requirement that the format of the data written to a package file be
engraved in granite. We should be able to do the following things, all
without losing compatibility with existing versions of RPM.

  - Add extra data to the file format.

  - Change the size of existing data.

  - Reorder the data.

Sounds like a big problem, but there's a solution…

</div>

<div class="sect3">

### <span id="s3-rpm-file-format-solution">Is There a Solution?</span>

The solution is to standardize the method by which information is
retrieved from a file. This is done by creating a well-defined data
structure that contains easily searched information about the data, and
then physically separating that information from the data.

When the data is required, it is found by using the easily searched
information, which points to the data itself. The benefits are, that the
data can be placed anywhere in the file, and that the format of the data
itself can change.

</div>

<div class="sect3">

### <span id="s3-rpm-file-format-header-structure">The Solution: The Header Structure</span>

The header structure is RPM's solution to the problem of easily
manipulating information in a standard way. The header structure's sole
purpose in life is to contain zero or more pieces of data. A file can
have more than one header structure in it. In fact, an RPM package file
has two — the signature, and the header. It was from this header that
the header structure got its name.

There are three sections to each header structure. The first section is
known as the *header structure header*. The header structure header is
used to identify the start of a header structure, its size, and the
number of data items it contains.

Following the header structure header is an area called the *index*. The
index contains one or more index entries. Each index entry contains
information about, and a pointer to, a specific data item.

After the index comes the *store*. It is in the store that the data
items are kept. The data in the store is packed together as closely as
possible. The order in which the data is stored is immaterial — a far
cry from the C structure used in the lead.

</div>

<div class="sect3">

### <span id="s3-rpm-file-format-header-structure-indepth">The Header Structure in Depth</span>

Let's take a more in-depth look at the actual format of a header
structure, starting with the header structure header:

<div class="sect4">

#### <span id="s4-rpm-file-format-header-structure-header">The Header Structure Header</span>

The header structure header always starts with a three-byte magic
number: `8e ad e8`. Following this is a one-byte version number. Next
are four bytes that are reserved for future expansion. After the
reserved bytes, there is a four-byte number that indicates how many
index entries exist in this header structure, followed by another
four-byte number indicating how many bytes of data are part of the
header structure.

</div>

<div class="sect4">

#### <span id="s4-rpm-file-format-index-entry">The Index Entry</span>

The header structure's index is made up of zero or more index entries.
Each entry is sixteen bytes longs. The first four bytes contain a *tag*
— a numeric value that identifies what type of data is pointed to by
the entry. The tag values change according to the header structure's
position in the RPM file. A list of the actual tag values, and what they
represent, will be included later in this appendix.

Following the tag, is a four-byte *type*, which is a numeric value that
describes the format of the data pointed to by the entry. The types and
their values do not change from header structure to header structure.
Here is the current list:

  - NULL = 0

  - CHAR = 1

  - INT8 = 2

  - INT16 = 3

  - INT32 = 4

  - INT64 = 5

  - STRING = 6

  - BIN = 7

  - STRING\_ARRAY = 8

A few of the data types might need some clarification. The STRING data
type is simply a null-terminated string, while the STRING\_ARRAY is a
collection of strings. Finally, the BIN data type is a collection of
binary data. This is normally used to identify data that is longer than
an INT, but not a printable STRING.

Next is a four-byte *offset* that contains the position of the data,
relative to the beginning of the store. We'll talk about the store in
just a moment.

Finally, there is a four-byte *count* that contains the number of data
items pointed to by the index entry. There are a few wrinkles to the
meaning of the count, and they center around the STRING and
STRING\_ARRAY data types. STRING data always has a count of 1, while
STRING\_ARRAY data has a count equal to the number of strings contained
in the store.

</div>

<div class="sect4">

#### <span id="s4-rpm-file-format-store">The Store</span>

The store is where the data contained in the header structure is stored.
Depending on the data type being stored, there are some details that
should be kept in mind:

  - For STRING data, each string is terminated with a null byte.

  - For INT data, each integer is stored at the natural boundary for its
    type. A 64-bit INT is stored on an 8-byte boundary, a 16-bit INT is
    stored on a 2-byte boundary, and so on.

  - All data is in network byte order.

With all these details out of the way, let's take a look at the
signature.

</div>

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-file-format-signature">The Signature</span>

The signature section follows the lead in the RPM package file. It
contains information that can be used to verify the integrity, and
optionally, the authenticity of the majority of the package file. The
signature is implemented as a header structure.

You probably noticed the word, "majority", above. The information in the
signature header structure is based on the contents of the package
file's header and archive only. The data in the lead and the signature
header structure are not included when the signature information is
created, nor are they part of any subsequent checks based on that
information.

While that omission might seem to be a weakness in RPM's design, it
really isn't. In the case of the lead, since it is used only for easy
identification of package files, any changes made to that part of the
file would, at worst, leave the file in such a state that RPM wouldn't
recognize it as a valid package file. Likewise, any changes to the
signature header structure would make it impossible to verify the file's
integrity, since the signature information would have been changed from
their original values.

<div class="sect3">

### <span id="s3-rpm-file-format-signature-analysis">Analyzing the Signature Area</span>

Using our new-found knowledge of header structures, let's take a look at
the signatures in `rpm-2.2.1-1.i386.rpm`:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>00000060: 8ead e801 0000 0000 0000 0003 0000 00ac  ................

          </code></pre></td>
</tr>
</tbody>
</table>

The first three bytes (`8ead e8`) contain the magic number for the start
of the header structure. The next byte (`01`) is the header structure's
version.

As we discussed earlier, the next four bytes (`0000 0000`) are reserved.
The four bytes after that (`0000 0003`) represent the number of index
entries in the signature section, namely, three. Following that are four
bytes (`0000 00ac`) that indicate how many bytes of data are stored in
the signature. The hex value `00ac`, when converted to decimal, means
the store is 172 bytes long.

Following the first 16 bytes is the index. Each of the three index
entries in this header structure consists of four 32-bit integers, in
the following order:

  - Tag

  - Type

  - Offset

  - Count

Let's take a look at the first index entry:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>00000070: 0000 03e8 0000 0004 0000 0000 0000 0001  ................

          </code></pre></td>
</tr>
</tbody>
</table>

The tag consists of the first four bytes (`0000 03e8`), which is 1000
when translated from hex. Looking in the RPM source directory at the
file `lib/signature.h`, we find the following tag definitions:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#define SIGTAG_SIZE         1000
#define SIGTAG_MD5          1001
#define SIGTAG_PGP          1002

          </code></pre></td>
</tr>
</tbody>
</table>

So the tag we are studying is for a size signature. Let's continue.

The next four bytes (`0000 0004`) contain the data type. As we saw
earlier, data type 4 means that the data stored for this index entry, is
a 32-bit integer. Skipping the next four bytes for a moment, the last
four bytes (`0000 0001`) are the number of 32-bit integers pointed to by
this index entry.

Now, let's go back to the four bytes prior to the count (`0000 0000`).
This number is the offset, in bytes, at which the size signature is
located. It has a value of zero, but the question is, zero bytes from
what? The answer, although it doesn't do us much good, is that the
offset is calculated from the start of the store. So first we must find
where the store begins, and we can do that by performing a simple
calculation.

First, go back to the start of the signature section. (We've made a copy
here so you won't need to flip from page to page)

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>00000060: 8ead e801 0000 0000 0000 0003 0000 00ac  ................

          </code></pre></td>
</tr>
</tbody>
</table>

After the magic, the version, and the four reserved bytes, there is the
number of index entries (`0000 0003`). Since we know that each index
entry is sixteen bytes long (four for the tag, four for the type, four
for the offset, and four for the count), we can multiply the number of
entries (3) by the number of bytes in each entry (16), and obtain the
total size of the index, which is 48 decimal, or 30 in hex. Since the
first index entry starts at hex offset 70, we can simply add hex 30 to
hex 70, and get, in hex, offset a0. So let's skip down to offset a0, and
see what's there:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>000000a0: 0004 4c4f b025 b097 1597 0132 df35 d169  ..LO.%.....2.5.i

          </code></pre></td>
</tr>
</tbody>
</table>

If we've done our math correctly, the first four bytes (`0004 4c4f`)
should represent the size of this file. Converting to decimal, this is
281,679. Let's take a look at the size of the actual file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># ls -al rpm-2.2.1-1.i386.rpm
                -rw-rw-r--   1 ed       ed         282015 Jul 21 16:05 rpm-2.2.1-1.i386.rpm

#
          </code></pre></td>
</tr>
</tbody>
</table>

Hmmm, something's not right. Or is it? It looks like we're short by 336
bytes, or in hex, 150. Interesting how that's a nice round hex number,
isn't it? For now, let's continue through the remainder of the index
entries, and see if hex 150 pops up elsewhere.

Here's the next index entry. It has a tag of decimal 1001, which is an
MD5 checksum. It is type 7, which is the BIN data type, it is 16 bytes
long, and its data starts four bytes after the beginning of the store:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>00000080: 0000 03e9 0000 0007 0000 0004 0000 0010  ................

          </code></pre></td>
</tr>
</tbody>
</table>

And here's the data. It starts with `b025` (Remember that offset of
four\!) and ends on the second line with `5375`. This is a 128-bit MD5
checksum of the package file's header and archive sections.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>000000a0: 0004 4c4f b025 b097 1597 0132 df35 d169  ..LO.%.....2.5.i
000000b0: 329c 5375 8900 9503 0500 31ed 6390 a520  2.Su......1.c.. 

          </code></pre></td>
</tr>
</tbody>
</table>

Ok, let's jump back to the last index entry:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>00000090: 0000 03ea 0000 0007 0000 0014 0000 0098  ................

          </code></pre></td>
</tr>
</tbody>
</table>

It has a tag value of `03ea` (1002 in decimal — a PGP signature block)
and is also a BIN data type. The data starts 20 decimal bytes from the
start of the data area, which would put it at file offset b4 (in hex).
It's a biggie — 152 bytes long\! Here's the data, starting with `8900`:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>000000b0: 329c 5375 8900 9503 0500 31ed 6390 a520  2.Su......1.c.. 
000000c0: e8f1 cba2 9bf9 0101 437b 0400 9c8e 0ad4  ........C{......
000000d0: 3790 364e dfb0 9a8a 22b5 b0b3 dc30 4c6f  7.6N....&quot;....0Lo
000000e0: 91b8 c150 704e 2c64 d88a 8fca 18ab 5b6f  ...PpN,d......[o
000000f0: f041 ebc8 d18a 01c9 3601 66f0 9ddd e956  .A......6.f....V
00000100: 3142 61b3 b1da 8494 6bef 9c19 4574 c49f  1Ba.....k...Et..
00000110: ee17 35e1 d105 fb68 0ce6 715a 60f1 c660  ..5....h..qZ`..`
00000120: 279f 0306 28ed 0ba0 0855 9e82 2b1c 2ede  &#39;...(....U..+...
00000130: e8e3 5090 6260 0b3c ba04 69a9 2573 1bbb  ..P.b`.&lt;..i.%s..
00000140: 5b65 4de1 b1d2 c07f 8afa 4a9b 0000 0000  [eM.......J.....

          </code></pre></td>
</tr>
</tbody>
</table>

It ends with the bytes `4a9b`. This is a 1,216-bit PGP signature block.
It is also the end of the signature section. There are four null bytes
following the last data item in order to round the size out so that it
ends on an 8-byte boundary. This means that the offset of the next
section starts at offset 150, in hex. Say, wasn't the size in the size
signature off by 150 hex? Yes, the size in the signature is the size of
the file — *less* the size of the lead and the signature sections.

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-file-format-header">The Header</span>

The header section contains all available information about the package.
Entries such as the package's name, version, and file list, are
contained in the header. Like the signature section, the header is in
header structure format. Unlike the signature, which has only three
possible tag types, the header has more than *sixty* different tags. The
list of currently defined tags appears later in this appendix on [the
Section called *Header Tag
Listing*](s1-rpm-file-format-rpm-file-format.md#s3-rpm-file-format-header-tag-listing).
Be aware that the list of tags changes frequently — the definitive list
appears in the RPM sources in `lib/rpmlib.h`.

<div class="sect3">

### <span id="s3-rpm-file-format-header-analysis">Analyzing the Header</span>

The easiest way to find the start of the header is to look for the
second header structure by scanning for its magic number (`8ead e8`).
The sixteen bytes, starting with the magic, are the header structures's
header. They follow the same format as the header in the signature's
header structure:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>00000150: 8ead e801 0000 0000 0000 0021 0000 09d3  ...........!....

          </code></pre></td>
</tr>
</tbody>
</table>

As before, the byte following the magic identifies this header structure
as being in version 1 format. Following the four reserved bytes, we find
the count of entries stored in the header (`0000 0021`). Converting to
decimal, we find that there are 33 entries in the header. The next four
bytes (`0000 09d3`) converted to decimal, tell us that there are 2,515
bytes of data in the store.

Since the header is a header structure just like the signature, we know
that the next 16 bytes are the first index entry:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>00000160: 0000 03e8 0000 0006 0000 0000 0000 0001  ................

          </code></pre></td>
</tr>
</tbody>
</table>

The first four bytes (`0000 03e8`) are the tag, which is the tag for the
package name. The next four bytes indicate the data is type 6, or a
null-terminated string. There's an offset of zero in the next four
bytes, meaning that the data for this tag is first in the store.
Finally, the last four bytes (`0000 0001`) show that the data count is
1, which is the only legal value for data of type STRING.

To find the data, we need to take the offset from the start of the first
index entry in the header (160), and add in the count of index entries
(21) multiplied by the size of an index entry (10). Doing the math (all
the values shown, are in hex, remember\!), we arrive at the offset to
the store, hex 370. Since the offset for this particular index entry is
zero, the data should start at offset 370:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>00000370: 7270 6d00 322e 322e 3100 3100 5265 6420  rpm.2.2.1.1.Red 

          </code></pre></td>
</tr>
</tbody>
</table>

Since the data type for this entry is a null-terminated string, we need
to keep reading bytes until we reach a byte whose numeric value is zero.
We find the bytes `72`, `70`, `6d`, and `00` — a null. Looking at the
ASCII display on the right, we find that the bytes form the string
`rpm`, which is the name of this package.

Now for a slightly more complicated example. Let's look at the following
index entry:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>00000250: 0000 0403 0000 0008 0000 0199 0000 0018  ................

          </code></pre></td>
</tr>
</tbody>
</table>

Tag 403 means that this entry is a list of filenames. The data type 8,
or STRING\_ARRAY, seems to bear this out. From the previous example, we
found that the data area for the header began at offset 370. Adding the
offset to the first filename (199), gives us 509. Finally, the count of
18 hex means that there should be 24 null-terminated strings containing
filenames:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>00000500: 696e 6974 6462 0a0a 002f 6269 6e2f 7270  initdb.../bin/rp
00000510: 6d00 2f65 7463 2f72 706d 7263 002f 7573  m./etc/rpmrc./us

          </code></pre></td>
</tr>
</tbody>
</table>

The byte at offset 509 is 2f — a "/". Reading up to the first null byte,
we find that the first filename is `/bin/rpm`, followed by `/etc/rpmrc`.
This continues on for 22 more filenames.

There are many more tags that we could decode, but they are all done in
the same manner.

</div>

<div class="sect3">

### <span id="s3-rpm-file-format-header-tag-listing">Header Tag Listing</span>

The following list shows the tags available, along with their defined
values, for use in the header. This list is current as of version 4.3 of
RPM. For the most up-to-date version, look in the file `lib/rpmlib.h` in
the latest version of the RPM sources.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>#define RPMTAG_NAME                     1000
#define RPMTAG_N        RPMTAG_NAME
#define RPMTAG_VERSION                  1001
#define RPMTAG_V        RPMTAG_VERSION
#define RPMTAG_RELEASE                  1002
#define RPMTAG_R        RPMTAG_RELEASE
#define RPMTAG_EPOCH                    1003
#define RPMTAG_E        RPMTAG_EPOCH
#define RPMTAG_SERIAL   RPMTAG_EPOCH         /* backward compatibility */
#define RPMTAG_SUMMARY                  1004
#define RPMTAG_DESCRIPTION              1005
#define RPMTAG_BUILDTIME                1006
#define RPMTAG_BUILDHOST                1007
#define RPMTAG_INSTALLTIME              1008
#define RPMTAG_SIZE                     1009
#define RPMTAG_DISTRIBUTION             1010
#define RPMTAG_VENDOR                   1011
#define RPMTAG_GIF                      1012
#define RPMTAG_XPM                      1013
#define RPMTAG_LICENSE                  1014
#define RPMTAG_COPYRIGHT RPMTAG_LICENSE      /* backward compatibility */
#define RPMTAG_PACKAGER                 1015
#define RPMTAG_GROUP                    1016
#define RPMTAG_CHANGELOG                1017 /* internal */
#define RPMTAG_SOURCE                   1018
#define RPMTAG_PATCH                    1019
#define RPMTAG_URL                      1020
#define RPMTAG_OS                       1021
#define RPMTAG_ARCH                     1022
#define RPMTAG_PREIN                    1023
#define RPMTAG_POSTIN                   1024
#define RPMTAG_PREUN                    1025
#define RPMTAG_POSTUN                   1026
#define RPMTAG_OLDFILENAMES             1027 /* obsolete */
#define RPMTAG_FILESIZES                1028
#define RPMTAG_FILESTATES               1029
#define RPMTAG_FILEMODES                1030
#define RPMTAG_FILEUIDS                 1031 /* internal */
#define RPMTAG_FILEGIDS                 1032 /* internal */
#define RPMTAG_FILERDEVS                1033
#define RPMTAG_FILEMTIMES               1034
#define RPMTAG_FILEMD5S                 1035
#define RPMTAG_FILELINKTOS              1036
#define RPMTAG_FILEFLAGS                1037
#define RPMTAG_ROOT                     1038 /* internal, obsolete */
#define RPMTAG_FILEUSERNAME             1039
#define RPMTAG_FILEGROUPNAME            1040
#define RPMTAG_EXCLUDE                  1041 /* internal, obsolete */
#define RPMTAG_EXCLUSIVE                1042 /* internal, obsolete */
#define RPMTAG_ICON                     1043
#define RPMTAG_SOURCERPM                1044
#define RPMTAG_FILEVERIFYFLAGS          1045
#define RPMTAG_ARCHIVESIZE              1046
#define RPMTAG_PROVIDENAME              1047
#define RPMTAG_PROVIDES RPMTAG_PROVIDENAME   /* backward compatibility */
#define RPMTAG_REQUIREFLAGS             1048
#define RPMTAG_REQUIRENAME              1049
#define RPMTAG_REQUIREVERSION           1050
#define RPMTAG_NOSOURCE                 1051 /* internal */
#define RPMTAG_NOPATCH                  1052 /* internal */
#define RPMTAG_CONFLICTFLAGS            1053
#define RPMTAG_CONFLICTNAME             1054
#define RPMTAG_CONFLICTVERSION          1055
#define RPMTAG_DEFAULTPREFIX            1056 /* internal, deprecated */
#define RPMTAG_BUILDROOT                1057 /* internal */
#define RPMTAG_INSTALLPREFIX            1058 /* internal, deprecated */
#define RPMTAG_EXCLUDEARCH              1059
#define RPMTAG_EXCLUDEOS                1060
#define RPMTAG_EXCLUSIVEARCH            1061
#define RPMTAG_EXCLUSIVEOS              1062
#define RPMTAG_AUTOREQPROV              1063 /* internal */
#define RPMTAG_RPMVERSION               1064
#define RPMTAG_TRIGGERSCRIPTS           1065
#define RPMTAG_TRIGGERNAME              1066
#define RPMTAG_TRIGGERVERSION           1067
#define RPMTAG_TRIGGERFLAGS             1068
#define RPMTAG_TRIGGERINDEX             1069
#define RPMTAG_VERIFYSCRIPT             1079
#define RPMTAG_CHANGELOGTIME            1080
#define RPMTAG_CHANGELOGNAME            1081
#define RPMTAG_CHANGELOGTEXT            1082
#define RPMTAG_BROKENMD5                1083 /* internal, obsolete */
#define RPMTAG_PREREQ                   1084 /* internal */
#define RPMTAG_PREINPROG                1085
#define RPMTAG_POSTINPROG               1086
#define RPMTAG_PREUNPROG                1087
#define RPMTAG_POSTUNPROG               1088
#define RPMTAG_BUILDARCHS               1089
#define RPMTAG_OBSOLETENAME             1090
#define RPMTAG_OBSOLETES RPMTAG_OBSOLETENAME /* backward compatibility */
#define RPMTAG_VERIFYSCRIPTPROG         1091
#define RPMTAG_TRIGGERSCRIPTPROG        1092
#define RPMTAG_DOCDIR                   1093 /* internal */
#define RPMTAG_COOKIE                   1094
#define RPMTAG_FILEDEVICES              1095
#define RPMTAG_FILEINODES               1096
#define RPMTAG_FILELANGS                1097
#define RPMTAG_PREFIXES                 1098
#define RPMTAG_INSTPREFIXES             1099
#define RPMTAG_TRIGGERIN                1100 /* internal */
#define RPMTAG_TRIGGERUN                1101 /* internal */
#define RPMTAG_TRIGGERPOSTUN            1102 /* internal */
#define RPMTAG_AUTOREQ                  1103 /* internal */
#define RPMTAG_AUTOPROV                 1104 /* internal */
#define RPMTAG_CAPABILITY               1105 /* internal, obsolete */
#define RPMTAG_SOURCEPACKAGE            1106 /* src.rpm header marker */
#define RPMTAG_OLDORIGFILENAMES         1107 /* internal, obsolete */
#define RPMTAG_BUILDPREREQ              1108 /* internal */
#define RPMTAG_BUILDREQUIRES            1109 /* internal */
#define RPMTAG_BUILDCONFLICTS           1110 /* internal */
#define RPMTAG_BUILDMACROS              1111 /* internal, unused */
#define RPMTAG_PROVIDEFLAGS             1112
#define RPMTAG_PROVIDEVERSION           1113
#define RPMTAG_OBSOLETEFLAGS            1114
#define RPMTAG_OBSOLETEVERSION          1115
#define RPMTAG_DIRINDEXES               1116
#define RPMTAG_BASENAMES                1117
#define RPMTAG_DIRNAMES                 1118
#define RPMTAG_ORIGDIRINDEXES           1119 /* internal */
#define RPMTAG_ORIGBASENAMES            1120 /* internal */
#define RPMTAG_ORIGDIRNAMES             1121 /* internal */
#define RPMTAG_OPTFLAGS                 1122
#define RPMTAG_DISTURL                  1123
#define RPMTAG_PAYLOADFORMAT            1124
#define RPMTAG_PAYLOADCOMPRESSOR        1125
#define RPMTAG_PAYLOADFLAGS             1126
#define RPMTAG_INSTALLCOLOR             1127 /* transaction color when installed */
#define RPMTAG_INSTALLTID               1128
#define RPMTAG_REMOVETID                1129
#define RPMTAG_SHA1RHN                  1130 /* internal - obsolete */
#define RPMTAG_RHNPLATFORM              1131
#define RPMTAG_PLATFORM                 1132
#define RPMTAG_PATCHESNAME              1133 /* placeholder (SuSE) */
#define RPMTAG_PATCHESFLAGS             1134 /* placeholder (SuSE) */
#define RPMTAG_PATCHESVERSION           1135 /* placeholder (SuSE) */
#define RPMTAG_CACHECTIME               1136
#define RPMTAG_CACHEPKGPATH             1137
#define RPMTAG_CACHEPKGSIZE             1138
#define RPMTAG_CACHEPKGMTIME            1139
#define RPMTAG_FILECOLORS               1140
#define RPMTAG_FILECLASS                1141
#define RPMTAG_CLASSDICT                1142
#define RPMTAG_FILEDEPENDSX             1143
#define RPMTAG_FILEDEPENDSN             1144
#define RPMTAG_DEPENDSDICT              1145
#define RPMTAG_SOURCEPKGID              1146
#define RPMTAG_FILECONTEXTS             1147
#define RPMTAG_FSCONTEXTS               1148 /* extension */
#define RPMTAG_RECONTEXTS               1149 /* extension */
#define RPMTAG_POLICIES                 1150 /* selinux *.te policy file. */

          </code></pre></td>
</tr>
</tbody>
</table>

</div>

</div>

<div class="sect2">

## <span id="s2-rpm-file-format-the-archive">The Archive</span>

Following the header section is the archive. The archive holds the
actual files that comprise the package. The archive is compressed using
GNU zip. We can verify this if we look at the start of the archive:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code>00000d40: 0000 001f 8b08 0000 0000 0002 03ec fd7b  ...............{
00000d50: 7c13 d516 388e 4e92 691b 4a20 010a 1428  |...8.N.i.J ...(

        </code></pre></td>
</tr>
</tbody>
</table>

In this example, the archive starts at offset `d43`. According to the
contents of `/usr/lib/magic`, the first two bytes of a **gzipped** file
should be `1f8b`, which is, in fact, what we see. The following byte
(`08`) is the flag used by GNU zip to indicate the file has been
compressed with **gzip**'s "deflation" method. The eighth byte has a
value of `02`, which means that the archive has been compressed using
**gzip**'s maximum compression setting. The following byte contains a
code indicating the operating system under which the archive was
compressed. A `03` in this byte indicates that the compression ran under
a UNIX-like operating system.

The remainder of the RPM package file is the compressed archive. After
the archive is uncompressed, it is an ordinary **cpio** archive in SVR4
format with a CRC checksum.

</div>

</div>

### Notes

|                                                                                         |                                                                                                                                                                                                              |
| --------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [<span class="footnote">\[1\]</span>](s1-rpm-file-format-rpm-file-format.md#AEN14145) | Please refer to [the Section called *Identifying RPM files with the **file(1)** command*](s1-rpm-file-format-file-command.md) for a discussion on identifying RPM package files with the **file** command. |
| [<span class="footnote">\[2\]</span>](s1-rpm-file-format-rpm-file-format.md#AEN14149) | The header is discussed in [the Section called *The Header*](s1-rpm-file-format-rpm-file-format.md#s2-rpm-file-format-header).                                                                             |
| [<span class="footnote">\[3\]</span>](s1-rpm-file-format-rpm-file-format.md#AEN14176) | It should be noted that the architecture used internally by RPM is actually stored in the header. This value is strictly for **file(1)**'s use.                                                              |

<div class="NAVFOOTER">

-----

|                                 |                               |                                           |
| :------------------------------ | :---------------------------: | ----------------------------------------: |
| [Prev](ch-rpm-file-format.md) |      [Home](index.md)       | [Next](s1-rpm-file-format-rpm-tools.md) |
| Format of the RPM File          | [Up](ch-rpm-file-format.md) |              Tools For Studying RPM Files |

</div>
