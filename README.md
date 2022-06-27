This is meant as a list of things to check after following the official Arch Linux installation guide.
Most of these things can apply to other minimal distributions.

Previously I've attempted to maintain a system setup script.
The advantages of this system is a repeatable automated installation.
In practice I've found that I rarely want systems to be identical and updating the script is too much work to be worth the trouble.
Also it's nice to set up systems manually and know exactly which commands have been run.

# Packages

## Basic

Can't live without these.

Arch Linux: `avahi base-devel dash git man-db neovim opendoas openssh terminus-font`

 - Note: sudo is installed as a dependency of base-devel.
 - Install xclip or wl-clipboard, depending on which protocol is used.

## X11 deps

When using `dwm`, install these.

Arch Linux: `libx11 libxft libxinerama xclip xorg-server xorg-xev xorg-xinit xorg-xkill xorg-xprop xorg-xrandr xorg-xsetroot`

# Config files `/etc/`

## /etc/doas.conf

Example configuration

```
permit persist :wheel
permit nopass :wheel cmd pacman args -Sy
permit nopass :storage cmd mount
permit nopass :storage cmd umount
```

## /etc/locale.conf

Default units can be set here.

```
LC_MEASUREMENT=fi_FI.UTF-8
LC_MONETARY=fi_FI.UTF-8
```

## /etc/makepkg.conf

Set `MAKEFLAGS` to `-j$(nproc)"`

## /etc/pacman.conf

- Uncomment Color
- Add ILoveCandy
- Optionally enable VerbosePkgList for columnated package listing on large monitors
- Optionally enable ParallelDownloads

# Enable local hostname resolution

Arch Linux:

- `systemctl enable avahi-daemon.service`
- Edit `/etc/nsswitch.conf` and add `mdns_minimal [NOTFOUND=return]` before resolve
- Reboot (is starting service enough?)

# Enable dash shell

Arch Linux: `ln -fs /bin/dash /bin/sh`

# Enable printer service

Arch Linux: `systemctl enable cups.socket`

# Install `yay`

`git clone https://aur.archlinux.org/yay.git`

# X11 setup

## Touchpad

```
# /etc/X11/xorg.conf.d/30-touchpad.conf
Section "InputClass"
	Identifier "touchpad"
	Driver "libinput"
	MatchIsTouchpad "on"
	Option "Tapping" "on"
	Option "ClickMethod" "clickfinger"
	Option "NaturalScrolling" "true"
	Option "AccelSpeed" "0.0"
EndSection
```

## Backlight

This may be required when using simple window managers

```
# /etc/udev/rules.d
ACTION=="add", SUBSYSTEM=="backlight", RUN+="/bin/chgrp video /sys/class/backlight/%k/brightness"
ACTION=="add", SUBSYSTEM=="backlight", RUN+="/bin/chmod g+w /sys/class/backlight/%k/brightness"
```
