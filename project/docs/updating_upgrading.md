## Leap
...

## Tumbleweed
### Introduction
Tumbleweed is a rolling-release distribution. This means it gets updates on a continuous basis, usually several times a week. However, to bring some order and make it easier to manage producing and consuming updates, Tumbleweed fetches updates in batches. A single batch of updates is called a _snapshot_.

!!! info
    Tumbleweed snapshots ought not to be confused with _Btrfs snapshots_, a completely different kind of snapshots. The latter make up the different filesystem states that allow _Btrfs_ users, using tools like YaST2 and `snapper`, to control which state they want to actualize).

Tumbleweed snapshots are thus batches of updates which are tested in openQA -- openSUSE's very best testing pipeline. The tests are thorough and check not only that each upgraded package is healthy, but also that all updates in the same snapshot play nice with one another. A snapshot is thus as healthy as the individual updates it holds, and as the compatibility between one another.

It should be emphasized that by default, Tumbleweed updates across all available updates, whether or not they belong to the same snapshots. In other words, the distribution of updates into snapshots is, by default, only used for production purposes. To take advantage of it at the user level, consider the [tumbleweed-cli section below](#tumbleweed-cli).

### Updating and upgrading
As for Leap updating Tumbleweed is done with

`sudo zypper dup`

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

### tumbleweed-cli

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

In addition to the "recovery" workflow described above, the user might use the tool preventively, by updating their system to a Tumbleweed snaphot located before the latest snapshot provided by Factory. Notice however that both the "recovery" and the preventive workflows could make your system unstable. This happens typically with packages provided by foreign repositories, which escape the scope of the rollbacks/fast-forwards mechanism. When foreign packages rely on dependences not or longer satisfied by the state of your system after rolling-back/fast-forwarding, they become unusable. Proceed only if you can accomodate these limitations.

#### Installation and use
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

#### Uninstall
Should you want to uninstall `tumbleweed-cli`, make sure you deactive its particular update mechanism before. This is done with:

`tumbleweed uninit`

From now on `zypper-dup` will again fetch updates across all snapshots, which is the default. You can remove `tumbleweed-cli` with:

`sudo zypper rm tumbleweed-cli`

## Further reading
* [tumbleweed-cli](https://github.com/boombatower/tumbleweed-cli)
* [snapshow reviewer](https://review.tumbleweed.boombatower.com/)
