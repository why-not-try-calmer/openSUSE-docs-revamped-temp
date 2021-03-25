## DNF package manager

`DNF` is a package manager for RPM-based distributions to install, update and remove packages. `DNF` was forked from Yum (Yellow-Dog Updater Modified) by Fedora. Among the many improvements, it uses `libdnf` in it's core and `libsolv` as a dependency resolver. `DNF` features support for plugins - which can be used to extend the core functionality of DNF -, automatic updates and parallel package downloads which makes it a great candidate to replace `Zypper` on `Tumbleweed` for instance where parallel download of packages can noticeably speed up the update process.

## Installation
On Leap 15.2 and on Tumbleweed DNF is available from the official repositories and can be installed without adding another repository.

### on the command line
```
# zypper install dnf
```

### with YaST2

Start YaST2, select "Software Management" and search for DNF. Select DNF and mark it for installation and press Accept.

## Getting DNF running
### Configuring repositories
Once the installation is finished you have no repositories configured for DNF to use. You need to symling your repository configurations from Zypp to DNF:
```
 # ln -s /etc/zypp/repos.d/ /etc/dnf/repos.d/
```
Alternatively you could also copy over repos.d, but then if you want/need to use zypper or dnf you need to make sure that repos.d is actually synced.
Make sure that there are no repos configured under /etc/yum/repos.d or you will see warnings when you're using DNF. To avoid this you can either remove the repos from /etc/yum/repos.d or just rename the repos.d folder.

## Using DNF on openSUSE
This is a list of the most used command to interact with packages using DNF.   
To refresh repos:

` # dnf makecache`

To install packages:

 `# dnf install packagename`

To search the repositories for a package type:

 `# dnf search packagename`

To remove a package:

 `# dnf remove packagename`

### Other common DNF commands include

`autoremove`: removes packages installed as dependencies that are no longer required by currently installed programs.

`check-update`: checks for updates, but does not download or install the packages.

`downgrade`: reverts to the previous version of a package.

`info`: provides basic information about the package including name, version, release, and description.

`reinstall`: reinstalls the currently installed package.

`upgrade`: checks the repositories for newer packages and updates them.

`exclude`: exclude a package from the transaction.

For example to update a Tumbleweed installation:
```
 # dnf makecache
 # dnf distro-sync
```
To update a Leap installation:
```
 # dnf makecache
 # dnf upgrade
```
This is the equivalent of: 
```
 # zypper refresh 
 # zypper dist-upgrade
```
or
```
 # zypper refresh 
 # zypper update
```

### Version locking a package
Packages can be excluded from being installed or updated by `DNF`. An interactive way of doing this is using the `--exclude=` flag: `sudo dnf update --exclude=packagename`. This can be troublesome if there are multiple packages to be excluded and simply don't want to type them all out every time the system is going through an update. To keep the list of excluded packages persisitent the `DNF` configuration file can be used:

* edit the `/etc/dnf/dnf.conf` file,
* add the packages you want to exclude the following way: `excludepkgs=packagename`.

## openSUSE flavored DNF
DNF on openSUSE also offers some aliases to mimic zypper commands out-of-the-box. These aliases can be listed:

`# dnf alias`

Which will output something like this:
```
 Alias dup='distro-sync'
 Alias dist-upgrade='distro-sync'
 Alias ref='makecache'
 Alias refresh='makecache'
 Alias ri='reinstall'
```
Using these aliases you can update the system or refresh repos like in zypper with:
```
 # dnf ref
 # dnf dup
```
which commands are the equivalent of:
```
 # zypper ref
 # zypper dup 
```
To add a new alias for example for remove:

`# dnf alias add rm=remove`

## Managing repositories
### Adding repositories
DNF in this sense is very similar to zypper in a sense that it has to be pointed to .repo file:

 `# dnf config-manager --add-repo URL`

To add a repository from OBS you would have to replace the `zypper addrepo` part with `dnf config-manager --add-repo` and the url at the end.

 `# dnf config-manager --add-repo https://download.opensuse.org/repositories/devel:languages:python/openSUSE_Tumbleweed/devel:languages:python.repo`

### Listing repositories
 `# dnf repolist`
or
 `# dnf repolist all`

These command will list the unique id and the name of the available repositories. `dnf repolist all` will add an extra status field after every repository listed indicating that the repository is enabled or disabled.

### Enable/Disable repository
Before enabling or disabling a repository it is best to list all out with their current status

 `# dnf repolist all`

Then using the `id` of the repository you wish to enable:

 `# dnf config-manager --set-enabled repo_id`

Or to disable:

 `# dnf config-manager --set-disabled repo_id`

??? info
    Other resources:
* [DNF on OBS](https://build.opensuse.org/package/show/openSUSE%3AFactory/dnf)
* [DNF reference guide](https://docs.fedoraproject.org/en-US/quick-docs/dnf/) 
* [DNF manual](https://dnf.readthedocs.io/en/latest/) 
* [DNF core plugins manual](https://dnf-plugins-core.readthedocs.io/en/latest/)
* [DNF extra plugins manual](https://dnf-plugins-extras.readthedocs.io/en/latest/) 

## Troubleshooting 
### DNF dup can't update the kernel 
Currently DNF is marking the kernel packages as protected and in case of a kernel update - which is fairly frequent on Tumbleweed - it will refuse to update. To get around the issue disable the running kernel protection:

  `# dnf --setopt=protect_running_kernel=False --refresh distro-sync `

__protect_running_kernel__: Controls whether the package corresponding to the running version of kernel is protected from removal. Default is True.
