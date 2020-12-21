## Zypper

Zypper is a command line package manager for installing, updating and removing packages. It also manages repositories, can perform various queries, and is a command-line interface to ZYpp system management library (libzypp).

### Definitions

- **repository** - Local or remote directory containing packages and various package information (package metadata).
- **alias** - Repository alias is a short version of repository name for use in repository handling commands and options as removerepo or --repo.
- **package** - Software package (RPM package, source package). Patches, patterns, and products are also commonly referred to as different types of packages.
- **patch** - Represents a package or a group of packages needed to install an update (a bug fix mostly).
- **pattern** - Represents a group of packages. For example an Http Server pattern would have the dependencies defined so that all packages needed to run and manage an http server would have been triggered for installation when upon installing the pattern.
- **product** - Represents the whole product (e.g. 'openSUSE Leap 15.3').

### Usage

!!! info
    The components enclosed in brackets are not required, thus the simplest way to execute zypper is to type its name followed by a command. See zypper help for a list of general options and all commands. To get help for a specific command, type zypper help command

`# zypper [--GLOBAL-OPTIONS] <COMMAND> [--COMMAND-OPTIONS] [ARGUMENTS]`

`# zypper <SUBCOMMAND> [--COMMAND-OPTIONS] [ARGUMENTS]`

!!! note on User Prompts
    Whenever Zypper needs input from you, it lists possible answers in brackets next to the prompt text. To choose the default answer, just press &lt;enter&gt; (the default answer is printed in capitals (with the exception of non-ascii characters)). Some prompts also have help available, in which case there is a question mark '?' listed as a possible answer.

!!! tip
    To make Zypper use the default answers without user interaction, use the `--non-interactive` global option.

### Commands

!!! tip
    Zypper provides commands to be used in various manners, but it also provides abbreviated aliases of those commands. If there is an alias it will be listed below the command.

#### Repository Management
- List all defined repositories:
    * `repos`
    * `lr`
- Add a new repository:
    * `addrepo`
    * `ar `
- Remove specified repository:
    * `removerepo`
    * `rr `
- Rename specified repository:
    * `renamerepo`
    * `nr`
- Modify specified repository:
    * `modifyrepo`
    * `mr`
- Refresh all repositories:
    * `refresh`
    * `ref`
- Clean local caches:
    * `clean`
    * `cc`
#### Software Management
- Install packages:
    * `install`
    * `in`
- Remove packages:
    * `remove`
    * `rm`
- Verify integrity of package dependencies:
    * `verify`
    * `ve`
- Install source packages and their build dependencies:
    * `source-install`
    * `si`
- Install newly added packages recommended by installed packages:
    * `install-new-recommends`
    * `inr`
                            
#### Update Management
- Update installed packages with newer versions:
    * `update`
    * `up`
- List available updates:
    * `list-updates`
    * `lu`
- Install needed patches:
    * `patch`
- List available patches:
    * `list-patches`
    * `lp`
- Perform a distribution upgrade:
    * `dist-upgrade`
    * `dup`
- Check for patches:
    * `patch-check`
    * `pchk`
#### Querying
- Search for packages matching a pattern:
    * `search`
    * `se`
- Show full information for specified packages:
    * `info`
    * `if`
- Show full information for specified patches:
    * `patch-info`
- Show full information for specified patterns:
    * `pattern-info`
- Show full information for specified products:
    * `product-info`
- List all available patches:
    * `patches`
    * `pch`
- List all available packages:
    * `packages`
    * `pa`
- List all available patterns:
    * `patterns`
    * `pt`
- List all available products:
    * `products`
    * `pd`
- List packages providing specified capability:
    * `what-provides`
    * `wp`
#### Package Locks
- Add a package lock:
    * `locks`
    * `ll`
- Remove a package lock:
    * `addlock`
    * `al`
- List current package locks:
    * `removelock`
    * `rl`
- Remove useless locks:
    * `cleanlocks`
    * `cl`
#### Locale Management
- List requested locales (languages codes):
    * `locales`
    * `lloc`
- Add locale(s) to requested locales:
    * `addlocale`
    * `aloc`
- Remove locale(s) from requested locales:
    * `removelocale`
    * `rloc`
#### Other Commands
- Compare two version strings:
    * `versioncmp`
    * `vcmp`
- Print the target operating system ID string:
    * `targetos`
    * `tos`
- Print report about licenses and EULAs of installed packages:
    * `licenses`
- Download rpms specified on the commandline to a local directory:
    * `download`
- Download source rpms for all installed packages to a local directory:
    * `source-download`
- Check if the needs-reboot flag was set:
    * `needs-rebooting`
- List running processes which might still use files and libraries deleted by recent upgrades:
    * `ps`
- Remove old kernels:
    * `purge-kernels`