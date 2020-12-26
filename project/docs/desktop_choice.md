## Desktop Environments
Leap offers the __GNOME__ and __KDE Plasma__ desktop environments by default. Tumbleweed offers __GNOME__, __KDE Plasma__, and __XFCE__. These options are chosen during the __System Role__ selection of the installation. Additional options are available in the __Software__ selection of the installer. These desktop environment patterns may also be viewed and modified with the [YaST](/yast/) __Software Management__ module.

Modern desktop environments have evolved into highly consistent and productive computing experiences. Choosing the right desktop environment for you can be a deeply personal experience; many people feel as passionately about their desktop environment as they do their distribution. 

One of the distinguishing options of openSUSE, during the installation, is the ability to choose a desktop environment. This provides a certain level of assurance that you will have a well-supported desktop experience for that respective desktop environment, no derivatives or spin-offs needed.

### Installation

During __openSUSE Installation__, there is a section titled __System Role__, which applies patterns to the software selected for the respective installation. This is the primary means to select a desktop environment. There is an additional option titled __Generic Desktop__. If you wish to install a desktop environment available in the installer, but not in a Role, you can select __Generic Desktop__ and customize the Software selection later within the installer.

The descriptions below are as provided by the respective projects.
### GNOME
> GNOME 3 has been designed from the ground up to help you have the best     possible computing experience.
>
> GNOME 3 provides a focused working environment that helps you to get things done, and it is packed with features that will make you more productive. A powerful search feature lets you access all your work from one place. Side-by-side windows makes it easy to view several documents at the same time, and we even provide a way to turn off notifications when you really need to concentrate on the task in hand.
>
> [GNOME.org](https://www.gnome.org)

??? info "Install Gnome"
    __Installer__ &gt; __System Role__: Select __Desktop with GNOME__
### KDE Plasma
> Plasma is made to stay out of the way as it helps you get things done. But under its light and intuitive surface, it's a powerhouse. So you're free to choose ways of usage right as you need them and when you need them.
>
> With Plasma the user is king. Not happy with the color scheme? Change it! Want to have your panel on the left edge of the screen? Move it! Don't like the font? Use a different one! Download custom widgets in one click and add them to your desktop or panel.
>
> [KDE.org](https://kde.org)

??? info "Install KDE Plasma"
    __Installer__ &gt; __System Role__: Select __Desktop with KDE Plasma__
### XFCE
> Xfce is a lightweight desktop environment for UNIX-like operating systems. It aims to be fast and low on system resources, while still being visually appealing and user friendly.
>
> Xfce embodies the traditional UNIX philosophy of modularity and re-usability. It consists of a number of components that provide the full functionality one can expect of a modern desktop environment. They are packaged separately and you can pick among the available packages to create the optimal personal working environment.
>
> Another priority of Xfce is adherence to standards, specifically those defined at freedesktop.org.
>
> [XFCE.org](https://www.xfce.org)

??? info "Install XFCE"
    Tumbleweed:

      * __Installer__ &gt; __System Role__: Select __Desktop with Xfce__

    Leap:

      * __Installer__ &gt; __System Role__: Select __Generic Desktop__
      * __Installer__ &gt; __Installer Settings__: Select __Software__
      * __Installer__ &gt; __Software Selection and System Tasks__:
          * De-Select __A very basic desktop__
          * Select __XFCE Desktop Environment__
    
    !!! note
        Clicking the __Details...__ button on __Installer__ &gt; __Software Selection and System Tasks__ displays packages associated with the respective patterns. 
### Generic Desktop
Another option for a __System Role__ is the __Generic Desktop__ environment. This option enables __IceWM__, but also allows selection of one of the optional desktop environments later, during the __Software__ selection of the installation. If you intend to choose one of the other desktop environments, choose __Yes__, when asked "Activate online repositories now?" and ensure __Main Repository (OSS)__ is enabled on the __Online Repositories__ step.
#### IceWM
> IceWM is a window manager for the X Window System. The goal of IceWM is speed, simplicity, and not getting in the user’s way. It comes with a taskbar with pager, global and per-window keybindings and a dynamic menu system. Application windows can be managed by keyboard and mouse. Windows can be iconified to the taskbar, to the tray, to the desktop or be made hidden. They are controllable by a quick switch window (Alt+Tab) and in a window list.
>
> [Ice-WM.org](https://ice-wm.org/)

??? info "Install IceWM"    
    __Installer__ &gt; __System Role__: Select __Generic Desktop__

### Additional Choices
Before confirming the installation, an overview is provided with the selected installation options, title __Installer Settings__. At this point, you can modify the __Software__ selected to be installed. In addition to reducing or adding packages, you can also select other desktop environments to install.

If you prefer one of these desktop environments, de-select the __A very basic desktop__ pattern, then select one of the below patterns:
#### Enlightenment
> Enlightenment is classed as a "desktop shell" as it provides everything you need to operate your desktop or laptop, but it is not a full application suite. This covers functionality including launching applications, managing their windows and performing system tasks like suspending, rebooting, managing files and so on.
>
>[Enlightenment.org](https://www.enlightenment.org)

??? info "Install Enlightment"

    * __Installer__ &gt; __System Role__: Select __Generic Desktop__
    * __Installer__ &gt; __Installer Settings__: Select __Software__
      * __Installer__ &gt; __Software Selection and System Tasks__:
          * De-Select __A very basic desktop__
          * Select __Enlightenment__
    
    !!! note
        Clicking the __Details...__ button on __Installer__ &gt; __Software Selection and System Tasks__ displays packages associated with the respective patterns. 
#### LXDE
> LXDE, which stands for Lightweight X11 Desktop Environment, is a desktop environment which is lightweight and fast. It is designed to be user friendly and slim, while keeping the resource usage low. LXDE uses less RAM and less CPU while being a feature rich desktop environment. Unlike other tightly integrated desktops LXDE strives to be modular, so each component can be used independently with few dependencies. This makes porting LXDE to different distributions and platforms easier. 
>
> [LXDE.org](http://www.lxde.org/)

??? info "Install LXDE"

    * __Installer__ &gt; __System Role__: Select __Generic Desktop__
    * __Installer__ &gt; __Installer Settings__: Select __Software__
      * __Installer__ &gt; __Software Selection and System Tasks__:
          * De-Select __A very basic desktop__
          * Select __LXDE Desktop Environment__
    
    !!! note
        Clicking the __Details...__ button on __Installer__ &gt; __Software Selection and System Tasks__ displays packages associated with the respective patterns. 
#### LXQt
>  LXQt is a lightweight Qt desktop environment. It will not get in your way. It will not hang or slow down your system. It is focused on being a classic desktop with a modern look and feel.
> Historically, LXQt is the product of the merge between LXDE-Qt, an initial Qt flavour of LXDE, and Razor-qt, a project aiming to develop a Qt based desktop environment with similar objectives as the current LXQt.
>
> [LXQt-Project.org](https://lxqt-project.org)

??? info "Install LXQt"

    * __Installer__ &gt; __System Role__: Select __Generic Desktop__
    * __Installer__ &gt; __Installer Settings__: Select __Software__
      * __Installer__ &gt; __Software Selection and System Tasks__:
          * De-Select __A very basic desktop__
          * Select __LXQt Desktop Environment__
    
    !!! note
        Clicking the __Details...__ button on __Installer__ &gt; __Software Selection and System Tasks__ displays packages associated with the respective patterns. 
#### MATE
> The MATE Desktop Environment is the continuation of GNOME 2. It provides an intuitive and attractive desktop environment using traditional metaphors for Linux and other Unix-like operating systems.
>
> MATE is under active development to add support for new technologies while preserving a traditional desktop experience.
>
> [Mate-Desktop.org](https://mate-desktop.org/)

??? info "Install MATE"

    * __Installer__ &gt; __System Role__: Select __Generic Desktop__
    * __Installer__ &gt; __Installer Settings__: Select __Software__
      * __Installer__ &gt; __Software Selection and System Tasks__:
          * De-Select __A very basic desktop__
          * Select __MATE Desktop Environment__
    
    !!! note
        Clicking the __Details...__ button on __Installer__ &gt; __Software Selection and System Tasks__ displays packages associated with the respective patterns. 
#### Sway
> Sway allows you to arrange your application windows logically, rather than spatially. Windows are arranged into a grid by default which maximizes the efficiency of your screen and can be quickly manipulated using only the keyboard. 
>
> [SwayWM.org](https://swaywm.org/)

??? info "Install LXQt"

    * __Installer__ &gt; __System Role__: Select __Generic Desktop__
    * __Installer__ &gt; __Installer Settings__: Select __Software__
      * __Installer__ &gt; __Software Selection and System Tasks__:
          * De-Select __A very basic desktop__
          * Select __The openSUSEway desktop environment pattern__
    
    !!! note
        Clicking the __Details...__ button on __Installer__ &gt; __Software Selection and System Tasks__ displays packages associated with the respective patterns.
### Advanced Options
#### Cinnamon
> Cinnamon is a Linux desktop which provides advanced innovative features and a traditional user experience.
> 
> The desktop layout is similar to Gnome 2 with underlying technology forked from Gnome Shell. Cinnamon makes users feel at home with an easy to use and comfortable desktop experience.
>
> [LinuxMint.com](https://projects.linuxmint.com/cinnamon/)

??? info "Install Cinnamon"
    Tumbleweed:

    * __Installer__ &gt; __System Role__: Select __Generic Desktop__
    * __Installer__ &gt; __Installer Settings__: Select __Software__
      * __Installer__ &gt; __Software Selection and System Tasks__:
          * De-Select __A very basic desktop__
          * Ensure Selection of __X Windows System__
          * Click __Details...__
          * Click __Search__ tab
            * Search for and Select __cinnamon__
          * Click __Accept__ and __Continue__

    !!! note
        In this setup configuration, you shouldn't have any Desktop Environment patterns selected, unless you desire multiple desktop environments.
        
        Clicking the __Details...__ button on __Installer__ &gt; __Software Selection and System Tasks__ displays packages associated with the respective patterns.
