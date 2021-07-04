### What is pipewire?

### Installing
Install the package:
```
sudo zypper in pipewire pipewire-pulseaudio
```
Then enable the pipwire audio socket: 
```
systemctl --user enable --now pipewire.{service,socket} pipewire-pulse.{service,socket}
```
Finally reboot your system:
```
systemctl reboot
```

### For KDE Plasma users
After rebooting, enable pipewire service making pipewire available to Plasma:
```
systemctl --user enable --now pipewire-media-session.service
```