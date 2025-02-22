### What is pipewire?

According to the [pipewire website](https://pipewire.org/)

> [It] is a project that aims to greatly improve handling of audio and video under Linux. It provides a low-latency, graph based processing engine on top of audio and video devices that can be used to support the use cases currently handled by both pulseaudio and JACK. PipeWire was designed with a powerful security model that makes interacting with audio and video devices from containerized applications easy, with supporting Flatpak applications being the primary goal. Alongside Wayland and Flatpak we expect PipeWire to provide a core building block for the future of Linux application development.

TLDR; Pipewire is a new multimedia framework that aims for minimal latency and improved security with support for Pulseaudio, JACK, ALSA, and GStreamer.

### Installing

!!! warning 
	This removes the package `pulseaudio`.

Install the package:

```
sudo zypper in pipewire pipewire-pulseaudio
```

Then enable pipewire sockets and session manager:

```
systemctl --user enable --now pipewire.socket
systemctl --user enable --now pipewire-pulse.socket
systemctl --user enable --now wireplumber.service
```

Pipewire services will be started automatically when needed.
