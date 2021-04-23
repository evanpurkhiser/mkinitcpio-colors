## mkinitcpio setcolors hook

Archlinux [mkinitcpio](https://wiki.archlinux.org/index.php/mkinitcpio) hook to
set the `Virtual Console` colors during early userspace.

Depends on [linux-vt-setcolors](https://github.com/EvanPurkhiser/linux-vt-setcolors).

### Usage

Install this by either using the [AUR](https://aur.archlinux.org/packages/setcolors-git) or by:
* copying the contents of `install/` to `/etc/initcpio/install/`
* copying the contents of `hooks/` to `/etc/initcpio/hooks/`
* copying `setcolors.service` to `/etc/systemd/system/`

Then, add to `HOOKS` in `/etc/mkinitcpio.conf` either `colors` or `sd-colors`
after `udev` or `systemd` respectively.

#### Configuration

Define colors in `/etc/vconsole.conf` with the format `COLOR_X=hexcode`, being
X a number between 0 and 15, like:

```sh
COLOR_0=000000 # black
COLOR_1=550000 # darkred
...
COLOR_15=ffffff # white
```

Regenerate the `initramfs` to apply the changes.

### Converting

The following sed command can be used to convert from a [setcolors configuration](https://github.com/EvanPurkhiser/linux-vt-setcolors/blob/master/example-colors/solarized)
to the `/etc/vconsole.conf` format.

```sh
$ sed 's/^\(.*\)#\(.\{6\}\).*$/COLOR_\1=\2/'
```
