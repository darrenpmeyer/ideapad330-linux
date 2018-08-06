# ideapad330-linux
Tricks, tweaks, and fixes for running Debian/Ubuntu-based distros on the Lenovo ideapad330


## Hardware support

Ubuntu 18.04 and distributions based upon it support most of the Ideapad 330 hardware out of the box. The major exception is the ELAN touchpad.

### ELAN 601x touchpad support

The only thing in the way of full touchpad support on most ideapad 330 models is the lack of an ACPI id in the correct kernel module.

**Diagnose:**

```
sudo apt install -y acpidump
sudo acpidump | grep -e '\.ELAN061[CD]\.'
```

The above should return something like:

```
  0EA0: 0D 45 4C 41 4E 30 36 31 43 00 5F 48 49 44 70 0A  .ELAN061C._HIDp.
```

If it *does not*, then there's no fix I'm aware of. If it *does*, then all you need is a patched kernel. The basic instructions can be found on [an Ask Ubuntu thread](https://askubuntu.com/questions/1049787/lenovo-ideapad-330-touchpad-not-working/), including instructions for compiling the modified kernel. Various things about the dependencies required are incomplete in that guide, but the modification instructions are correct.

If you don't wish to compile it yourself, this repository provides a pre-compiled version:

```
wget -nd https://github.com/darrenpmeyer/ideapad330-linux/raw/master/bin/linux-image-4.18.0-rc3-custom_4.18.0-rc3-custom-1_amd64.deb
sudo dpkg -i linux-image-4.18.0-rc3-custom_4.18.0-rc3-custom-1_amd64.deb
```

After rebooting, the on-board touchpad should function.

### YubiKey U2F support

`udev` rules need to be added for U2F support (**Note:** U2F support is only guaranteed to work in the Google Chrome browser. I've had luck with some builds of Chromium, but there's not guarantee.)

```
wget -nd https://github.com/Yubico/libu2f-host/raw/master/70-u2f.rules
sudo mv 70-u2f.rules /etc/udev/rules.d/
```

Depending on your configuration, a reboot may be required

