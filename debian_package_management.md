# Debian Package Management

In such distributions, including Kali, the Debian package is the
canonical way to make software available to end-users. Understanding
the package management system will give you a great deal of insight
into how Kali is structured, enable you to more effeectively
troubleshoot issues, and help you quickly locate help and documentation
for the wide array of tools and utilities included in Kali linux.

We will introduce the Debian package management system and introduce
`dpkg` and the APT suite of tools. One of the primary strengths
of Kali Linux lies in the flexibility of its package management
system, which leverages these tools to provide near-seamless
installation, upgrades, removal, and manipulation of application
software, and even of the base operating system itself. It is
critical that you understand how this system works to get the
most out of Kali and streamline your efforts. The days of
painful compilations, disastrous upgrades, debugging `gcc`,
`make`, and `configure` problems are long gone, however, the
number of available applications has exploded and you need to
understand the tools designed to take advantage of them. This
is also a critical skill because there are a number of security
tools that, due to licensing or other issues, cannot be included
in Kali but have Debian packages available for download. It is
important that you know how to process and install these packages
and how they impact the system, especially when things don't go
as expected.

## Intro to APT

### Relationship between APT and `dpkg`

A Debian package is a compressed archive of a software application.
A *binary package* (a `.deb` file) contains files that can be directly
used (such as programs or documentation), while a *source package*
contains the source code for the software and the instructions required
for building a binary package. A Debian package contains the application's
files as well as other *metadata* including the names of the
dependencies the application needs, as well as scripts that enable
the execution of commands at different stages in the package's
lifecycle (installation, removal, and upgrades).

The `dpkg` tool was designed to process and install `.deb` packages,
but if it encountered an unsatisfied dependency (like a missing library)
that would prevent the package from installing, `dpkg` would simply
list the missing dependency, because it had no awareness or built-in
logic to find or process the packages the might satisfy those dependencies.
The Advanced Package Tool (APT), including `apt` and `apt-get`, were
designed to address these shortcomings and could automatically
resolve these issues.

