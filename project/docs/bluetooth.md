## Pairing a Bluetooth device across different operating systems

### With Windows 10 (adapted from [StackExchange](https://unix.stackexchange.com/questions/255509/bluetooth-pairing-on-dual-boot-of-windows-linux-mint-ubuntu-stop-having-to-p))
1. Pair all devices on Linux 
2. Pair all devices on Windows
3. From Windows, go to _Device & Printers_ in _Control Panel_ and go to your Bluetooth device's properties. Then, in the _Bluetooth_ section, you can find the unique identifier. Copy it as you will need it.
4. Download __PsExec__ from [here](http://technet.microsoft.com/en-us/sysinternals/bb897553.aspx).
5. Unzip the zip you downloaded and open a _CMD_ window with elevated privileges. (Click the Start menu, search for cmd, then right-click the CMD and click __Run as Administrator__.)
6. `cd` into the folder where you unzipped your download.
7. Run `psexec -s -i regedit.exe`
8. Navigate to find the keys under `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\BTHPORT\Parameters\Keys`.  If there is no _CurrentControlSet_, try _ControlSet001_.
9. You should see a few keys labels with the MAC addresses -- write down the MAC address associated with the unique identifier you copied before.
10. Go back to Linux (if not in Linux) and add your Windows key to your Linux config entries. Just note that the Bluetooth port's MAC address is formatted differently when moving from Windows to Linux - referenced as aa1122334455 in Windows in my example above. The Linux version will be in all caps and punctuated by ':' after every two characters - for example `AA:11:22:33:44:55`.  
11. Switch to root: do `sudo -s`
12. `cd` to your Bluetooth config location at `/var/lib/bluetooth/[bth port  MAC addresses]`
13. Here you'll find folders for each device you've paired with. The folder names being the Bluetooth devices' MAC addresses and contain a single file info. In these files, you'll see the link key you need to replace with your Windows ones, like so:
```
[LinkKey]
Key=< NEW KEY, ex. B99999999FFFFFFFFF999999999FFFFF >
```
14. Finally restart your bluetooth systemd service with `sudo systemctl restart bluetooth`
