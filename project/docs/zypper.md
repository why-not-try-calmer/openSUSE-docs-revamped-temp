## Zypper

Zypper is a command line package manager for installing, updating and removing packages. It also manages repositories, can perform various queries, and is a command-line interface to ZYpp system management library (libzypp).

??? info "Show Definitions"
    `repository`
    :   Local or remote directory containing packages and various package information (package metadata).

    `package`
    :   Software package (RPM package, source package). Patches, patterns, and products are also commonly referred to as different types of packages.

    `patch`
    :   Represents a package or a group of packages needed to install an update (a bug fix mostly).

    `pattern`
    :   Represents a group of packages. For example an Http Server pattern would have the dependencies defined so that all packages needed to run and manage an http server would have been triggered for installation when upon installing the pattern.

    `product`
    :   Represents the whole product (e.g. 'openSUSE Leap 15.3').

### What is a Package Manager?
Software is incredibly interconnected. Programming a user interface, a developer's code will specify the location on the screen
of widgets, such as buttons or text, and define how these things rearrange when a window is resized. They will not describe how
to render a button, instead they will opt to reference other code that does that for them (known as a library).
This allows for a number of benefits. Some people specialise on writing button drawing routines while others specialise on using those routines to
create feature-full applications.

Apps written for Linux use this principle a lot and it opens up a lot of opportunity. If 100 apps all use the same
library, we could install that library once and all 100 apps could use that single copy when running. This has
the following benefits:

  1. We save space! Instead of every graphical application containing the code necessary for drawing a button,
  we have that code installed once!
  2. If a library used by many is found to contain insecure code, buggy or slow code, the library can be fixed and updated
  and every app that uses it gains that improvement. We don't have to rely on developers tracking the libraries that they use
  and rebuilding their apps every time one changes.
  3. If you decide that you want to improve upon a library in some way. You can exchange the version on your computer with
  your modified version and all applications that use it will adopt your changes. This is a core principle in 
  [free software](https://en.wikipedia.org/wiki/Free_software).

However, there is a draw back. When installing software, we have to ensure that the libraries that it requires are also
installed (known as dependencies), and when removing software, we cannot remove a dependency without checking to make sure that
nothing else depends upon it. The user is also in a position where they have to track and update dependencies.

These problems are solved with a package manager. Applications, libraries and many other things can built into packages which
contains what you want install and a reference to any other packages that it depends on. The package manager is a bit
of software that downloads, installs, uninstalls and updates packages for you. If you tell the package manager to install firefox,
it will download a package containing the firefox executable, as well as a number of packages that contain the dependencies
of firefox. It also holds a database of all installed packages so it knows when it can uninstall certain dependencies without
breaking software on your computer. Zypper is the name of the package manager used by openSUSE and SUSE Enterprise Linux.



### Usage

!!! info
    The components enclosed in brackets are not required, thus the simplest way to execute zypper is to type its name followed by a command. See zypper help for a list of general options and all commands. To get help for a specific command, type zypper help command

`# zypper [--GLOBAL-OPTIONS] <COMMAND> [--COMMAND-OPTIONS] [ARGUMENTS]`

`# zypper <SUBCOMMAND> [--COMMAND-OPTIONS] [ARGUMENTS]`

!!! note on User Prompts
    Whenever Zypper needs input, it lists possible answers in brackets next to the prompt text. To choose the default answer, press &lt;__Enter__&gt; (the default answer is printed in capitals, with the exception of non-ascii characters). Some prompts also have help available, in which case there is a question mark '?' listed as a possible answer.

!!! tip
    To make Zypper use the default answers without user interaction, use the `--non-interactive` global option.

### Commands    

!!! tip
    Zypper provides commands to be used in various manners, but it also provides abbreviated aliases of those commands. If there is an alias it will be listed next to the command.

    ``# zypper`` without any commands or arguments will print a list of commands and options.

#### Repository Management
##### List all defined repositories
* `repos` or `lr`
##### Add a new repository
* `addrepo` or `ar `
##### Remove specified repository
* `removerepo` or `rr `
##### Rename specified repository
* `renamerepo` or `nr`
##### Modify specified repository
* `modifyrepo` or `mr`
##### Refresh all repositories
* `refresh` or `ref`

!!! example  
    ``$ sudo zypper ref``
##### Clean local caches
* `clean` or `cc`
#### Software Management
##### Install packages
* `install` or `in`

!!! example  
    ``$ sudo zypper in [package]``

    ``$ sudo zypper in [package1] [package2] [...]``
##### Remove packages
* `remove` or `rm`

!!! example  
    ``$ sudo zypper rm [package]``

    ``$ sudo zypper rm [package1] [package2] [...]``
##### Verify integrity of package dependencies
* `verify` or `ve`
##### Install source packages and their build dependencies
* `source-install` or `si`
##### Install newly added packages recommended by installed packages
* `install-new-recommends` or `inr`
#### Update Management
##### Update installed packages with newer versions
* `update` or `up`

!!! example
    ``$ sudo zypper up``
##### List available updates
* `list-updates` or `lu`

!!! example
    ``$ sudo zypper lu``
##### Install needed patches
* `patch`
##### List available patches
* `list-patches` or `lp`
##### Perform a distribution upgrade
* `dist-upgrade` or `dup`

!!! example
    ``$ sudo zypper dup``
##### Check for patches
* `patch-check` or `pchk`
#### Querying
##### Search for packages matching a pattern
* `search` or `se`

!!! example
    ``# zypper se [package]``

    ??? Example "Example: Search for Firefox"
        ```
        # zypper se firefox
        Loading repository data...
        Reading installed packages...

        S  | Name                               | Summary                                      | Type
        ---+------------------------------------+----------------------------------------------+--------
        i+ | MozillaFirefox                     | Mozilla Firefox Web Browser                  | package
        i+ | MozillaFirefox-branding-openSUSE   | openSUSE branding of MozillaFirefox          | package
           | MozillaFirefox-branding-upstream   | Upstream branding for Firefox                | package
           | MozillaFirefox-buildsymbols        | Breakpad buildsymbols for Firefox            | package
           | MozillaFirefox-devel               | Devel package for Firefox                    | package
           | MozillaFirefox-translations-common | Common translations for Firefox              | package
           | MozillaFirefox-translations-other  | Extra translations for Firefox               | package
           | eid-mw-firefox                     | Firefox Extension for Belgium eID Middleware | package
           | firefox-esr-branding-openSUSE      | openSUSE branding of MozillaFirefox          | package
           | firefox-uget-integrator            | Integration of uGet with Firefox             | package
        ```
##### Show full information for specified packages
* `info` or `if`

!!! example
    ``$ sudo zypper if [package]``
##### Show full information for specified patches
* `patch-info`
##### Show full information for specified patterns
* `pattern-info`
##### Show full information for specified products
* `product-info`
##### List all available patches
* `patches` or `pch`
##### List all available packages
* `packages` or `pa`
##### List all available patterns
* `patterns` or `pt`
##### List all available products
* `products` or `pd`

??? example
    ```
    # zypper pd
    Loading repository data...
    Reading installed packages...
    S  | Repository                  | Internal Name         | Name                  | Version    | Arch   | Is Base
    ---+-----------------------------+-----------------------+-----------------------+------------+--------+--------
    i+ | @System                     | openSUSE              | openSUSE Tumbleweed   | 20201216-0 | x86_64 | Yes
       | openSUSE-Tumbleweed-Non-Oss | openSUSE-Addon-NonOss | openSUSE NonOSS Addon | 20201218-0 | x86_64 | No
       | openSUSE-Tumbleweed-Non-Oss | openSUSE-Addon-NonOss | openSUSE NonOSS Addon | 20201218-0 | i586   | No
    v  | openSUSE-Tumbleweed-Oss     | openSUSE              | openSUSE Tumbleweed   | 20201218-0 | x86_64 | No
    v  | openSUSE-Tumbleweed-Oss     | openSUSE              | openSUSE Tumbleweed   | 20201218-0 | i586   | No
       | openSUSE-Tumbleweed-Oss     | MicroOS               | openSUSE MicroOS      | 20201218-0 | x86_64 | No
       | openSUSE-Tumbleweed-Oss     | MicroOS               | openSUSE MicroOS      | 20201218-0 | i586   | No
    ```
##### List packages providing specified capability
* `what-provides` or `wp`
#### Package Locks
##### Add a package lock
* `locks` or `ll`
##### Remove a package lock
* `addlock` or `al`
##### List current package locks
* `removelock` or `rl`
##### Remove useless locks
* `cleanlocks` or `cl`
#### Locale Management
##### List requested locales (languages codes)
* `locales` or `lloc`
##### Add locale(s) to requested locales
* `addlocale` or `aloc`
##### Remove locale(s) from requested locales
* `removelocale` or `rloc`
#### Other Commands
##### Get command-specific help
* `help`

!!! example
    ``$ zypper help in``
##### Compare two version strings
* `versioncmp` or `vcmp`
##### Print the target operating system ID string
* `targetos` or `tos`
##### Print report about licenses and EULAs of installed packages
* `licenses`
##### Download rpms specified on the commandline to a local directory
* `download`
##### Download source rpms for all installed packages to a local directory
* `source-download`
##### Check if the needs-reboot flag was set
* `needs-rebooting`
##### List running processes which might still use files and libraries deleted by recent upgrades
* `ps`

!!! example
    ``# zypper ps``

    ``$ sudo zypper ps``
##### Remove old kernels
* `purge-kernels`
### Global Options

??? info "Show Options"
    | Option | Description | Alias |
    | --- | --- | --- |
    | --color | Use colors in output |
    | --config &lt;FILE&gt;| Use specified config file instead of the default | -c &lt;FILE&gt; |
    | --help | Help | -h |
    | --ignore-unknown | Ignore unknown packages | -i |
    | --no-abbrev | Do not abbreviate text in tables | -A |
    | --no-color | Do no use colors in output |
    | --non-interactive | Do not ask anything, use default answers automatically | -n |
    | --promptids | Output a list of zypper's user prompts |
    | --quiet | Surpress normal output, print only error messages | -q |
    | --table-style &lt;INTEGER&gt; | Table style (0-11) | -s &lt;INTEGER&gt; |
    | --terse | Terse output for machine consumption | -t |
    | --userdata &lt;STRING&gt; | User defined transaction id used in history and plugins |
    | --verbose | Increase verbosity | -v |
    | --version | Output the version number | -V |
    | --xmlout | Switch to XML output | -x |

### Repository Options

??? info "Show Options"
    | Option | Description | Alias |
    | --- | --- | --- |
    | --disable-repositories | Do not read meta-data from repositories |
    | --gpg-auto-import-keys | Automatically trust and import new repository signing keys |
    | --no-gpg-checks | Ingore GPG check failures |
    | --no-cd | Ignore CD/DVD repositories |
    | --no-refresh | Do not refresh the repositories |
    | --no-remote | Ignore remote repositories |
    | --plus-content &lt;TAG&gt; | Additionally use disabled repositories providing a specific keyword |
    | --plus-repo &lt;URI&gt; | Use an additional repository | -p &lt;URI&gt; |
    | --releasever | Set the value of $releasever in all .repo files |

### Command Options

!!! info
    Command-specific options can be obtained using `# zypper help <COMMAND>`

??? Example
    ```
    # zypper help in
    install (in) [OPTIONS] <CAPABILITY|RPM_FILE_URI> ...

    Install packages with specified capabilities or RPM files with specified location. A capability is
    NAME[.ARCH][OP<VERSION>], where OP is one of <, <=, =, >=, >.

    Command options:

    -t, --type <TYPE>           Type of package (package, patch, pattern, product).
    -n, --name                  Select packages by plain name, not by capability. Default: false
    -C, --capability            Select packages solely by capability. Default: false
        --details               Show the detailed installation summary. Default: false
        --from <ALIAS|#|URI>    Select packages from the specified repository.
        --oldpackage            Allow to replace a newer item with an older one. Handy if you are doing
                                a rollback. Unlike --force it will not enforce a reinstall. Default:
                                false
        --allow-unsigned-rpm    Silently install unsigned rpm packages given as commandline parameters.
                                Default: false
    -f, --force                 Install even if the item is already installed (reinstall), downgraded or
                                changes vendor or architecture. Default: false
    -r, --repo <ALIAS|#|URI>    Work only with the specified repository.
    -y, --no-confirm            Don't require user interaction. Alias for the --non-interactive global
                                option. Default: false
    -D, --dry-run               Don't change anything, just report what would be done. A meaningful file
                                conflict check can only be performed if used together with
                                '--download-only'. Default: false
        --replacefiles          Install the packages even if they replace files from other, already
                                installed, packages. Default is to treat file conflicts as an error.
                                --download-as-needed disables the fileconflict check. Default: false
    -l, --auto-agree-with-licenses
                                Automatically say 'yes' to third party license confirmation prompt. See
                                'man zypper' for more details. Default: false
        --download <MODE>       Set the download-install mode. Available modes: only, in-advance,
                                in-heaps, as-needed Default: DownloadDefault
    -d, --download-only         Only download the packages, do not install.

    Solver options

        --debug-solver          Create a solver test case for debugging.
        --force-resolution      Force the solver to find a solution (even an aggressive one) rather than
                                asking.
    -R, --no-force-resolution   Do not force the solver to find a solution, let it ask.
        --solver-focus <MODE>   Set the solvers general attitude when resolving a job.
        --recommends            Install also recommended packages in addition to the required ones.
        --no-recommends         Do not install recommended packages, only required ones.

    Expert options

        --allow-downgrade
        --no-allow-downgrade    Whether to allow downgrading installed resolvables.
        --allow-name-change
        --no-allow-name-change  Whether to allow changing the names of installed resolvables.
        --allow-arch-change
        --no-allow-arch-change  Whether to allow changing the architecture of installed resolvables.
        --allow-vendor-change
        --no-allow-vendor-change
                                Whether to allow changing the vendor of installed resolvables.
    ```
