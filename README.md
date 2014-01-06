## Mkinitcpio colors hook

Use this [mkinitcpio](https://wiki.archlinux.org/index.php/Mkinitcpio) hook to
set the virtual terminal colors during early userspace.

Requires the [setcolors](https://github.com/EvanPurkhiser/linux-vt-setcolors) utility.

### Usage

First add the `colors` hook to your `/etc/mkinitcpio.conf` files HOOKS list. You
will probably want to place the hook fairly eairly if you don't want the colors
to abruptly change.

To define colors you will want to edit your `/etc/vconsole.conf` file and
specify the colors in the format `XOLOR_X=hexcode`. Where X is a number between
0 and 15.

For example:

````sh
COLOR_0=ff0000
COLOR_1=00ff00
...
```

### Converting from `setcolors` format

You may use this sed command to convert from a [setcolors color configuration
file](https://github.com/EvanPurkhiser/linux-vt-setcolors/blob/master/example-colors/solarized)
to the required format for the `/etc/vconsole.conf` file.

````sh
$ sed 's/^\(.*\)#\(.\{6\}\).*$/COLOR_\1=\2/'
```
