## Setting up MicroOS as a desktop OS
### Audience and context
This document is meant for users interested in using the openSUSE MicroOS as a desktop. Although this operating system is offered in two flavours (one based on Leap, the other on Tumbleweed), we will only be concerned with the Tumbleweed base, as the Leap base does not provide the required patterns for desktop usage.

### Pre-installation
1. Download the Tumbleweed base ISO image from [https://en.opensuse.org/Portal:MicroOS/Downloads](https://en.opensuse.org/Portal:MicroOS/Downloads)
2. Write the ISO on a USB drive and start the installation process.
3. Select GNOME or KDE Plasma as desktop environment.
4. If desired, select _encryption_. 

In what follows:

* commands starting with `$` can be copied straight into your terminal prompt.
* commands starting with `#` can only be run inside a transactional-update shell.

!!! info
    A _transactional-update shell_ is a chroot where you can open the snapshot you want to boot into next. In that context you can make adjustments to the normally immutable root filesystem. It is meant mostly for advanced users, so mind your keystrokes.

### Ways to install applications
You can install applications in several ways:

* flatpaks from flathub -- the _preferred solution_ 
* RPM's via `sudo transactional-update pkg install package_name`
* RPM's in a toolbox `toolbox -u`
* snaps (requires some extra setup to get snapd installed)
* AppImages

!!! info
    The reason _flatpaks_ are preferred is that they provide a self-contained environment offering a pleasant balance between security, dependence integrity and performance. See [here](https://flatpak.org/) for details.

### Full drive encryption
If you need full drive encryption you can go through the ISO installer as normal, except for the last screen, where you need to click on _Partitioning_. Then use the guided partitioner to enable encryption. _LVM_ is not necessary for it to work. 

Bear in mind however that with full drive encryption, you will need to enter your password twice: 
1. the first time to open the grub menu
2. the second time to decrypt during boot.

Alternatively, the second decrypt can be performed automatically; instructions are provided at [manual](https://en.opensuse.org/SDB:Encrypted_root_file_system).

#### Setting up a key file
The steps below describe how to set up a key file. You will need to execute the commands and edit the files listed below in a _transactional-update_ shell.
```
$ sudo transactional-update shell
  # touch /.root.key
  # chmod 600 /.root.key
  # dd if=/dev/urandom of=/.root.key bs=1024 count=1
  # cryptsetup luksAddKey /dev/sda1 /.root.key
```
Replace `/dev/sda1` with the root partition (`sudo fdisk -l` to see partitions)
```
  # vim /etc/crypttab
```
Edit `/etc/crypttab`, find the line starting with the `UUID` reference of the root partition (`UUID=...`) and write the path to the key file in the third column.

    cr_sda1 UUID=... /.root.key

  (The partition name is just an example.)S
Configure `dracut` to add the key file to the initrd:
```
  # echo -e 'install_items+=" /.root.key "' | tee --append /etc/dracut.conf.d/99-root-key.conf > /dev/null
  # mkinitrd
  # exit
$ sudo reboot
```

### Installation: GNOME
Make sure to install a browser via transactional-updates if you want to use GNOME extensions. The easiest way to install GNOME extensions is via the website provided by the [GNOME project](https://extensions.gnome.org/). For this to work however, you will need a browser installed on the system via transactional-update, as it will not work inside a containerized environment (like flatpaks, snaps, AppImages or in a toolbox). The browser will need to have the plugin mentioned on the top of the page when you first open it. 

After installation of the plugin, you can reload the page and choose the GNOME Extensions you want to install.

To get GNOME extensions running, you need the following command to install Chromium directly:
```
$ sudo transactional-update pkg install chromium
$ sudo reboot
```

* You can also install some other useful packages through transactional-update and then reboot:
```
$ sudo transactional-update pkg install gnome-remote-desktop gnome-shell-search-provider-nautilus gnome-color-manager
$ sudo reboot
```

* Some necessary commands for _GNOME Software_: 
```
$ gsettings set org.gnome.software install-bundles-system-wide false
$ gsettings set org.gnome.software allow-updates false
$ gsettings set org.gnome.software download-updates false
$ gsettings set org.gnome.software enable-repos-dialog false
$ gsettings set org.gnome.software first-run true
```
and the following:
```
$ sudo rm -Rf /var/cache/app-info
$ sudo transactional-update shell
  # rpm -e --nodeps libzypp-plugin-appdata
  # zypper al libzypp-plugin-appdata
  # exit
$ sudo reboot
```

### Installation: KDE
* In _Discover_, enable the _flatpak_ repository.
* For theming, normal theme installations don’t work in KDE Plasma. But you can install themes via a transactional-update shell:
    * (For example the Arc theme and papirus-icon-theme)
```
$ sudo transactional-update shell
  # cd /tmp
  # wget -qO- https://raw.githubusercontent.com/PapirusDevelopmentTeam/arc-kde/master/install.sh | sh
  # wget -qO- https://git.io/papirus-icon-theme-install | sh
  # exit
$ sudo reboot
```
* Other themes (also for GNOME):
    * [Qogir-icon-theme](https://github.com/vinceliuice/Qogir-icon-theme)
    * [Matcha-gtk](https://github.com/vinceliuice/Matcha-gtk-theme)
    * [Matcha-KDE](https://github.com/vinceliuice/Matcha-kde)
    * [Akwa-gtk](https://github.com/berkiyo/akwa)
    
### General tips
It is recommended to disable both automatic updating and automatic rebooting. They are handy features for servers running 24/7), but the developers are thinking about a different and better integration with desktop usage. For now it is recommended to do the same, at least until you have become more familiar with MicroOS. Then you will be able to come up with the best automatic updating & rebooting strategy for your workflow and use case. So:
```
$ sudo systemctl disable --now transactional-update.timer
$ sudo systemctl disable --now rebootmgr.service
```
And then let’s check:
```
$ sudo rebootmgrctl is-active
*RebootMgr is dead*
$ sudo rebootmgrctl status
*Error: The name org.opensuse.RebootMgr was not provided by any .service files*
```
Updates are now to be initialized manually with:
```
$ sudo transactional-update dup
$ sudo reboot
```
Or
```
$ sudo transactional-update shell
  # zypper dup
  # exit
$ sudo reboot
```

### Installing flatpaks

The `flatpak` package is already installed; all that needs to be done is to add the flathub repo. Within KDE Plasma this is possible from within _Discover_. For GNOME you have to use one of the two options below (also possible for a terminal prompt if you don't like Discover):

If you want to install flathub for only your user (in `/home/~ folder`):
```
$ flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```
Updating flatpaks via the terminal are done with the command `flatpak update`

If you want to install flathub for all users (in `/var folder`):
```
$ flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```
Updating flatpaks via the terminal are done with the command `sudo flatpak update`

Some flatpaks you could install from [flathub](https://flathub.org/home):
- [Firefox](https://flathub.org/apps/details/org.mozilla.firefox)
- [Libreoffice](https://flathub.org/apps/details/org.libreoffice.LibreOffice)
- [VLC](https://flathub.org/apps/details/org.videolan.VLC)
- etc.

### Installing snaps

Snaps are mainly used with Ubuntu, but it is possible to install them as well in openSUSE MicroOS with the following commands.

```
$ sudo transactional-update shell
  # zypper addrepo --refresh https://download.opensuse.org/repositories/system:/snappy/openSUSE_Tumbleweed snappy
  # zypper --gpg-auto-import-keys refresh
  # zypper ref
  # zypper dup --from snappy
  # zypper in snapd
  # exit
$ sudo reboot

$ sudo mksubvolume /snap
$ source /etc/profile
$ sudo systemctl enable --now snapd
$ sudo systemctl enable --now snapd.apparmor
```
You can install snaps via the terminal (for example the snap store where you can find snaps and install them via a graphical interface):
```
$ sudo snap install snap-store
```

### Installing virtualbox
```
$ sudo transactional-update shell
  # zypper ref
  # zypper in virtualbox-qt
  # usermod -a -G vboxusers <myuser>
  # exit
$ sudo reboot
```

#### Adding the extension pack for the host
_Installation can be done after you've rebooted into a new snapshot where virtualbox-qt is installed_
```
$ sudo transactional-update shell
  # cd /tmp
  # LatestVirtualBoxVersion=$(wget -qO - https://download.virtualbox.org/virtualbox/LATEST-STABLE.TXT) && wget "https://download.virtualbox.org/virtualbox/${LatestVirtualBoxVersion}/Oracle_VM_VirtualBox_Extension_Pack-${LatestVirtualBoxVersion}.vbox-extpack"
  # VBoxManage extpack install --replace Oracle_VM_VirtualBox_Extension_Pack-${LatestVirtualBoxVersion}.vbox-extpack
  # exit
$ sudo reboot
```

### Adding software from OBS
Find what you need on [https://software.opensuse.org](https://software.opensuse.org). Then choose the openSUSE Tumbleweed repository, as MicroOS desktop is built from the same base as Tumbleweed. Everything that runs on Tumbleweed should in theory run reliably on MicroOS Desktop.

You can't use the 1-click install as there is no YaST and the 1-click installer has no access to a transactional-update. So for example if you install `zerotier`:
1. Pick a _TumbleWeed_ repository.
2. Click on _Expert Download_.
3. Choose the option _Add repository and install manually_.
4. Copy the commands into a _transactional-update shell_
```
$ sudo transactional-update shell
  # zypper addrepo https://download.opensuse.org/repositories/home:alphard:RHEL/openSUSE_Tumbleweed/home:alphard:RHEL.repo 
  # zypper ref
  # zypper in zerotier-one
  # exit
$ sudo reboot
```
If you have added repositories to `zypper` by yourself, it is possible that they will not refresh automatically with a `dup` command. You wll have to give them a flag for autorefresh:
```
$ sudo transactional-update shell
  # zypper lr -p
  # zypper mr -f <number of repo you want to add Yes to in the Refresh colomn>
  # exit
$ sudo reboot
```

### Tips if you're installing on laptop
Two important packages were found missing for some users:

* `tlp` for better battery management
* `ModemManager` for laptops with a WWAN modem

You can install both with:
```
$ sudo transactional-update pkg install tlp ModemManager
$ sudo reboot
```

### Some useful aliases for bash to type the commands faster
Edit the file `~/.bashrc`

And add the following lines in the file at the end:
```
#My custom aliases
alias shalt="sudo systemctl halt"
alias sboot="sudo systemctl reboot"
alias sts="sudo transactional-update shell"
alias stsc="sudo transactional-update shell --continue"
alias stdc="sudo transactional-update dup --continue"
alias std="sudo transactional-update dup"
alias sfu="sudo flatpak update && flatpak update"
alias fsu="sudo flatpak update && flatpak update && sudo snap refresh"
alias dfs="sudo transactional-update dup && sudo flatpak update && flatpak update && sudo snap refresh"
```
Then for example typing `std` in a terminal prompt will issue the command `sudo transactional-update dup`.

## Known Issues
### No graphical session/login screen in Hyper-V
_Hyper-V_ requires the package `xf86-video-fbdev` for graphical session:
```
$ sudo transactional-update pkg install xf86-video-fbdev
$ sudo reboot
```

### Toolbox is not starting ("potentially insufficient UIDs and GIDs")
UUI and GUI need to be set for `toolbox` to work:
```
$ sudo transactional-update shell
  # echo "<yourusername>:100000:65536" > /etc/subuid 
  # echo "<yourusername>:100000:65536" > /etc/subgid
  # exit
$ sudo reboot
```

### Graphical applications in the toolbox are missing fonts/icons
They need to be installed inside the toolbox:
```
sudo zypper install xorg-x11-fonts-core adwaita-icon-theme
```

## Alternative for Gnome Extensions
There is an alternative to installing GNOME Extensions, such that no RPM installed browser is necessary. It's currently under development at [Github](https://github.com/ekistece/GetExtensions). You can install it via `toolbox`; the commands are as follows:
```
$ toolbox -u
$ sudo zypper in git python38-pip
$ cd /tmp
$ git clone https://github.com/ekistece/GetExtensions.git
$ pip3 install ./GetExtensions --user
$ exit
```
You can then open the application _Get Extensions_ that is available from your launchpad.

To uninstall:
```pip3 uninstall getextensions```
