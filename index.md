<div class="BOOK">

<span id="index"></span>

<div class="TITLEPAGE">

[Copyright](ln55.html) © 2000 Red Hat, Inc.

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

[Preface](ch-preface.html)

[Linux and RPM — A Brief
History](ch-preface.html#s1-preface-linux-and-rpm-history)

[Parts of the book, and who they're for](s1-preface-parts.html)

[Acknowledgements](s1-preface-acknowledgements.html)

I. [RPM and Computer Users — How to Use RPM to Effectively Manage Your
Computer](p108.html)

1\. [An Introduction to Package Management](ch-intro-to-rpm.html)

[What are Packages, and Why Manage
Them?](ch-intro-to-rpm.html#s1-intro-to-rpm-what-are-packages)

[Package Management: How to Do
It?](s1-intro-to-rpm-package-management-how.html)

[RPM Design Goals](s1-intro-to-rpm-rpm-design-goals.html)

[What's in a package?](s1-intro-to-rpm-whats-in-package.html)

[Let's Get Started](s1-intro-to-rpm-lets-get-started.html)

2\. [Using RPM to Install Packages](ch-rpm-install.html)

[**rpm -i** — What does it
do?](ch-rpm-install.html#s1-rpm-install-rpm-i-what-does-it-do)

[Performing an Install](s1-rpm-install-performing-install.html)

[Two handy options](s1-rpm-install-handy-options.html)

[Additional options to **rpm
-i**](s1-rpm-install-additional-options.html)

3\. [Using RPM to Erase Packages](ch-rpm-erase.html)

[**rpm -e** — What Does it
Do?](ch-rpm-erase.html#s1-rpm-erase-what-does-it-do)

[Erasing a Package](s1-rpm-erase-erasing-package.html)

[Additional Options](s1-rpm-erase-additional-options.html)

[**rpm -e** and Config files](s1-rpm-erase-and-config-files.html)

[Watch Out\!](s1-rpm-erase-watch-out.html)

4\. [Using RPM to Upgrade Packages](ch-rpm-upgrade.html)

[**rpm -U** — What Does it
Do?](ch-rpm-upgrade.html#s1-rpm-upgrade-what-it-does)

[Upgrading a Package](s1-rpm-upgrade-upgrading-a-package.html)

[They're Nearly Identical…](s1-rpm-upgrade-nearly-identical.html)

5\. [Getting Information About Packages](ch-rpm-query.html)

[**rpm -q** — What does it
do?](ch-rpm-query.html#s1-rpm-query-what-it-does)

[The Parts of an RPM Query](s1-rpm-query-parts.html)

[A Few Handy Queries](s1-rpm-query-handy-queries.html)

6\. [Using RPM to Verify Installed Packages](ch-rpm-verify.html)

[**rpm -V** — What Does it
Do?](ch-rpm-verify.html#s1-rpm-verify-what-it-does)

[When Verification Fails — **rpm -V** Output](s1-rpm-verify-output.html)

[Selecting What to Verify, and How](s1-rpm-verify-what-to-verify.html)

[We've Lied to You…](s1-rpm-verify-we-lied.html)

7\. [Using RPM to Verify Package Files](ch-rpm-checksig.html)

[**rpm -K** — What Does it
Do?](ch-rpm-checksig.html#s1-rpm-checksig-what-it-does)

[Configuring PGP for **rpm -K**](s1-rpm-checksig-configuring-pgp.html)

[Using **rpm -K**](s1-rpm-checksig-using-rpm-k.html)

8\. [Miscellanea](ch-rpm-miscellania.html)

[Other RPM
Options](ch-rpm-miscellania.html#s1-rpm-miscellania-other-options)

[Using **rpm2cpio**](s1-rpm-miscellania-rpm2cpio.html)

[Source Package Files and How To Use
Them](s1-rpm-miscellania-srpms.html)

II. [RPM and Developers — How to Distribute Your Software More Easily
With RPM](p5206.html)

9\. [The Philosophy Behind RPM](ch-rpm-philosophy.html)

[Pristine
Sources](ch-rpm-philosophy.html#s1-rpm-philosophy-pristine-sources)

[Easy Builds](s1-rpm-philosophy-easy-builds.html)

[Multi-architecture/operating system
Support](s1-rpm-philosophy-multi-architecture.html)

[Easier For Your Users](s1-rpm-philosophy-easier-for-users.html)

[To Summarize…](s1-rpm-philosophy-summary.html)

10\. [The Basics of Developing With RPM](ch-rpm-basics.html)

[The Inputs](ch-rpm-basics.html#s1-rpm-basics-inputs)

[The Engine: RPM](s1-rpm-basics-the-engine.html)

[The Outputs](s1-rpm-basics-outputs.html)

[And Now…](s1-rpm-basics-and-now.html)

11\. [Building Packages: A Simple Example](ch-rpm-build.html)

[Creating the Build Directory
Structure](ch-rpm-build.html#s1-rpm-build-creating-build-directories)

[Getting the Sources](s1-rpm-build-getting-sources.html)

[Creating the Spec File](s1-rpm-build-creating-spec-file.html)

[Starting the Build](s1-rpm-build-starting-build.html)

[When Things Go Wrong](s1-rpm-build-when-things-go-wrong.html)

12\. [**rpmbuild** Command Reference](ch-rpm-b-command.html)

[**rpmbuild** — What Does it
Do?](ch-rpm-b-command.html#s1-rpm-b-command-what-it-does)

[Other Build-related
Commands](s1-rpm-b-command-other-build-commands.html)

13\. [Inside the Spec File](ch-rpm-inside.html)

[Comments: Notes Ignored by
RPM](ch-rpm-inside.html#s1-rpm-inside-comments)

[Tags: Data Definitions](s1-rpm-inside-tags.html)

[Scripts: RPM's Workhorse](s1-rpm-inside-scripts.html)

[Macros: Helpful Shorthand for Package
Builders](s1-rpm-inside-macros.html)

[The **%files** List](s1-rpm-inside-files-list.html)

[Directives For the **%files**
list](s1-rpm-inside-files-list-directives.html)

[The Lone Directive: **%package**](s1-rpm-inside-package-directive.html)

[Conditionals](s1-rpm-inside-conditionals.html)

14\. [Adding Dependency Information to a Package](ch-rpm-depend.html)

[An Overview of Dependencies](ch-rpm-depend.html#s1-rpm-depend-overview)

[Automatic Dependencies](s1-rpm-depend-auto-depend.html)

[Manual Dependencies](s1-rpm-depend-manual-dependencies.html)

[To Summarize…](s1-rpm-depend-summary.html)

15\. [Making a Relocatable Package](ch-rpm-reloc.html)

[Why relocatable packages?](ch-rpm-reloc.html#s1-rpm-reloc-why)

[The **prefix** tag: Relocation Central](s1-rpm-reloc-prefix-tag.html)

[Relocatable Wrinkles: Things to Consider](s1-rpm-reloc-wrinkles.html)

[Building a Relocatable Package](s1-rpm-reloc-building-relocatable.html)

16\. [Making a Package That Can Build Anywhere](ch-rpm-anywhere.html)

[Using Build Roots in a
Package](ch-rpm-anywhere.html#s1-rpm-anywhere-using-build-roots)

[Having RPM Use a Different Build
Area](s1-rpm-anywhere-different-build-area.html)

[Specifying File
Attributes](s1-rpm-anywhere-specifying-file-attributes.html)

17\. [Adding PGP Signatures to a Package](ch-rpm-pgp.html)

[Why Sign a Package?](ch-rpm-pgp.html#s1-rpm-pgp-why-sign)

[Getting Ready to Sign](s1-rpm-pgp-getting-ready.html)

[Signing Packages](s1-rpm-pgp-signing-packages.html)

18\. [Creating Subpackages](ch-rpm-subpack.html)

[What Are
Subpackages?](ch-rpm-subpack.html#s1-rpm-subpack-what-are-they)

[Why Are They Needed?](s1-rpm-subpack-why.html)

[Our Example Spec File: Subpackages
Galore\!](s1-rpm-subpack-example-intro.html)

[Spec File Changes For
Subpackages](s1-rpm-subpack-spec-file-changes.html)

[Build-Time Scripts: Unchanged For
Subpackages](s1-rpm-subpack-build-time-scripts.html)

[Building Subpackages](s1-rpm-subpack-building-subpackages.html)

19\. [Building Packages for Multiple Architectures and Operating
Systems](ch-rpm-multi.html)

[Architectures and Operating Systems: A
Primer](ch-rpm-multi.html#s1-rpm-multi-primer)

[What Does RPM Do To Make Multi-Platform Packaging
Easier?](s1-rpm-multi-multi-platform-easier.html)

[Build and Install Platform
Detection](s1-rpm-multi-build-install-detection.html)

[**optflags** — The Other `rpmrc` File
Entry](s1-rpm-multi-optflags.html)

[Platform-Dependent Tags](s1-rpm-multi-platform-dependent-tags.html)

[Platform-Dependent
Conditionals](s1-rpm-multi-platform-dependent-conditional.html)

[Hints and Kinks](s1-rpm-multi-hints-and-kinks.html)

20\. [Real-World Package Building](ch-rpm-rw-build.html)

[An Overview of
Amanda](ch-rpm-rw-build.html#s1-rpm-rw-build-amanda-overview)

[Initial Building Without RPM](s1-rpm-rw-build-build-without-rpm.html)

[Initial Building With RPM](s1-rpm-rw-build-initial-build-with-rpm.html)

[Package Building](s1-rpm-rw-build-package-building.html)

21\. [A Guide to the RPM Library API](ch-rpm-rpmlib.html)

[An Overview of rpmlib](ch-rpm-rpmlib.html#s1-rpm-rpmlib-overview)

[rpmlib Functions](s1-rpm-rpmlib-functions.html)

[Example Code](s1-rpm-rpmlib-example-code.html)

III. [Appendixes](p14028.html)

A. [Format of the RPM File](ch-rpm-file-format.html)

[RPM File Naming
Convention](ch-rpm-file-format.html#s1-rpm-file-format-file-naming-convention)

[RPM File Format](s1-rpm-file-format-rpm-file-format.html)

[Tools For Studying RPM Files](s1-rpm-file-format-rpm-tools.html)

[Identifying RPM files with the **file(1)**
command](s1-rpm-file-format-file-command.html)

B. [The `rpmrc` File](ch-rpmrc-file.html)

[Using the **--showrc**
Option](ch-rpmrc-file.html#s1-rpmrc-file-showrc-option)

[Different Places an `rpmrc` File
Resides](s1-rpmrc-file-rpmrc-file-locations.html)

[`rpmrc` File Syntax](s1-rpmrc-file-rpmrc-file-syntax.html)

[`rpmrc` File Entries](s1-rpmrc-file-rpmrc-file-entries.html)

C. [Concise RPM Command Reference](ch-rpm-commands.html)

[Global Options](ch-rpm-commands.html#s1-rpm-commands-global-options)

[Informational Options](s1-rpm-commands-information-options.html)

[Query Mode](s1-rpm-commands-query-mode.html)

[Verify Mode](s1-rpm-commands-verify-mode.html)

[Install Mode](s1-rpm-commands-install-mode.html)

[Upgrade Mode](s1-rpm-commands-upgrade-mode.html)

[Erase Mode](s1-rpm-commands-erase-mode.html)

[Build Mode](s1-rpm-commands-build-mode.html)

[Rebuild Mode](s1-rpm-commands-rebuild-mode.html)

[Recompile Mode](s1-rpm-commands-recompile-mode.html)

[Resign Mode](s1-rpm-commands-resign-mode.html)

[Add Signature Mode](s1-rpm-commands-add-signature-mode.html)

[Check Signature Mode](s1-rpm-commands-check-signature-mode.html)

[Initialize Database
Mode](s1-rpm-commands-initialize-database-mode.html)

[Rebuild Database Mode](s1-rpm-commands-rebuild-database-mode.html)

D. [Available Tags For **--queryformat**](ch-queryformat-tags.html)

[List of **--queryformat**
Tags](ch-queryformat-tags.html#s1-queryformat-tags-queryformat-tags)

E. [Concise Spec File Reference](ch-rpm-specref.html)

[Comments](ch-rpm-specref.html#s1-rpm-specref-comments)

[The Preamble](s1-rpm-specref-preamble.html)

[Scriptlets](s1-rpm-specref-scripts.html)

[Macros](s1-rpm-specref-macros.html)

[The **%files** List](s1-rpm-specref-files-list.html)

[Directives For the **%files**
list](s1-rpm-specref-files-list-directives.html)

[**%package** Directive](s1-rpm-specref-package.html)

[Conditionals](s1-rpm-specref-conditionals.html)

F. [RPM-related Resources](ch-rpm-resources.html)

[Where to Get RPM](ch-rpm-resources.html#s1-rpm-resources-where-to-get)

[Where to Talk About RPM](s1-rpm-resources-where-to-talk.html)

[RPM On the World Wide Web](s1-rpm-resources-www-resources.html)

[RPM's License](s1-rpm-resources-license.html)

[GNU GENERAL PUBLIC LICENSE](s1-rpm-resources-gpl.html)

G. [An Introduction to PGP](ch-pgp-intro.html)

[PGP — Privacy for Regular
People](ch-pgp-intro.html#s1-pgp-intro-pgp-overview)

[Installing PGP for RPM's Use](s1-pgp-intro-installing-pgp.html)

[Index](i18625.html)

</div>

</div>

<div class="NAVFOOTER">

-----

|  |  |                         |
| :- | :-: | ----------------------: |
|  |  | [Next](ch-preface.html) |
|  |  |                 Preface |

</div>
