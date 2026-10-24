# sxstatus

A very simple status bar script for [dwm](https://dwm.suckless.org/), written in bash. Each item on the bar is a small standalone module, so adding or removing one is just editing a single line.

<p align="center">
<img src="screenshots/sxstatus.png" alt="sxstatus screenshot" width="800">
</p>

## Modules

| Module | Shows |
|---|---|
| `media_sx` | current song from any MPRIS player (browsers ignored) |
| `memory_sx` | memory in use |
| `battery_sx` | battery level, icon follows charge, bolt while charging |
| `volume_sx` | volume, or a muted icon |
| `wifi_sx` | LAN if a cable is connected, otherwise the wifi network |
| `bt_sx` | bluetooth state and connected device |
| `clock_sx` | date and time |
| `mpdstat_sx` | current song from MPD |
| `package_sx` | number of installed packages |
| `update_sx` | number of upgradable packages |
| `temp_sx` | cpu temperature |
| `uptime_sx` | system uptime |

The default status line uses the first seven. `package_sx` and `update_sx` are heavier and increase cpu usage, so they are left out by default. `temp_sx` reads `thermal_zone0`, which some laptops do not have.

## Dependencies

Requires dwm with the [status2d](https://dwm.suckless.org/patches/status2d/) patch, since the modules print color codes.

    sudo apt install playerctl wireplumber network-manager bluez rfkill fonts-font-awesome dmenu

Icons come from Font Awesome, so it has to be in the `fonts[]` array of your dwm `config.h`.

## Installation

    cd ~/.config
    git clone https://github.com/vq6oon/sxstatus.git
    cd sxstatus
    chmod 755 -R modules/ menus/
    make
    sudo make install

Then start it from your session file, for example `~/.xsessionrc`:

    sxstatus &

The `sxstatus` script itself is installed to `/usr/bin`, but the modules are read from `~/.config/sxstatus/modules`. After editing the `sxstatus` script, run `sudo make install` again. Editing a module needs no reinstall, only a restart of `sxstatus`.

## Customizing

The status line is one `xsetroot` line in the `sxstatus` script. Reorder or remove modules there:

    xsetroot -name "$(memory_sx) $(battery_sx) $(volume_sx) $(clock_sx)"

To add a module, drop an executable script in `modules/` that prints one line, and call it from that line.

## Wifi and bluetooth menus

`menus/wifi_menu` and `menus/bt_menu` are dmenu menus to toggle the radios, pick a wifi network and connect bluetooth devices. The bar itself is not clickable, so bind them to keys in dwm:

    static const char *wifimenu[] = { "/bin/sh", "-c", "$HOME/.config/sxstatus/menus/wifi_menu", NULL };
    static const char *btmenu[]   = { "/bin/sh", "-c", "$HOME/.config/sxstatus/menus/bt_menu", NULL };

## Colors

Modules take their text color from dwm's `dwm.normfgcolor` X resource, so the status bar matches the dwm bar. With the pywal setup from my [dwm](https://github.com/vq6oon/dwm) repo, it follows the wallpaper. Without it, a fixed fallback color is used.

## Credits

Based on [sxstatus](https://github.com/diws1/sxstatus) by diws1. Original license is in `LICENSE`.