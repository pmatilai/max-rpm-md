<div class="NAVHEADER">

Maximum RPM: Taking the RPM Package Manager to the Limit

</div>

[Prev](s1-rpm-file-format-rpm-file-format.md)

Appendix A. Format of the RPM File

[Next](s1-rpm-file-format-file-command.md)

-----

<div class="sect1">

# <span id="s1-rpm-file-format-rpm-tools">Tools For Studying RPM Files</span>

In the `tools` directory packaged with the RPM sources, are a number of
small programs that use the RPM library to extract the various sections
of a package file. Normally used by the RPM developers for debugging
purposes, these tools can also be used to make it easier to understand
the RPM package file format. Here is a list of the programs, and what
they do:

  - **rpmlead** — Extracts the lead section from a package file.

  - **rpmsignature** — Extracts the signature section from a package
    file.

  - **rpmheader** — Extracts the header from a package file.

  - **rpmarchive** — Extracts the archive from a package file.

  - **dump** — Displays a header structure in an easily readable format.

The first four programs take an RPM package file as their input. The
package file can be read either from standard input, or by including the
file name on the command line. In either case, the programs write to
standard output. Here is how **rpmlead** can be used to display the lead
from a package file:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmlead foo.rpm | od -x
0000000 abed dbee 0003 0000 0100 7072 2d6d 2e32
0000020 2e32 2d31 0031 0000 0000 0000 0000 0000
0000040 0000 0000 0000 0000 0000 0000 0000 0000
…
0000100 0000 0000 0000 0000 0000 0000 0100 0500
0000120 0004 0000 e124 bfff b36b 0800 e600 bfff
0000140

#
      </code></pre></td>
</tr>
</tbody>
</table>

Since each of these programs can also act as filters, the following
command is equivalent to the one above:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># cat foo.rpm | rpmlead | od -x
0000000 abed dbee 0003 0000 0100 7072 2d6d 2e32
0000020 2e32 2d31 0031 0000 0000 0000 0000 0000
0000040 0000 0000 0000 0000 0000 0000 0000 0000
…
0000100 0000 0000 0000 0000 0000 0000 0100 0500
0000120 0004 0000 e124 bfff b36b 0800 e600 bfff
0000140

#
      </code></pre></td>
</tr>
</tbody>
</table>

The **dump** program is used in conjunction with **rpmsignature** or
**rpmheader**. It makes decoding header structures a snap:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre class="screen"><code># rpmsignature foo.rpm | dump
Entry count: 3
Data count : 172

             CT  TAG                  TYPE             OFSET      COUNT
Entry      : 000 (1000)NAME           INT32_TYPE       0x00000000 00000001
       Data: 000 0x00044c4f (281679)
Entry      : 001 (1001)VERSION        BIN_TYPE         0x00000004 00000016
       Data: 000 b0 25 b0 97 15 97 01 32 
       Data: 008 df 35 d1 69 32 9c 53 75 
Entry      : 002 (1002)RELEASE        BIN_TYPE         0x00000014 00000152
       Data: 000 89 00 95 03 05 00 31 ed 
       Data: 008 63 90 a5 20 e8 f1 cb a2 
       Data: 016 9b f9 01 01 43 7b 04 00 
       Data: 024 9c 8e 0a d4 37 90 36 4e 
       Data: 032 df b0 9a 8a 22 b5 b0 b3 
       Data: 040 dc 30 4c 6f 91 b8 c1 50 
       Data: 048 70 4e 2c 64 d8 8a 8f ca 
       Data: 056 18 ab 5b 6f f0 41 eb c8 
       Data: 064 d1 8a 01 c9 36 01 66 f0 
       Data: 072 9d dd e9 56 31 42 61 b3 
       Data: 080 b1 da 84 94 6b ef 9c 19 
       Data: 088 45 74 c4 9f ee 17 35 e1 
       Data: 096 d1 05 fb 68 0c e6 71 5a 
       Data: 104 60 f1 c6 60 27 9f 03 06 
       Data: 112 28 ed 0b a0 08 55 9e 82 
       Data: 120 2b 1c 2e de e8 e3 50 90 
       Data: 128 62 60 0b 3c ba 04 69 a9 
       Data: 136 25 73 1b bb 5b 65 4d e1 
       Data: 144 b1 d2 c0 7f 8a fa 4a 9b 

#
      </code></pre></td>
</tr>
</tbody>
</table>

One aspect of **dump** worth noting, is that it is optimized for
decoding the header section of a package file. When used with
**rpmsignature**, it displays the tag names used in the header, instead
of the signature tag names. The data is displayed properly in either
case, however.

</div>

<div class="NAVFOOTER">

-----

|                                                 |                               |                                                    |
| :---------------------------------------------- | :---------------------------: | -------------------------------------------------: |
| [Prev](s1-rpm-file-format-rpm-file-format.md) |      [Home](index.md)       |       [Next](s1-rpm-file-format-file-command.md) |
| RPM File Format                                 | [Up](ch-rpm-file-format.md) | Identifying RPM files with the **file(1)** command |

</div>
