## Nvidia proprietary drivers

### Determine which driver is required by your GPU

1. There are two Nvidia driver options available in the Nvidia repository - nvidia-glG05 and nvidia-glG04.
2. The nvidia-glG05 package corresponds with the Nvidia 460 series driver.
3. The nvidia-glG04 package corresponds with the Nvidia 390 series driver.
4. Please refer to the Nvidia [website](https://www.nvidia.com/en-us/drivers/unix/) to determine which driver best supports your GPU.

### Setup the driver

1. Go to _YAsT2_.
2. Then _Software Management_.
3. On the menu, click __Configuration__ > __Repositories__... (or do `Ctrl + R`).
4. Click __Add__ > __Community Repositories__.
5. Select __nVidia Graphics Drivers__ > __Accept__ > __Trust__.
6. On the _Configured Software Repositories_ > Click __Ok__
7. Back to _Software Management Windows_"_ > __View__ > __Repositories__ > Select __nVidia Graphics Drivers__.
8. Select __Nvidia-ComputeG05__ & __x11-video-nvidiaG05__ > __Accept__ (Some graphic cards need _G04_)

### Configure CUDA

1. On terminal run `nvidia-smi` and look for _CUDA Version_.
2. Run `sudo zypper addrepo https://developer.download.nvidia.com/compute/cuda/repos/opensuse15/x86_64/cuda-opensuse15.repo`
3. `sudo zypper ref -f`
4. `sudo zypper se cuda-nvcc-"CUDA Version"`
6. `export PATH=/usr/local/cuda-"version"/bin:${PATH:+${PATH}}`
   `export LD_LIBRARY_PATH=/usr/local/cuda-"version"/lib64:${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}`
