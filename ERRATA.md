# FreeBSD

## Hosts which underwent an upgrade from 13.0 to 13.1

On hosts which have upgraded from 13.0 to 13.1, the virtualbox driver gives the following error:
```
VBoxHeadless: Error -1908 in suplibOsInit!
VBoxHeadless: Kernel driver not installed

VBoxHeadless: Tip! Make sure the kernel module is loaded. It may also help to reinstall VirtualBox.
```

With the September quarterlies, and Virtualbox version 6.1.36, you will need to remove the driver and uninstall Virtualbox, then reboot.

```
kldunload vboxdrv
pkg delete -f virtualbox-ose virtualbox-ose-kmod 
```

Now reboot, and log back in as root.

```
pkg install -y virtualbox-ose virtualbox-ose-kmod
kldload vboxdrv
mkdir -p /etc/vbox
mkdir -p /usr/local/etc/vbox
echo "* 0.0.0.0/0" > /usr/local/etc/vbox/networks.conf
ln -s /usr/local/etc/vbox/networks.conf /etc/vbox/networks.conf
service vboxnet restart
```

The `packbox` and `startvms` commands should work fine now.

## FreeBSD 13.1 uses /usr/local/etc/vbox

While most `virtualbox` installations make use of `/etc/vbox`, including older FreeBSD versions 12.x, 13.0, from FreeBSD 13.1 `/usr/local/etc/vbox/` is used and `/etc/box/` shouldn't exist.

However if `/etc/vbox` doesn't exist there is an error. You can create the directory and symlink across `/usr/local/etc/vbox/networks.conf`, and you'll still get an error as follows. You can safely ignore it.
```
packer-builder-virtualbox-iso plugin: stderr: WARNING: Directory /etc/vbox found, but ignored. VirtualBox
configuration files are stored in /usr/local/etc/vbox/.
```

