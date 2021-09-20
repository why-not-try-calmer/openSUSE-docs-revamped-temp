## Introduction
Tumbleweed is a rolling-release distribution. This means it gets updates on a continuous basis, usually several times a week. However, to bring some order and make it easier to manage producing and consuming updates, Tumbleweed fetches updates in batches. A single batch of updates is called a _snapshot_.

!!! info
    Tumbleweed snapshots ought not to be confused with _Btrfs snapshots_, a completely different kind of snapshots. The latter make up the different filesystem states that allow _Btrfs_ users, using tools like YaST2 and `snapper`, to control which state they want to actualize).

Tumbleweed snapshots are thus batches of updates which are tested in openQA -- openSUSE's very best testing pipeline. The tests are thorough and check not only that each upgraded package is healthy, but also that all updates in the same snapshot play nice with one another. A snapshot is thus as healthy as the individual updates it holds, and as the compatibility between one another.

It should be emphasized that by default, Tumbleweed updates across all available updates, whether or not they belong to the same snapshots. In other words, the distribution of updates into snapshots is, by default, only used for production purposes. To take advantage of it at the user level, consider the [tumbleweed-cli section below](#tumbleweed-cli).

## Updating/upgrading

Unlike other Linux distributions, such as Leap, on Tumbleweed there is no distinction between updating (on other distributions: applying updates to packages) and upgrading (on other distributions: bringing the entire operating system to its latest possible state). You update/upgrade your system with:

```
sudo zypper dup
```

What this command does is to check whether all repositories registered against the system provide packages with available updates, collect and return the results. Upon confirmation from the user (`y` in a terminal window) it will download and then install them.

!!! info
    On Tumbleweed you will never have to use **zypper-up**, which is how you would update packages on Leap. The reason is that since Tumbleweed is a rolling-release distribution, updating it means not just updating an arbitrary collection of packages; it also means using the updated packages as _the new snapshot_ which Tumbleweed is to realize. In addition to this **zypper-dup** enjoys some extra power over **zypper-up**, being able to not only edit packages, but to remove them when they are no longer required.

It may occur that some repositories, when registered against the system, have not been configured to automatically look for new available updates during a `zypper-dup`. For this reason it might be useful to accompany this command with another one forcing all repositories to check for new updates, resulting in the pair:

`sudo zypper ref && sudo zypper dup`

That said, you can ensure that all repositories have been properly configured by listing them and, where applicable, by enabling their autorefresh feature. From the list of repositories obtained from:

`zypper lr`

identify repositories whose autorefresh value you want to tweak. For example:

```
#  | Alias                            | Name                | Enab-> | GPG Check        | Refresh
---+----------------------------------+---------------------+--------+-------------+-------------
 2 | brave-browser-beta               | brave-browser-beta  | Yes    | (r ) Yes         | No
```

shows that our `brave-browser-beta` repository has its refresh feature disabled. We can now set it to `enabled` with:

`sudo zypper mr -f 2`

which is a compact instance of 

`sudo zypper modifyrepo --refresh <repo identifier>` 

## Rolling back

On Tumbleweed _rolling back_ means changing the on-disk state of your system to a previous iteration. This is documented in details [there](/snapper#rolling-back). In a nutshell:

1. Reboot your system. From the GRUB2 screen (bootloader), select __Start Bootloader from a read-only snapshot__.
2. Select the snapshot you want to boot to.
3. Log in normally and do `sudo snapper rollback`.

## Reverting to a previous kernel image

As it happens, the Linux kernel sometimes undergoes regressions. For the end user this often translates into issues where the system has trouble communicating with the hardware. The user could rollback to a previous _Btrfs_ snapshot (see previous section), but this would move the entire system -- not just the kernel -- to a previous state. However there are situations where only the kernel is the culprit. When that is the case you are often better off _reverting_ to an older kernel instead of clinging on a old Tumbleweed snapshot through a snapper rollback.

Here we will look at two likely scenarios:

1. You want to revert to a recent kernel version shortly after noticing issues with the latest kernel.
2. You want to revert to a not-so-recent kernel version because you weren't able to identify issues before it was removed from the official Tumbleweed repository.

For either scenario to work, you will need to configure your system to accept using an older kernel and to expose GRUB2 entries enabling you to use it persistently. So if you find yourself in the (1) or in the (2) scenario, please follow along:

Open zypper configuration file:

```
$   sudo nano /etc/zypp/zypp.conf
```

Make sure that the line `multiversion = provides:multiversion(kernel)` is not commented out (make sure there is no `#` prepended to `multiversion = provides:multiversion(kernel)`). If the line is commented out, use the arrow keys to bring the cursor near the `#` and delete it.

Then look at the line reading `multiversion.kernels = <...>`. It encodes the policy the system uses for keeping kernels. By default, what fills the `<...>` is `latest,latest-1,running`. This means that zypper should not remove:

- the kernel available from the latest Tumbleweed snapshot installed on the system; nor
- the one before; nor
- the current, running kernel

Once you have identified the exact kernel version you want to keep using for some time, simply add it as:

```
multiversion.kernels = latest,latest-1,5.13.12-2-default,running    # new: 5.13.12-2-default
```

Now the system should not remove:

- the kernel available from the latest Tumbleweed snapshot installed on the system; nor
- the one before; nor
- the kernel image named `5.13.12-2-default` (new)
- the current, running kernel

If you have followed through, you system now will _accept_ to use an old kernel __name__. Now you need to install and expose to the bootloader the __actual__ kernel image bearing the name.

### Reverting to an old but recent kernel still in the official repositories

If the kernel you have just allowed using the instructions from the previous section exists in the official repository, you simply have to do:

```
$   sudo zypper in <name of the kernel package>
```

where `<name of the kernel package>` could well be `5.13.12-2-default`.

Finally, reboot your system and pick the newly installed kernel from the corresponding GRUB2 entry:

__GRUB2__:

1. Select `Advanced options for openSUSE Tumbleweed`
2. Select the entry corresponding to the newly installed kernel.

### Reverting to an old kernel only present in Tumbleweed history

Unfortunately the official repositories do not store a wide gamut of kernels at any time. This means that if you notice that you need to revert to an old kernel after the kernel is gone from the official repositories, you need to manually download the kernel using [Tumbleweed's official "history" repositories](https://download.opensuse.org/history/).

Just like other packages, kernel packages are organized under Tumbleweed snapshots and CPU architecture. Thus to pick the right kernel package, you need to know:

1. the version of the kernel you need;
2. the Tumbleweed snapshot that shipped (1);
3. your CPU architecture.

Note that there is exactly one `kernel-default` per snapshot per CPU architecture.

These informations will enable you to find a directory on a path of type: `https://download.opensuse.org/history/<snapshot-you-need>/tumbleweed/repo/oss/<your-cpu-architecture>/`, and in it a `kernel-default-<version>.<your-cpu-architecture>.rpm` package. Download it and do:

```
$   sudo zypper in -f --oldpackage <path/to/kernel package.rpm>
```

`zypper` will take care of writing a new GRUB entry for the newly installed old kernel, but it is likely to place the new entry _below_ the entries for the more recent kernel entry or entries you are staying away from. If you want to set the new entry as default, follow the path below and select the entry listed at the end:

* _Yast_ > _Bootloader_ > _Bootloader Options_ > _Default Boot Section_ > __`<new kernel entry>`__ (select it)

You can now reboot and select the new entry and press `Enter` to start using the newly installed kernel.

If for any reason you need to cling on to an outdated kernel installed with the above method, discarding kernel packages shipped with more recent Tumbleweed snapshots, you can add a package lock to the corresponding package name:

```
$   sudo zypper al "kernel-*"
```

Bear in mind that this will put a lock on all packages with a name starting with "kernel-". Make sure to unlock all of them by first listing the locks with

```
$   zypper ll
```

and unlocking them with

```
$   sudo zypper rl <package_name>
```

as soon as possible, as keeping an outdated kernel induces stability and security risks for your system.

It's also a good idea to keep your kernel version aligned with the Tumbleweed snapshot it stems from. For that, you can use `tumbleweed-cli`.

## tumbleweed-cli

!!! warning
    `tumbleweed-cli` is an experimental tool that comes with a couple of safety issues:
    
    - the evaluation algorithm used to [collect statistics](https://review.tumbleweed.boombatower.com/) about the 'health condition' of available Tumbleweed snapshots has been alleged to produce false positives.
    
    - the advantages of rolling back to a previous Tumbleweed snapshot comes with the drawback that the user is no longer in sync with possible upcoming security fixes, which are shipped to target only the latest available snapshot.

This tools is exclusively designed for _Tumbleweed_. If you have not already, please read the short [introduction](#introduction).

`tumbleweed-cli` enables the user to control finely the update process by making explicit which Tumbleweed snapshot one is upgrading to. It also allows the user to select and rollback (or fast-forward) to any Tumbleweed snapshot comprised in a collection of recently released snapshots.

!!! info
    In this paragraph _snapshot_ is used to denote Tumbleweed snaphots unless qualified otherwise. Tumbleweed snapshots are "slices" coming out Factory -- the repository and production line for the Tumbleweed distribution. They consist of a collection of updates targeting a subset of all packages available. Do not conflate them with _Btrfs snapshots_, which are on-disk temporal stages of the file system. Both are discussed below.

`tumbleweed-cli` may come in handy for users running into outstanding issues experienced with the latest or several snaphots across several snapshots -- including the latest one. It may be useful when three conditions occur:

1. There is evidence that the cause(s) of the issue is one or several system packages; and
2. The likely cause(s) "spill over" several single _Btrfs snapshots_ including the latest one; and
3. No previous _Btrfs snapshot_ captures the state of the system which the user desires to rollback to.

In addition to the _responsive_ workflow described above, the user might use the tool _preventively_, by updating their system to a Tumbleweed snaphot located before the latest snapshot provided by Factory. Notice however that both the responsive and the preventive workflows could make your system unstable. This happens typically with packages provided by foreign repositories, which escape the scope of the rollbacks/fast-forwards mechanism. When foreign packages rely on dependences not or longer satisfied by the state of your system after rolling-back/fast-forwarding, they become unusable. Proceed only if you can accomodate these limitations.

### Installation and use
To install `tumbleweed-cli`:

`zypper in tumbleweed-cli`

To intialize it:

`tumbleweed init`

The developer suggests to run this command to verify that the URLs of the repos contain a snapshot version:

`zypper lr -EUP`

Once this is done you can finally take advantage of the snapshotting system employed for producing Tumbleweed packages, keeping your system in sync with recent snapshot of your choice. Updating to the latest snapshot released is done with:

`tumbleweed update`

As explained above, packages installed from foreign repositories are not included in Tumbleweed snaphshots, and will not update when using `tumbleweed-cli`. For these packages you will still have to run `(sudo zypper ref &&) sudo zypper dup`.

At any time you can then check the status of your system with respect to the latest snapshots released:

`tumbleweed status`

that returns for example:

```
latest   : 20210414
target   : 20210414
installed: 20210414
```

where `target` refers to the next snapshot the system is going to update to with `tumbleweed-update`.

Setting the target to the previous snapshot and reverting to it is performed with:

`tumbleweed revert --install`

or if the previous snapshot is for instance 20210411, the equivalent:

```
tumbleweed switch 20210411
sudo zypper dup
```

`tumbleweed-switch` is thus the more generic option allowing you to set the target to whichever snapshot you fancy, provided it is available. The full list of snapshots available for switching is obtained with:

`tumbleweed history`

### Uninstall
Should you want to uninstall `tumbleweed-cli`, make sure you deactive its particular update mechanism before. This is done with:

`tumbleweed uninit`

From now on `zypper-dup` will again fetch updates across all snapshots, which is the default. You can remove `tumbleweed-cli` with:

`sudo zypper rm tumbleweed-cli`

## Further reading
* [tumbleweed-cli](https://github.com/boombatower/tumbleweed-cli)
* [snapshow reviewer](https://review.tumbleweed.boombatower.com/)
