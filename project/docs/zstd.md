## Enable zstd compression

!!! Warning
    Use `zstd` exclusively on _Tumbleweed_ as _Leap_ 15.x doesn't support it.

First enable `zstd` compression on `/` (rootfs) by adding `compress=zstd:1` mount option to `/etc/fstab`:

```
<UUID> / btrfs compress=zstd:1 0 0
<UUID> /home btrfs subvol=/@/home 0 0
```

Once set, this mount option applies to any subvolumes mounted from the same filesystem.

This will apply `zstd` compression to all the files directly under the relevant directories. The files already present under these directories will not be touched, however. To retroactively compress them, use:

`sudo btrfs filesystem defrag -czstd -rv / /home/`

## Costs & benefits
Compression is about making your files take less space on your hard drive, but it requires extra computations when reading or writing. In other words it trades (very marginally increased) processor and RAM usage for less hard drive usage. The tradeoff is likely to benefit the most modern machines with solid state drives (_SSD_), which are likely to have less storage capacity than their desktop counterparts, while having comparable computational power. 

It should also be noticed that `zstd`compression can increase the lifespan of SSDs, as it may decrease reads over time.
