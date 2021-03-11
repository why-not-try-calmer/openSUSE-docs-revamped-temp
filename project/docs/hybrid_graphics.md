## Enabling Hybrid Graphics
[ here goes an interesting introduction and a brief explanation of what this section is about, e.g. why this is needed and who is it for. ]
###Dedicated NVIDIA GPU
####Getting the necessary software
1. [Install the correct Nvidia driver](install_proprietary.md).
2. Install the the __suse-prime__ package using zypper or YaST.  If you want to install via zypper run the following command in a terminal session: `sudo zypper in suse-prime`. Users on NVIDIA graphic cards released before GeForce 600 series should instead install __suse-prime-bbswitch__.
:   !!! info 
        In case you are using KDE, a widget called _SUSE Prime Selector_ can be added as graphical way to switch graphics. Just right-click your desktop and go to _Add Widgets..._.
3. To offload rendering to the dedicated GPU (NVIDIA Optimus) please see [Offloading specific applications to the Nvidia GPU](hybrid_graphics.md#Offloading specific applications to the Nvidia GPU). This allows you to use both graphical units simultaneously.

###Intel and NVIDIA
* From the a terminal session, you can switch your main GPU too: for the NVIDIA GPU run `sudo prime-select nvidia`; conversely, for the Intel GPU run `sudo prime-select intel`. After executing either command, log out and back on to apply the changes.
* If you want to use the KDE widget, just click it and select _Switch to NVidia_. Then log out and back in and you should be set.

###AMD APU and Nvidia
When using XOrg 1.20.6 or higher: to make the APU the primary GPU and have the ability to offload specific applications to the GPU, you can skip to the offloading section below.

If you are using an older release of XOrg, such as the case for Leap 15.2, or you wish to exclusively use the NVIDIA card, pick between the following steps:

* To enable only the NVIDIA GPU, run `sudo prime-select nvidia` from the command line.
* To enable the AMD GPU, run `sudo prime-select unset`.
    
After executing either command, log out and back on to apply the changes.

###Offloading specific applications to the Nvidia GPU
With the integrated Intel GPU or AMD APU set as your main GPU, you can also offload specific applications using an envoronment variable. For example, to run __supertuxkart__ using the the NVIDIA driver, you can launch it from a terminal session as follows:
    
    __NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only supertuxkart
    
To offload a game from Steam using the NVIDIA driver, you can modify properties of the game, by right clicking it, going to **Properties** choosing __Set Launch Options__ and adding a line like this:

    __NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only %command%

For the offload process to work properly on integrated Intel cards, make sure the file `/etc/X11/xorg.conf.d/90-intel.conf` exists. If you do not have it, enable the NVIDIA card once and switch back to the integrated GPU; the required file will be created automatically along the way. Note that this only applies to XOrg sessions.

If everything worked out fine for you, feel free to copy and modify the _.desktop_ files of your more demanding programs (like MATLAB or Octave) and append the enviroment variables to the `Exec` section of the file:
    
    Exec=__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only /usr/bin/octave --gui %f
    
This makes the desired program offload to the dedicated GPU everytime you launch it from the shortcut.
        
###Dedicated AMD GPU
To offload anything to the AMD GPU simply use the special enviroment variable to launch the desired program, as per the following example:

    DRI_PRIME=1 supertuxkart
