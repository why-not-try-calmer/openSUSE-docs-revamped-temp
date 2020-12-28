## Language
* ? => Can fail to connect if wifi if Installer was opened before logging in to Wifi
* keyboard layout
* keyboard language
* keyboard Test
* ? => Can fail to analyzse already active partitions
* OK connection => Can ask to look up for additional software ("Actiivate online repositories now? dialog box"
## List of Online Repositories
* [x] Main (updates)
* [x] Main (FOSS)
* [x] Main (non-FOSSS)
* [ ] Main (Sources)
* [ ] Main (DEBUG)
## System Role
* Plasma
* GNOME
* XFCE 
* Generic desktop
* Server
* Transactional server
## Suggested partitioning
* Guided (/!\ do not use that) -> Selection Hard Disk(s)
  * Choose what to do with the existing Windows systems (nothing / resize if needed / resize or remove if needed / remove even if not needed
  * Guided Choose what to do with the existing Linux / other partitions (nothing / remove if needed / remove even if not needed)
 * Expert: Start with current proposal (/!\ barely intelligble display)
 * Expert: Start with existing partitions (the only acceptable display, but requires some fine-tuning)
## Time zone 
## User creation
## Installation settings
* _Booting_ (/!\ hides Boot Code options, Kernel parameters, Bootloader options)
* _Software_ (/!\ hides all the software selection choices!!)
* _Default systemd target_
* _System_ (_System and Hardware Settings_)
* Security
    * _cpu mitigations_
    * firewall _on | off_
    * ssh service _on | off_
    * sh port blocked _y | n_ 
* Import SSH Host Keys and Configuration
* Network configuration
    * wicked (_switch to NetworkManager_) 