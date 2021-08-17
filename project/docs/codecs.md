# Installing Codecs 
You need to play online or offline multimedia content but the content does not play or you receive errors. Usually this is a sign of missing codecs. Due to legal limitations proprietary codecs can't be stored and served directly from the __openSUSE__/__SUSE__ infrastructure. To install certain codec packages you will need to add the Packman repository and install the required software from there. Some commonly used and installed codecs are:

- ffmpeg
- gstreamer-plugins-good
- gstreamer-plugins-bad
- gstreamer-plugins-libav
- gstreamer-plugins-ugly
- libavcodec-full
- vlc-codecs

!!! info
    Tumbleweed users who only occasionally use codecs (for example through a web browser or a handful of dedicated applications such as _VLC_ or _OBS Studio_) might find it convenient _to avoid_ the addition of external repositories by using a version of these applications shipping their own codecs. On Tumbleweed the advantage of this approach is that codecs will be kept separate from system libraries, securing extra stability for the user. Interested readers may refer to [this page](/alternative_procurement#flatpaks) for explanations.

You can get them using `Zypper`, _OBS Package Installer_ (`opi`) or `YaST`.
## OBS Package Installer 
`opi` can be used to search and install software from the _Open Build Service_ (OBS) and it works on __openSUSE__ and __SUSE__. To install the required codecs:

1. Launch a _terminal emulator_ (_Konsole_, _Gnome Terminal_ etc.),
1. Install `opi`: `sudo zypper install opi`
1. Install codecs with `opi`: `opi codecs`
1. Update the system: `zypper dist-upgrade --from packman`

## The manual way
### Adding the Packman repository
For installing codecs we have to add the Packman repository. Remember if things don’t work after installing the codecs, make sure all your multimedia packages are coming from Packman.

#### Using the command line
Use the following commands to add all of the third-party Packman repository (Packman Essentials is included), according to your installed openSUSE version:
 
##### Tumbleweed
`sudo zypper addrepo -cfp 90 'https://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Tumbleweed/' packman`

##### Leap
`sudo zypper addrepo -cfp 90 'https://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Leap_$releasever/' packman`

#### Using YaST
1. Launch `YaST`,
1. Select _Software Repositories_,
1. Press _Add_ (lower left corner),
1. From the list select _Community repositories_ and press _Next_,
1. Select _Packman Repository_ and press _Ok_.

### Installing codecs
#### Using the command line
1. Refresh your local repository: `sudo zypper ref`,
1. Install some generally needed codecs: `zypper install ffmpeg lame gstreamer-plugins-libav gstreamer-plugins-bad gstreamer-plugins-ugly gstreamer-plugins-good libavcodec-full vlc-codecs`,
1. Update everything to the latest Version `zypper dist-upgrade --from packman`.

#### Using YaST
1. Launch `YaST`,
1. Select _Software Management_,
1. View > Repositories > Packman Repository,
1. Click "Switch system packages" to packages from Packman repository.

Furthermore you can type in the _Search_ field the name of the codec packages you're looking for.
