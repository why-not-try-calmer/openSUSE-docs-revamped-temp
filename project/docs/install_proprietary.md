## Nvidia proprietary drivers

### Determine which driver is required by your GPU

There are two Nvidia driver options available in the Nvidia repository -- __nvidia-glG05__ and __nvidia-glG04__.

* The __nvidia-glG05__ package corresponds with the Nvidia 460 series driver.
* The __nvidia-glG04__ package corresponds with the Nvidia 390 series driver.

Please refer to the Nvidia [website](https://www.nvidia.com/en-us/drivers/unix/) to determine which driver best supports your GPU.

### Setup the driver

#### With Yast
1. Open _YAsT2_.
2. Then _Software Management_.
3. On the menu, click __Configuration__ &gt; __Repositories__... (or do `Ctrl + R`).
4. Click __Add__ &gt; __Community Repositories__.
5. Select __nVidia Graphics Drivers__ &gt; __Accept__ &gt; __Trust__.
6. On the _Configured Software Repositories_ &gt; Click __Ok__
7. Back to _Software Management Windows_" &gt; __View__ &gt; __Repositories__ &gt; Select __nVidia Graphics Drivers__.
8. Select __x11-video-nvidiaG05__ &gt; __Accept__ (Some graphic cards need _G04_, see the first section above)
9. Reboot.

#### Using the command line
1. Add the Nvidia Repository:
  - Tumbleweed: `sudo zypper addrepo --refresh https://download.nvidia.com/opensuse/tumbleweed NVIDIA`
  - Leap: `sudo zypper addrepo --refresh 'https://download.nvidia.com/opensuse/leap/$releasever' NVIDIA`
2. Install the appropriate driver by running `sudo zypper in x11-video-nvidiaG05` or `sudo zypper in x11-video-nvidiaG04`
3. Reboot.

#### CUDA
CUDA can be installed with the __Nvidia-ComputeG05__ or __Nvidia-ComputeG04__ package.

See [Nvidia's documentation](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html) for further information.


### Hybrid Graphics/Optimus
See [Hybrid Graphics](hybrid_graphics.md)
