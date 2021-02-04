## Installing Codecs 

## Download & install Codecs 

For Installing codecs we have to add Packman Repo.(Require Root privilages use (su -i)) Remember If things don’t work after installing the codecs, make sure all your multimedia packages are coming from Packman.

1. Add  Pacman Repo `zypper addrepo -f http://packman.inode.at/suse/openSUSE_Tumbleweed/packman`
2. Installing Multimedia codecs `zypper install vlc vlc-codecs k3b-codecs ffmpeg lame gstreamer-plugins-libav gstreamer-plugins-bad gstreamer-plugins-ugly gstreamer-plugins-good gstreamer-fluendo-mp3 libdvdcss2`
3. Update to latest Version `zypper dist-upgrade --from packman`
 
## Installing Codecs for DVDs Using CLI

You can install any DVD player(for example VLC). 

1. `sudo zypper ar https://download.videolan.org/SuSE/<SUSE version> VLC`
2. `sudo zypper mr -r VLC`
3. `sudo zypper in vlc`





