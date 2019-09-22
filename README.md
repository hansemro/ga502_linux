# Readme

## 400 MHz workaround
[Apply gnox's boost workaround](https://bbs.archlinux.org/viewtopic.php?pid=1858966#p1858966)

In short:
* use bios version 207
* use amd_microcode
* disable boost (copy disable_boost.conf to /etc/tempfiles.d/)
* blacklist usci_ccg
* use nvidia power management rules (copy 80-nvidia-pm.rules to /etc/udev/rules.d/)

## AMD + NVIDIA using PRIME Render Offloading
1. Install nvidia-beta (AUR), nvidia-utils-beta (AUR), and patched xorg-server.

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

Remove 10-nvidia-drm-outputclass.conf (from /usr/share/X11/xorg.conf.d/) if the nvidia gpu is set as primary (it shouldn't).
