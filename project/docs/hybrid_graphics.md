## Enabling Hybrid Graphics
###Intel and Nvidia
####Installation
1. [Install the correct Nvidia driver.](/install_proprietary.md) If using xorg 1.20.6 or greater, this is all that is required if you want to use the Intel GPU as your primary and have the ability to offload specific applications to the GPU. If that's the case, you can skip to the offloading section below. If you are using a release that includes an older version of xorg, such as Leap 15.2, or you wish to exclusively use the Intel or Nvidia card, continue to the next section.
####Set the Nvidia card as your active GPU
1. Install the the __suse_prime__ package using zypper or Yast.
2. To enable the Nvidia GPU, run `sudo prime-select nvidia` from the command line. Log off and back on.
3. To enable the Intel GPU, run `sudo prime-select intel` from the command line. Log off and back on.

###Recent AMD APUs and Nvidia
####Installation
1. [Install the correct Nvidia driver.](/install_proprietary.md) If using xorg 1.20.6 or greater, this is all that is required if you want to use the AMD GPU as your primary and have the ability to offload specific applications to the GPU. If that's the case, you can skip to the offloading section below. If you are using a release that includes an older version of xorg, such as Leap 15.2, or you wish to exclusively use the Intel or Nvidia card, continue to the next section.
####Set the Nvidia card as your active GPU
1. Install the the __suse_prime__ package using zypper or Yast.
2. To enable the Nvidia GPU, run `sudo prime-select nvidia` from the command line. Log off and back on.
3. To enable the AMD GPU, run `sudo prime-select unset` from the command line. Log off and back on.

###Offloading specific applications to the Nvidia GPU
1. With the onboard Intel or AMD GPU set as your active GPU, you can also offload specific applications using an envoronment variable.
2. For example, to run __supertuxkart__ using the the Nvidia driver, you can run `__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia supertuxkart`.
3. To launch a game from Steam using the Nvidia driver, you can modify properties of the game, choose __Set Launch Options__ and add `__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia %command%`.