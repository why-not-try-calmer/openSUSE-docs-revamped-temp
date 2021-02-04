## Nvidia proprietary drivers

### Determine which driver is required by your GPU

1. There are two Nvidia driver options available in the Nvidia repository - __nvidia-glG05__ and __nvidia-glG04__.
2. The __nvidia-glG05__ package corresponds with the Nvidia 460 series driver.
3. The __nvidia-glG04__ package corresponds with the Nvidia 390 series driver.
4. Please refer to the Nvidia [website](https://www.nvidia.com/en-us/drivers/unix/) to determine which driver best supports your GPU.

### Setup the driver

####With Yast
1. Go to _YAsT2_.
2. Then _Software Management_.
3. On the menu, click __Configuration__ &gt; __Repositories__... (or do `Ctrl + R`).
4. Click __Add__ &gt; __Community Repositories__.
5. Select __nVidia Graphics Drivers__ &gt; __Accept__ &gt; __Trust__.
6. On the _Configured Software Repositories_ &gt; Click __Ok__
7. Back to _Software Management Windows_" &gt; __View__ &gt; __Repositories__ &gt; Select __nVidia Graphics Drivers__.
8. Select __x11-video-nvidiaG05__ &gt; __Accept__ (Some graphic cards need _G04_, see the first section above)
9. Reboot.

####Using the command line
1. Add the Nvidia Repository. If using Tumbleweed for example, you would run `sudo zypper addrepo --refresh https://download.nvidia.com/opensuse/tumbleweed NVIDIA`. For Leap, you can run `sudo zypper addrepo --refresh 'https://download.nvidia.com/opensuse/leap/$releasever' NVIDIA`.
2. Install the appropriate driver by running `sudo zypper in x11-video-nvidiaG05` or `sudo zypper in x11-video-nvidiaG04`
3. Reboot.

####CUDA
1. CUDA can be installed with the __Nvidia-ComputeG05__ or __Nvidia-ComputeG04__ package.
2. See [Nvidia's documentation](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html) for further information.


###Hybrid Graphics/Optimus
<<<<<<< HEAD
1. See [Hybrid Graphics](/hybrid_graphics.md)
=======
####Intel and Nvidia
#####Installation
1. Follow the above directions to install the correct driver.
2. Install the the __suse_prime__ package using zypper or Yast.
#####Set the Nvidia card as your active GPU
1. Run `sudo prime-select nvidia` from the command line.
2. Log off and back on.
#####Set the Intel card as your active GPU
1. Run `sudo prime-select intel` from the command line.
2. Log off and back on.
#####Offloading specific applications to the Nvidia GPU
1. With Intel card set as your active GPU, you can also offload specific applications using an envoronment variable.
2. For example, to run __supertuxkart__ using the the Nvidia driver, you can run `__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia supertuxkart`.
3. To launch a game from Steam using the Nvidia driver, you can modify properties of the game, choose __Set Launch Options__ and add `__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia %command%`.

####Recent AMD APUs and Nvidia
1. Follow the above directions to install the correct driver. 
2. The open source AMD GPU drivers will load by default.
3. You can launch a program using the Nvidia card using an environment variable. For example, to run __supertuxkart__ using the the Nvidia driver, you can run `__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia supertuxkart`.
4. To launch a game from Steam using the Nvidia driver, you can modify properties of the game, choose __Set Launch Options__ and add `__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia %command%`.

###CUDA
1. CUDA can be installed with the __Nvidia-ComputeG05__ or __Nvidia-ComputeG04__ package.
2. See [Nvidia's documentation](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html) for further information.
>>>>>>> 3ef67f17faaa882351e22ccb41d408a2db4aa69a
