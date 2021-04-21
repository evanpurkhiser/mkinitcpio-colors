## Mkinitcpio colors hook

Use this [mkinitcpio](https://wiki.archlinux.org/index.php/Mkinitcpio) hook to
set the virtual terminal colors during early userspace.

Requires the [setcolors](https://github.com/EvanPurkhiser/linux-vt-setcolors) utility.

### Usage

If you have successfully installed this package, either by using the corresponding `AUR` repository, or by
* copying the contents of the `install` directory to `/usr/lib/initcpio/install`
* copying the contents of the `hooks` directory to `/usr/lib/initcpio/hooks`
* copying the `setcolors.service` file to `/usr/lib/systemd/system`
you can proceed to actually use the `mkinitcpio` hook

First you have to determine, wether you're using a `busybox` or `systemd` based `initramfs`, though.

#### How to tell

The default one is `busybox` for all Arch Linux installations done from an official `archiso`. So at the time of writing, you should know that you're using a `systemd` based `initramfs`, as you would have to actively edit `/etc/mkinitcpio.conf`.

Generally speaking and with no guarantee regarding exceptional cases, if you have the `udev` hook present in your `HOOKS` list in `/etc/mkinitcpio.conf`, you're using a `busybox` based initramfs, if you have the `systemd` hook present, you're using a `systemd` one.

#### `busybox`

Add the `colors` hook to your `/etc/mkinitcpio.conf` `HOOKS` list. You
will probably want to place the hook fairly eairly if you don't want the colors
to abruptly change.

#### `systemd`

Add the `sd-colors` hook to your `/etc/mkinitcpio.conf` `HOOKS` list. You
will probably want to place the hook fairly eairly if you don't want the colors
to abruptly change, but you will definitely want to place it after the `systemd` hook.

You will then need to enable the `setcolors.service` `systemd` service

```
# systemctl enable setcolors.service
```

#### In any case

To define colors you will want to edit your `/etc/vconsole.conf` file and
specify the colors in the format `COLOR_X=hexcode`. Where X is a number between
0 and 15. For example:

```sh
COLOR_0=ff0000
COLOR_1=00ff00
...
COLOR_15=0000ff
```

You will need to run `mkinitcpio -p linux` each time you make changes to your
color configuration.

### Converting from `setcolors` format

You may use this sed command to convert from a [setcolors color configuration
file](https://github.com/EvanPurkhiser/linux-vt-setcolors/blob/master/example-colors/solarized)
to the required format for the `/etc/vconsole.conf` file.

```sh
$ sed 's/^\(.*\)#\(.\{6\}\).*$/COLOR_\1=\2/'
```
