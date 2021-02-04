## Installing Codecs 
You need to play online or offline multimedia content but the content does not want to play or shows errors. Usually this is a sign of missing codecs: install these packages from Packman to play most music and video:

- ffmpeg
- gstreamer-plugins-good
- gstreamer-plugins-bad
- gstreamer-plugins-libav
- gstreamer-plugins-ugly
- libavcodec-full
- vlc-codecs

You can get them using Zypper, opi or YaST.

## Download & install Codecs Using Zypper

For Installing codecs we have to add Packman Repo.(Require Root privilages use (su -i)) Remember If things don’t work after installing the codecs, make sure all your multimedia packages are coming from Packman.

Use the following commands to add all of the third-party Packman repository (Packman Essentials is included), according to your installed openSUSE version:
 
# Tumbleweed

`sudo zypper addrepo -cfp 90 'https://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Tumbleweed/' packman`

# Leap

`sudo zypper addrepo -cfp 90 'https://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Leap_$releasever/' packman`

Then, install the desired codecs by first refreshing your local repository database and allowing vendor change for the required packages

1. Add  Pacman Repo `zypper addrepo -f http://packman.inode.at/suse/openSUSE_Tumbleweed/packman`
2. Installing Multimedia codecs `zypper install vlc vlc-codecs k3b-codecs ffmpeg lame gstreamer-plugins-libav gstreamer-plugins-bad gstreamer-plugins-ugly gstreamer-plugins-good gstreamer-fluendo-mp3 libdvdcss2`
3. Update to latest Version `zypper dist-upgrade --from packman`
 
## Installing Codecs for DVDs Using Zypper

You can install any DVD player(for example VLC). 

1. `sudo zypper ar https://download.videolan.org/SuSE/<SUSE version> VLC`
2. `sudo zypper mr -r VLC`
3. `sudo zypper in vlc`





