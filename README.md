# Readme

## New 400 MHz workaround
Update kernel to 5.6. Optionally remove the old workaround if boost is needed.

https://patchwork.kernel.org/patch/11292815/

https://patchwork.kernel.org/patch/11292813/

## ~~400 MHz workaround~~
[Apply gnox's boost workaround](https://bbs.archlinux.org/viewtopic.php?pid=1858966#p1858966)

In short:
* use bios version 207
* use amd_microcode
* disable boost (copy disable_boost.conf to /etc/tempfiles.d/)
* blacklist usci_ccg
* use nvidia power management rules (copy 80-nvidia-pm.rules to /etc/udev/rules.d/)

## AMD + NVIDIA using PRIME Render Offloading
1. Install nvidia-beta, nvidia-utils-beta, and patched xorg-server.

2. Create new Xorg config (/etc/X11/xorg.conf.d/10-monitor.conf) with the following:
```
Section "ServerLayout"
	Identifier	"layout"
	Option		"AllowNVIDIAGPUScreens"
EndSection

Section "Device"
	Identifier	"AMD"
	Driver		"amdgpu"
	Option		"DRI" "3"
	Option		"VariableRefresh" "true"
EndSection

Section "Screen"
	Identifier	"AMD"
	Device		"AMD"
EndSection

Section "Device"
	Identifier	"nvidia"
	Driver		"nvidia"
	Option		"AllowEmptyInitialConfiguration"
	BusID		"PCI:1:0:0"
EndSection

Section	"ServerFlags"
	Option		"IgnoreABI" "1"
EndSection
```

3. Remove 10-nvidia-drm-outputclass.conf (from /usr/share/X11/xorg.conf.d/) if the nvidia gpu is set as primary (it shouldn't).

### Installing patched xorg-server
For Arch Linux:
```
git clone https://gitlab.freedesktop.org/aplattner/arch-xorg-server.git
cd arch-xorg-server
makepkg -si
```

For Ubuntu 18.04/19.04: get the patched xorg-server from this ppa:
https://launchpad.net/~aplattner/+archive/ubuntu/ppa/

# Additional Resources
https://forum.manjaro.org/t/asus-rog-zephyrus-ga502du-installation-and-configuration-guide-updated-information-2-7-2020/118392
