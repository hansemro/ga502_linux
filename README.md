# Readme


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
	BusID		"PCI:5:0:0"
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
