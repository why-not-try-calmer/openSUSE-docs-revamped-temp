## Setting up MicroOS as a desktop OS
### Pre-installation
1. grab ISO from [https://en.opensuse.org/Portal:MicroOS/Downloads](https://en.opensuse.org/Portal:MicroOS/Downloads)
2. put ISO on a USB drive and start the installation proces.
3. choose if you want to use Gnome or KDE.

### Installation: GNOME
Make sure you install a browser and unzip via transactional-updates if you want to use Gnome extensions. Then some commands are in order:

* Install some useful packages trough transactional-update and then reboot:
```
$ sudo transactional-update pkg install gnome-remote-desktop gnome-shell-search-provider-nautilus chromium gnome-tweaks unzip
$ sudo reboot
```

* Some necessary commands for Gnome Software: 
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
* For theming, normal theme installations don’t work in KDE. But you can install themes via a transactional-update shell:
    * (For example the Arc theme and papirus-icon-theme)
```
$ sudo transactional-update shell
  # cd /tmp
  # wget -qO- https://raw.githubusercontent.com/PapirusDevelopmentTeam/arc-kde/master/install.sh | sh
  # wget -qO- https://git.io/papirus-icon-theme-install | sh
  # exit
$ sudo reboot
```
* Other themes:
    * [Qogir-icon-theme](https://github.com/vinceliuice/Qogir-icon-theme)
    * [Matcha-gtk](https://github.com/vinceliuice/Matcha-gtk-theme)
    * [Matcha-KDE](https://github.com/vinceliuice/Matcha-kde)

### General tips
I recommend disabling automatic updating and rebooting as well. It’s a cool feature, and we’re thinking about how to have it better integrated into a desktop distribution. For now, I’ve disabled it and I recommend doing the same, at least until you have learned a little bit about it, are more familiar with the idea and with the basics of how it works, and come come-up with the best automatic updating & rebooting strategy for your workflow and use case. So:
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

Updates are now to be initialised manually with:
```
$ sudo transactional-update dup
```
Or
```
$ sudo transactional-update shell
  # zypper dup
  # exit
```

I also recommend installing a few more packages that are useful.
```
$ sudo transactional-update pkg install
bash-completion wget nfs-client autofs
$ sudo reboot
```


### Installing flatpaks

The flatpak package is already installed, all that needs to be done is add the flathub repo. Within KDE this is possible from within Discover. For Gnome you have to use one of the two options below (also possible for KDE if you don't like Discover):

If you want to install flathub for only your user (in `/home/~ folder`):
```
$ flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```
If you want to install flathub for all users (in `/var folder`):
```
$ flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```
Some flatpaks you could install:
- Firefox
- Libreoffice
- VLC
- etc.


### Installing snapds

Snaps are mainly used within Ubuntu, however it is possible to install them as well in openSUSE MicroOS, the following commands will install it.

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
You can install snaps then via the terminal (for example the snap store where you can find snaps and install them via the GUI):
```
$ sudo snap install snap-store
```

### Installing virtualbox
```
$ sudo transactional-update shell
  # zypper ref
  # zypper in virtualbox-qt
  # exit
$ sudo reboot
```

## Adding the extension pack for the host
```
$ sudo transactional-update shell
  # cd /tmp
  # LatestVirtualBoxVersion=$(wget -qO - https://download.virtualbox.org/virtualbox/LATEST-STABLE.TXT) && wget "https://download.virtualbox.org/virtualbox/${LatestVirtualBoxVersion}/Oracle_VM_VirtualBox_Extension_Pack-${LatestVirtualBoxVersion}.vbox-extpack"
  # VBoxManage extpack install --replace Oracle_VM_VirtualBox_Extension_Pack-${LatestVirtualBoxVersion}.vbox-extpack
  # exit
$ sudo reboot
```

### Adding software from OBS
Find what you are looking for on [https://software.opensuse.org](https://software.opensuse.org).

Then choose the OpenSUSE Tumbleweed repo, but you can't use the 1-click install, as there is no YaST and the 1-click installer has no access to a transactional-update.
So for example if we install `zerotier`, pick a TumbleWeed repository. But click on _Expert Download_
And choose the _Add repository and install manually_ option.
```
$ sudo transactional-update shell
  # zypper addrepo https://download.opensuse.org/repositories/home:alphard:Network:RPM/openSUSE_Tumbleweed/home:alphard:Network:RPM.repo 
  # zypper ref
  # zypper in zerotier-one
  # exit
$ sudo reboot
```
### Tips if you're installing on laptop
My laptop install was missing 2 important packages:

* `tlp` for better battery management

* `ModemManager` because my laptop has a WWAN moden

You can install both with:
```
$ sudo transactional-update pkg install tlp ModemManager
$ sudo reboot
```


## Known Issues
### No graphical session/login screen in Hyper-V
hyper-v needs package "xf86-video-fbdev" for graphical session
```
sudo transactional-update pkg install xf86-video-fbdev
```

### Toolbox is not starting ("potentially insufficient UIDs and GIDs")
uid/guid needs to be set for toolbox to work
```
echo "<yourusername>:100000:65536" > /etc/subuid 
echo "<yourusername>:100000:65536" > /etc/subgid
```

### Graphical applications in the toolbox are missing fonts/icons
They need to be installed inside the toolbox
```
sudo zypper install xorg-x11-fonts-core adwaita-icon-theme
```

