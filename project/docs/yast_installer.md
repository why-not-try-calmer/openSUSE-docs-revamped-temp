# Step by step guide for openSUSE Leap
## Before you continue

This guide targets **openSUSE Leap**; however the differences between the installers for the openSUSE variants -- like Tumbleweed, MicroOS and Kubic -- are minor and most parts of the installer will be identical. This guide also assumes that you will have no other Linux, Windows or MacOS operating systems alongside openSUSE after you are done installing.

!!! warning
    Expect slight differences in *System Role* and *Installation Settings*.

### Minimum system requirements

* Any AMD64/Intel* EM64T processor (32-bit processors are not supported).
* 1 GB physical RAM (4 GB or more strongly recommended).
* 10 GB available disk space for a minimal installation, 16 GB for a graphical desktop (more is recommended). In case you plan to use Btrfs snapshots a minimum of 40 GB for the root partition is recommended.
* Supports most modern sound and graphics cards, 1024 x 768 display resolution (higher recommended).

### Precautions

* Make sure that you have backed up all your data from the disk where you're planning to install openSUSE to.
* If installing on a separate drive, it is recommended to disconnect the drives not in use. You can always probe a foreign operating systems to Grub from a finalized installation with *YaST*, but selecting the wrong drive to install openSUSE will overwrite the disk.
* In case you are using a Network image (Netiso) make sure that you have a strong internet connection. A wired connection is always more reliable than a wifi signal.
* If installing on a laptop, it is considered good practice to plug in the AC charger.

You are now ready to boot the USB drive, which you have previously prepared as explained above and turn on your computer. Boot from the USB drive and select installation from the boot menu.

If you want to see the details of the installation while the installer is booting you can press the `ESC` key.

??? info   
    On the left you will see a list of items where you can follow the steps you will take next or have already taken. You can always press `Back` in case you want to change something you configured earlier.
## Getting started    
## Welcome
### Language, keyboard and license agreement

*Welcome to the YaST installer!*

Here you can change the language under `Language` (Alt+L), change the keyboard layout under `Keyboard Layout` (Alt+K) and you have a chance to test your selected keyboard layout in the field under `Keyboard Test` (Alt+y).

The `License Agreement` informs you on roles, responsibilities and potential limitations.

## Network activation
!!! note "Online repositories"
    If you have an active internet conenction over an ethernet cable connected to your computer, right after you pass the *"Language, Keyboard and License Agreement"* section you will be prompted, asking if you want to *"Activate online repositories now?"*.It is recommended to select *Yes* which will provide you with a list of repositories, pre-selecting the main ones in advance. 

### Network settings
You can skip this part if:
* you have an active wired internet connection which can be automatically configured by the system; or
* you're not planning to use online repositories during the installation process and rely on the packages provided by the DVD installer.   

If you don't have a network connection -- because you have don't have an ethernet port or you're using wireless connections only -- you will be prompted to configure your network settings. By default the YaST system installer is using _Wicked_ as the network controller. Keep in mind that if you choose to use _Network Manager_ (NM) instead of _Wicked_, your network configuration will not be transferred from _Wicked_ to _Network Manager_.

You will see a list of available network adapters in a list from which you can select the one that you wish to configure and press `Edit`. In case of configuring a wireless adapter you will see the `Wireless Specific` tab open. Here you can `Scan Network`, select the name of your wireless network (SSID), select the authentication mode -- which in most cases will be WPA-PSK "home" -- and input your password. 
On the `Addresses` tab you will be able to configure the IP address of your adapter. You can select `Dynamic Address` in which case you will receive an address from your router/other dhcp server on your network or `Statically Assigned IP Address` in which case you need to type an ip address, a subnet mask and a hostname. Click `Next` when finished.    
![Wireless configuration](image/yast_wireless_specific.png)

**If** you configured a statically assigned IP address:
1. Switch to the `Routing` tab.
2. Press `Add`, add your default gateway address and select the network adapter to which you want this route assigned to.
3. Press `OK` then `Next` to apply your changes and continue the installation.

## Online repositories 
### List of online repositories

These repositories are the following:

* _Update Repository (Non-Oss)_: Non-free -- as in Free Open-Source Software (FOSS) -- update repository. Provides security and maintenance updates for the distribution. _Selected by default._
* _Non-OSS Repository_: Non-free repository. Provides packages like Steam. _Selected by default._
* _Main Update Repository_: Main free -- as in FOSS -- update repository. Provides security and maintenance updates for the distribution. _Selected by default._
* _Main Repository_: Main free repository provides FOSS only. _Selected by default._
* _Update Repository (Debug)_: A repository for those who want to debug application updates for openSUSE, for experts only. 
* _Untested Updates_: Security and maintenance updates that requires testing. Add this repository if you want to participate in testing.
* _Source Repository_: Provides all source packages in the distribution, for experts only.
* _Debug Repository_: A repository for those who want to debug applications for openSUSE, for experts only.

NOTE: I need to further clarify this bit!!! Do keep in mind that if you're on a metered network and you choose to add the online repos, the installer will download packages with newer versions. 

### System role

At this section of the preparation you can select from a list of predefined use cases which will tailor the system for the selected scenario. It is recommended to take a look at the [Choosing the right image](pick_an_image.md) page prior to continuing as it provides a brief explanation of desktop environments. Do note if you're using a DVD installer without an active internet connection, some of the listed desktop envirnments will not be available.

* _Desktop with KDE Plasma_: the Plasma desktop with KDE applications.
* _Desktop with GNOME_: the GNOME desktop with GNOME applications.
* _Generic Desktop_: IceWM, a minimal desktop environment with minimal amount of packages.
* _Server_: A server installation without providing any graphical environment. _Not recommended for inexperienced users._
* _Transactional Server_: A server installation with a read-only root filesystem in an immutable server style. This will provide atomic, automatic updates and packages can be installed with `transactional-update` only. _Not recommended for inexperienced users._

Select one and press __Next__ to continue.

## Disk
### Suggested Partitioning
![Suggested Partitioning](image/yast_suggested_partitioning.png) 

### Guided Setup

### Expert Partitioner

## Time Zone
### Clock and Time Zone

Select your region and the time zone that you want to use for the system. The hardware clock is by default set to UTC. Select _Other Settings_ for other time settings:

* _Manually_: Set time and date manually.
* _Synchronize with NTP Server_: Automatically synchronize time and date with the provided NTP servers which you can initiate right now by selecting *"Synchronize now"*.
* _Run NTP as daemon_: Run NTP as a daemon so the service always runs and starts automatically on boot and sets the time and date based on the received values from the NTP server.
* _Save NTP Configuration_: save or discard the NTP configuration into the installed system.

## User Settings
### Local User

At this point you can create a local user or skip that by selecting _Skip User Creation_ you can set up a password for `root`. 
You need to fill out the empty fields in order to create a first user:

* _User's Full Name_: A human-friendly version of your username, which will be shown in login windows for example.
* _Username_: The actual username for your account. This will be used in all login contexts, whether dialogue boxes or terminal prompts, including virtual console (vc) or over Secure Shell (SSH).
* _Password_: The password for your user to secure your account.

By default you will also use this newly created account to act as the administrator. The password set there will be set for the `root` user as well.

!!! note "Root user"
    openSUSE creates the root user by default, in contrast to other Linux distributions. To avoid using the same password for both users, untick the _Use this password for system administrator_ checkbox. Clicking __Next__ will take you to the page where you can set a different password for root. At this page you can also import existing SSH public keys. This is useful if you're installing openSUSE over another distribution.

## Installation
## Installation Overview
### Installation Settings
DO NOT SKIP THIS!
