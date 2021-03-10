## Enabling Hybrid Graphics
###Intel and Nvidia
####Installation
1. [Install the correct Nvidia driver.](install_proprietary.md) 
    * If using xorg 1.20.6 or greater, this is all that is required if you want to use the Intel GPU as your primary and have the ability to offload specific applications to the GPU. If that's the case, you can skip to the offloading section below. 
    * If you are using a release that includes an older version of xorg, such as Leap 15.2, or you wish to exclusively use the Intel or Nvidia card, continue to the next section.
2. Set the Nvidia card as your active GPU:
    * Install the the __suse-prime__ package using zypper or Yast.
    * To enable the Nvidia GPU, run `sudo prime-select nvidia` from the command line. Log off and back on.
    * To enable the Intel GPU, run `sudo prime-select intel` from the command line. Log off and back on.

###Recent AMD APUs and Nvidia
####Installation
[Install the correct Nvidia driver.](install_proprietary.md). And then...
* If using xorg 1.20.6 or greater, this is all that is required if you want to use the AMD GPU as your primary and have the ability to offload specific applications to the GPU. If that's the case, you can skip to the offloading section below.
* If you are using a release that includes an older version of xorg, such as Leap 15.2, or you wish to exclusively use the Intel or Nvidia card, continue to the next section.
####Set the Nvidia card as your active GPU

:   !!! warning
        Users on NVIDIA graphics released before GeForce 600 series should instead install __suse-prime-bbswitch__.

1. Install the the __suse-prime__ package using zypper or Yast.
2. Then:
  
    * To enable the Nvidia GPU, run `sudo prime-select nvidia` from the command line. Log off  and back on.
    * To enable the AMD GPU, run `sudo prime-select unset` from the command line. Log off and back on.

###Offloading specific applications to the Nvidia GPU
1. With the onboard Intel or AMD GPU set as your active GPU, you can also offload specific applications using an envoronment variable.
1. For example, to run __supertuxkart__ using the the Nvidia driver, you can run `__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only supertuxkart`.
1. To launch a game from Steam using the Nvidia driver, you can modify properties of the game, by choosing __Set Launch Options__ and adding `__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only %command%`.
1. Additionally, to launch some programs (e.g. Minecraft or Steam) using the dGPU, you need to add the following lines to `/etc/X11/xorg.conf` (create the file if it does not exist). Note that this only applies to X.Org sessions:

:   !!! note
        This is not needed if you are using `suse-prime`. After enabling the NVIDIA card once,
        the file `/etc/X11/xorg.conf.d/90-intel.conf` will be created automatically, with content
        matching the one below.
        
```
Section "Device"
  Identifier "iGPU"
  Driver "modesetting"
EndSection

Section "Screen"
  Identifier "iGPU"
  Device "iGPU"
EndSection

Section "Device"
  Identifier "dGPU"
  Driver "nvidia"
EndSection
```
This allows the creation of Nvidia _GPU screens_, and after the fact, a new entry should be added to the output of `xrandr --listproviders`, in a way that your output should show an entry like this:
```
Provider 1: id: 0x240; cap: 0x2 (Sink Output); crtcs: 4; outputs: 1; associated providers: 1; name: NVIDIA-G0
    output HDMI-1-0
```
