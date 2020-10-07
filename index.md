<div class="BOOK">

<span id="index"></span>

<div class="TITLEPAGE">

[Copyright](ln55.md) © 2000 Red Hat, Inc.

## Taking the RPM Package Manager to the Limit

### <span id="AEN31"></span>Edward C. Bailey

<div class="affiliation">

<span class="orgname">Red Hat, Inc.  
</span>

</div>

### <span id="AEN40"></span>Paul Nasrat

### <span id="AEN44"></span>Matthias Saou

### <span id="AEN48"></span>Ville Skyttä

<span class="isbn"> 1-888172-78-9  
</span>

-----

</div>

<div class="TOC">

**Table of Contents**

[Preface](ch-preface.md)

[Linux and RPM — A Brief
History](ch-preface.md#s1-preface-linux-and-rpm-history)

[Parts of the book, and who they're for](s1-preface-parts.md)

[Acknowledgements](s1-preface-acknowledgements.md)

I. [RPM and Computer Users — How to Use RPM to Effectively Manage Your
Computer](p108.md)

1\. [An Introduction to Package Management](ch-intro-to-rpm.md)

[What are Packages, and Why Manage
Them?](ch-intro-to-rpm.md#s1-intro-to-rpm-what-are-packages)

[Package Management: How to Do
It?](s1-intro-to-rpm-package-management-how.md)

[RPM Design Goals](s1-intro-to-rpm-rpm-design-goals.md)

[What's in a package?](s1-intro-to-rpm-whats-in-package.md)

[Let's Get Started](s1-intro-to-rpm-lets-get-started.md)

2\. [Using RPM to Install Packages](ch-rpm-install.md)

[**rpm -i** — What does it
do?](ch-rpm-install.md#s1-rpm-install-rpm-i-what-does-it-do)

[Performing an Install](s1-rpm-install-performing-install.md)

[Two handy options](s1-rpm-install-handy-options.md)

[Additional options to **rpm
-i**](s1-rpm-install-additional-options.md)

3\. [Using RPM to Erase Packages](ch-rpm-erase.md)

[**rpm -e** — What Does it
Do?](ch-rpm-erase.md#s1-rpm-erase-what-does-it-do)

[Erasing a Package](s1-rpm-erase-erasing-package.md)

[Additional Options](s1-rpm-erase-additional-options.md)

[**rpm -e** and Config files](s1-rpm-erase-and-config-files.md)

[Watch Out\!](s1-rpm-erase-watch-out.md)

4\. [Using RPM to Upgrade Packages](ch-rpm-upgrade.md)

[**rpm -U** — What Does it
Do?](ch-rpm-upgrade.md#s1-rpm-upgrade-what-it-does)

[Upgrading a Package](s1-rpm-upgrade-upgrading-a-package.md)

[They're Nearly Identical…](s1-rpm-upgrade-nearly-identical.md)

5\. [Getting Information About Packages](ch-rpm-query.md)

[**rpm -q** — What does it
do?](ch-rpm-query.md#s1-rpm-query-what-it-does)

[The Parts of an RPM Query](s1-rpm-query-parts.md)

[A Few Handy Queries](s1-rpm-query-handy-queries.md)

6\. [Using RPM to Verify Installed Packages](ch-rpm-verify.md)

[**rpm -V** — What Does it
Do?](ch-rpm-verify.md#s1-rpm-verify-what-it-does)

[When Verification Fails — **rpm -V** Output](s1-rpm-verify-output.md)

[Selecting What to Verify, and How](s1-rpm-verify-what-to-verify.md)

[We've Lied to You…](s1-rpm-verify-we-lied.md)

7\. [Using RPM to Verify Package Files](ch-rpm-checksig.md)

[**rpm -K** — What Does it
Do?](ch-rpm-checksig.md#s1-rpm-checksig-what-it-does)

[Configuring PGP for **rpm -K**](s1-rpm-checksig-configuring-pgp.md)

[Using **rpm -K**](s1-rpm-checksig-using-rpm-k.md)

8\. [Miscellanea](ch-rpm-miscellania.md)

[Other RPM
Options](ch-rpm-miscellania.md#s1-rpm-miscellania-other-options)

[Using **rpm2cpio**](s1-rpm-miscellania-rpm2cpio.md)

[Source Package Files and How To Use
Them](s1-rpm-miscellania-srpms.md)

II. [RPM and Developers — How to Distribute Your Software More Easily
With RPM](p5206.md)

9\. [The Philosophy Behind RPM](ch-rpm-philosophy.md)

[Pristine
Sources](ch-rpm-philosophy.md#s1-rpm-philosophy-pristine-sources)

[Easy Builds](s1-rpm-philosophy-easy-builds.md)

[Multi-architecture/operating system
Support](s1-rpm-philosophy-multi-architecture.md)

[Easier For Your Users](s1-rpm-philosophy-easier-for-users.md)

[To Summarize…](s1-rpm-philosophy-summary.md)

10\. [The Basics of Developing With RPM](ch-rpm-basics.md)

[The Inputs](ch-rpm-basics.md#s1-rpm-basics-inputs)

[The Engine: RPM](s1-rpm-basics-the-engine.md)

[The Outputs](s1-rpm-basics-outputs.md)

[And Now…](s1-rpm-basics-and-now.md)

11\. [Building Packages: A Simple Example](ch-rpm-build.md)

[Creating the Build Directory
Structure](ch-rpm-build.md#s1-rpm-build-creating-build-directories)

[Getting the Sources](s1-rpm-build-getting-sources.md)

[Creating the Spec File](s1-rpm-build-creating-spec-file.md)

[Starting the Build](s1-rpm-build-starting-build.md)

[When Things Go Wrong](s1-rpm-build-when-things-go-wrong.md)

12\. [**rpmbuild** Command Reference](ch-rpm-b-command.md)

[**rpmbuild** — What Does it
Do?](ch-rpm-b-command.md#s1-rpm-b-command-what-it-does)

[Other Build-related
Commands](s1-rpm-b-command-other-build-commands.md)

13\. [Inside the Spec File](ch-rpm-inside.md)

[Comments: Notes Ignored by
RPM](ch-rpm-inside.md#s1-rpm-inside-comments)

[Tags: Data Definitions](s1-rpm-inside-tags.md)

[Scripts: RPM's Workhorse](s1-rpm-inside-scripts.md)

[Macros: Helpful Shorthand for Package
Builders](s1-rpm-inside-macros.md)

[The **%files** List](s1-rpm-inside-files-list.md)

[Directives For the **%files**
list](s1-rpm-inside-files-list-directives.md)

[The Lone Directive: **%package**](s1-rpm-inside-package-directive.md)

[Conditionals](s1-rpm-inside-conditionals.md)

14\. [Adding Dependency Information to a Package](ch-rpm-depend.md)

[An Overview of Dependencies](ch-rpm-depend.md#s1-rpm-depend-overview)

[Automatic Dependencies](s1-rpm-depend-auto-depend.md)

[Manual Dependencies](s1-rpm-depend-manual-dependencies.md)

[To Summarize…](s1-rpm-depend-summary.md)

15\. [Making a Relocatable Package](ch-rpm-reloc.md)

[Why relocatable packages?](ch-rpm-reloc.md#s1-rpm-reloc-why)

[The **prefix** tag: Relocation Central](s1-rpm-reloc-prefix-tag.md)

[Relocatable Wrinkles: Things to Consider](s1-rpm-reloc-wrinkles.md)

[Building a Relocatable Package](s1-rpm-reloc-building-relocatable.md)

16\. [Making a Package That Can Build Anywhere](ch-rpm-anywhere.md)

[Using Build Roots in a
Package](ch-rpm-anywhere.md#s1-rpm-anywhere-using-build-roots)

[Having RPM Use a Different Build
Area](s1-rpm-anywhere-different-build-area.md)

[Specifying File
Attributes](s1-rpm-anywhere-specifying-file-attributes.md)

17\. [Adding PGP Signatures to a Package](ch-rpm-pgp.md)

[Why Sign a Package?](ch-rpm-pgp.md#s1-rpm-pgp-why-sign)

[Getting Ready to Sign](s1-rpm-pgp-getting-ready.md)

[Signing Packages](s1-rpm-pgp-signing-packages.md)

18\. [Creating Subpackages](ch-rpm-subpack.md)

[What Are
Subpackages?](ch-rpm-subpack.md#s1-rpm-subpack-what-are-they)

[Why Are They Needed?](s1-rpm-subpack-why.md)

[Our Example Spec File: Subpackages
Galore\!](s1-rpm-subpack-example-intro.md)

[Spec File Changes For
Subpackages](s1-rpm-subpack-spec-file-changes.md)

[Build-Time Scripts: Unchanged For
Subpackages](s1-rpm-subpack-build-time-scripts.md)

[Building Subpackages](s1-rpm-subpack-building-subpackages.md)

19\. [Building Packages for Multiple Architectures and Operating
Systems](ch-rpm-multi.md)

[Architectures and Operating Systems: A
Primer](ch-rpm-multi.md#s1-rpm-multi-primer)

[What Does RPM Do To Make Multi-Platform Packaging
Easier?](s1-rpm-multi-multi-platform-easier.md)

[Build and Install Platform
Detection](s1-rpm-multi-build-install-detection.md)

[**optflags** — The Other `rpmrc` File
Entry](s1-rpm-multi-optflags.md)

[Platform-Dependent Tags](s1-rpm-multi-platform-dependent-tags.md)

[Platform-Dependent
Conditionals](s1-rpm-multi-platform-dependent-conditional.md)

[Hints and Kinks](s1-rpm-multi-hints-and-kinks.md)

20\. [Real-World Package Building](ch-rpm-rw-build.md)

[An Overview of
Amanda](ch-rpm-rw-build.md#s1-rpm-rw-build-amanda-overview)

[Initial Building Without RPM](s1-rpm-rw-build-build-without-rpm.md)

[Initial Building With RPM](s1-rpm-rw-build-initial-build-with-rpm.md)

[Package Building](s1-rpm-rw-build-package-building.md)

21\. [A Guide to the RPM Library API](ch-rpm-rpmlib.md)

[An Overview of rpmlib](ch-rpm-rpmlib.md#s1-rpm-rpmlib-overview)

[rpmlib Functions](s1-rpm-rpmlib-functions.md)

[Example Code](s1-rpm-rpmlib-example-code.md)

III. [Appendixes](p14028.md)

A. [Format of the RPM File](ch-rpm-file-format.md)

[RPM File Naming
Convention](ch-rpm-file-format.md#s1-rpm-file-format-file-naming-convention)

[RPM File Format](s1-rpm-file-format-rpm-file-format.md)

[Tools For Studying RPM Files](s1-rpm-file-format-rpm-tools.md)

[Identifying RPM files with the **file(1)**
command](s1-rpm-file-format-file-command.md)

B. [The `rpmrc` File](ch-rpmrc-file.md)

[Using the **--showrc**
Option](ch-rpmrc-file.md#s1-rpmrc-file-showrc-option)

[Different Places an `rpmrc` File
Resides](s1-rpmrc-file-rpmrc-file-locations.md)

[`rpmrc` File Syntax](s1-rpmrc-file-rpmrc-file-syntax.md)

[`rpmrc` File Entries](s1-rpmrc-file-rpmrc-file-entries.md)

C. [Concise RPM Command Reference](ch-rpm-commands.md)

[Global Options](ch-rpm-commands.md#s1-rpm-commands-global-options)

[Informational Options](s1-rpm-commands-information-options.md)

[Query Mode](s1-rpm-commands-query-mode.md)

[Verify Mode](s1-rpm-commands-verify-mode.md)

[Install Mode](s1-rpm-commands-install-mode.md)

[Upgrade Mode](s1-rpm-commands-upgrade-mode.md)

[Erase Mode](s1-rpm-commands-erase-mode.md)

[Build Mode](s1-rpm-commands-build-mode.md)

[Rebuild Mode](s1-rpm-commands-rebuild-mode.md)

[Recompile Mode](s1-rpm-commands-recompile-mode.md)

[Resign Mode](s1-rpm-commands-resign-mode.md)

[Add Signature Mode](s1-rpm-commands-add-signature-mode.md)

[Check Signature Mode](s1-rpm-commands-check-signature-mode.md)

[Initialize Database
Mode](s1-rpm-commands-initialize-database-mode.md)

[Rebuild Database Mode](s1-rpm-commands-rebuild-database-mode.md)

D. [Available Tags For **--queryformat**](ch-queryformat-tags.md)

[List of **--queryformat**
Tags](ch-queryformat-tags.md#s1-queryformat-tags-queryformat-tags)

E. [Concise Spec File Reference](ch-rpm-specref.md)

[Comments](ch-rpm-specref.md#s1-rpm-specref-comments)

[The Preamble](s1-rpm-specref-preamble.md)

[Scriptlets](s1-rpm-specref-scripts.md)

[Macros](s1-rpm-specref-macros.md)

[The **%files** List](s1-rpm-specref-files-list.md)

[Directives For the **%files**
list](s1-rpm-specref-files-list-directives.md)

[**%package** Directive](s1-rpm-specref-package.md)

[Conditionals](s1-rpm-specref-conditionals.md)

F. [RPM-related Resources](ch-rpm-resources.md)

[Where to Get RPM](ch-rpm-resources.md#s1-rpm-resources-where-to-get)

[Where to Talk About RPM](s1-rpm-resources-where-to-talk.md)

[RPM On the World Wide Web](s1-rpm-resources-www-resources.md)

[RPM's License](s1-rpm-resources-license.md)

[GNU GENERAL PUBLIC LICENSE](s1-rpm-resources-gpl.md)

G. [An Introduction to PGP](ch-pgp-intro.md)

[PGP — Privacy for Regular
People](ch-pgp-intro.md#s1-pgp-intro-pgp-overview)

[Installing PGP for RPM's Use](s1-pgp-intro-installing-pgp.md)

[Index](i18625.md)

</div>

</div>

<div class="NAVFOOTER">

-----

|  |  |                         |
| :- | :-: | ----------------------: |
|  |  | [Next](ch-preface.md) |
|  |  |                 Preface |

</div>
