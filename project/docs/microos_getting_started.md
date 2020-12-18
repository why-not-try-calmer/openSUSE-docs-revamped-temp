 >>>>MicroOS as a desktop OS first things to do<<<<

grab ISO from https://en.opensuse.org/Portal:MicroOS/Downloads
Put ISO on a USB drive and start the installation proces.
Choose if you want to use Gnome or KDE.


>>>For Gnome<<<
For Gnome make sure you install a browser and unzip via transactional-updates if you want to use Gnome extensions

$ gsettings set org.gnome.software install-bundles-system-wide false
$ gsettings set org.gnome.software allow-updates false
$ gsettings set org.gnome.software download-updates false
$ gsettings set org.gnome.software enable-repos-dialog false
$ gsettings set org.gnome.software first-run true

$ sudo transactional-update pkg install gnome-remote-desktop gnome-shell-search-provider-nautilus chromium gnome-tweaks unzip
$ sudo reboot

>>>For KDE<<<
In discover enable the flatpak repo

>>>Theming<<<
Normal theme installations don’t work in KDE. But you can install themes via a transactional-update shell: (For example the Arc theme and papirus-icon-theme)

$ sudo transactional-update shell
# cd /tmp
# wget -qO- https://raw.githubusercontent.com/PapirusDevelopmentTeam/arc-kde/master/install.sh | sh
# wget -qO- https://git.io/papirus-icon-theme-install | sh
# exit
$ sudo reboot

Other themes:
Qogir-icon-theme:
https://github.com/vinceliuice/Qogir-icon-theme

Matcha-gtk:
https://github.com/vinceliuice/Matcha-gtk-theme

Matcha-KDE:
https://github.com/vinceliuice/Matcha-kde


>>>General stuff<<<
I recommend disabling automatic updating and rebooting as well. It’s a cool feature, and we’re thinking about how to have it better integrated into a desktop distribution. For now, I’ve disabled it and I recommend doing the same, at least until you have learned a little bit about it, are more familiar with the idea and with the basics of how it works, and come come-up with the best automatic updating & rebooting strategy for your workflow and use case. So:
$ sudo systemctl disable --now transactional-update.timer
$ sudo systemctl disable --now rebootmgr.service

And then let’s check:
$ sudo rebootmgrctl is-active
RebootMgr is dead
$ sudo rebootmgrctl status
Error: The name org.opensuse.RebootMgr was not provided by any .service files


$ sudo transactional-update pkg install
bash-completion wget unzip nfs-client autofs
$ sudo reboot

If you want to install flathub for only your user (in /home/~ folder):
$ flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

If you want to install flathub for all users (in /var folder):
$ flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

and the following:
$ sudo rm -Rf /var/cache/app-info
$ sudo transactional-update shell
# rpm -e --nodeps libzypp-plugin-appdata
# zypper al libzypp-plugin-appdata
# exit
$ sudo reboot

>>>Via flatpak install<<<

Firefox
Libreoffice
VLC
etc.

>>>If you want to use snapd do the following<<<

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

You can install snaps then via the terminal:
$ sudo snap install

>>>If you want to install virtualbox<<<

$ sudo transactional-update shell
# zypper ref
# zypper in virtualbox-qt
# exit
$ sudo reboot
>>If you want to install the extension pack for the host<<


$ sudo transactional-update shell
# cd /tmp
# LatestVirtualBoxVersion=$(wget -qO - https://download.virtualbox.org/virtualbox/LATEST-STABLE.TXT) && wget "https://download.virtualbox.org/virtualbox/${LatestVirtualBoxVersion}/Oracle_VM_VirtualBox_Extension_Pack-${LatestVirtualBoxVersion}.vbox-extpack"
# VBoxManage extpack install --replace Oracle_VM_VirtualBox_Extension_Pack-${LatestVirtualBoxVersion}.vbox-extpack
# exit
$ sudo reboot

>>>If you want to install software from OBS<<<

Find what you are looking for on software.opensuse.org
Choose the OpenSUSE Tumbleweed repo, but you can't use the 1-click install, as there is no YaST and the 1-click installer has no access to a transactional-update.
So for example if we install zerotier. choose a TumbleWeed repository. But click on 'Expert Download'
And choose the 'Add repository and install manually' option.

$ sudo transactional-update shell
# zypper addrepo https://download.opensuse.org/repositories/home:alphard:Network:RPM/openSUSE_Tumbleweed/home:alphard:Network:RPM.repo # zypper ref
# zypper in zerotier-one
# exit
$ sudo reboot

>>>If you install on a laptop<<<

My laptop install was missing 2 important packages. TLP for better battery management and ModemManager because my laptop has a WWAN moden.
$ sudo transactional-update pkg install tlp ModemManager
$ sudo reboot
