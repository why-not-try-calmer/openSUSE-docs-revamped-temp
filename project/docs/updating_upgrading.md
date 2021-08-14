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
    This section needs some rework as `tumbleweed-cli` introduces some safety issues in that:
    a) the evaluation algorithm used to [collect statistics](https://review.tumbleweed.boombatower.com/) about the 'health condition' of available Tumbleweed snapshots has been alleged to produce false positives.
    b) the advantages of rolling back to a previous Tumbleweed snapshot comes with the drawback that the user is no longer in sync with possible upcoming security fixes, which are shipped to target only the latest available snapshot.

If you have not already, please read the short [introduction](#introduction).

Tumbleweed users are gifted with the very useful command line utility `tumbleweed-cli`. This program lets you control finely your update process my making explicit which snapshot you are upgrading to, and by allowing you to roll in or back from snapshots. This comes in handy in cases where you know for sure that some outstanding issue is caused by one or by a combination of packages from the same snapshot, and go back to a previous snapshot which created no trouble. In addition to your own testing you can get some heads-up on which snapshots are deemed healthy or not from this [web application](https://review.tumbleweed.boombatower.com/).

To install `tumbleweed-cli`:

`zypper in tumbleweed-cli`

Then intialize it:

`tumbleweed init`

The developer suggests to run this command to verify that the URLs of the repos contain a snapshot version:

`zypper lr -EUP`

Once this is done you can finally take advantage of the snapshotting system employed for producing Tumbleweed packages, keeping your system aligned with the latest healthy snapshot released. This is done by simply running:

`tumbleweed update`

It is worth noting that external repositories, that is, repositories manually added and whose packages are not included in Tumbleweed snaphshots, will not be updated just by running this command. You will still have to run `(sudo zypper ref &&) sudo zypper dup`. This is simply a consequence of the fact that the updates provided by Tumbleweed snapshots are but a subset of all the packages in reach of `zypper-dup`, which has access to the updates provided by external repositories.

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

Should you want to uninstall `tumbleweed-cli`, make sure you deactive its particular update mechanism before. This is done with:

`tumbleweed uninit`

From now on `zypper-dup` will again fetch updates across all snapshots, which is the default. You can remove `tumbleweed-cli` with:

`sudo zypper rm tumbleweed-cli`

## Further reading
* [tumbleweed-cli](https://github.com/boombatower/tumbleweed-cli)
* [snapshow reviewer](https://review.tumbleweed.boombatower.com/)
