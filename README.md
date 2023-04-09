Keyless Entry
=============

Easily switch on/off keyless reboot for ext4 Linux systems with LUKS encryption

Home page: <https://github.com/jikamens/keyless-entry>

Introduction
------------

LUKS filesystem encryption on Linux is great, but sometimes you want
to be able to reboot your system without needing to enter a decryption
key on reboot, e.g., if you need to reboot it remotely. This script
helps you do that.

The mechanism of this script is based on the approach that
[Tobias](https://askubuntu.com/users/344231/tobias) outlined in
<https://askubuntu.com/a/997668>. Thank you to Tobias for leading the
way!

This script is only known to work with ext4 filesystems, and probably
does not work with ZFS. Pull requests to remedy that will be
gratefully accepted.

Installation
------------

Copy the file `keyless-entry` to somewhere in your root filesystem, e.g., `/usr/local/bin`, and make sure it is executable.

You also need the `cryptsetup` utility to be installed, and you
probably also need the `cryptsetup` integration with `initramfs`. In
recent Ubuntu releases, you get these by installing the
`cryptsetup-bin` and `cryptsetup-initramfs` packages. You may need
other packages on other Linux distributions. If you know what's needed
for another OS, feel free to submit a PR to update this README.

Usage
-----

After installing the script, run it from its installed location with
one argument, `configure`. It will prompt you for your existing
decryption key once for every encrypted filesystem listed in
`/etc/crypttab`. After doing this _your system is not yet configured
to reboot without a key being typed._

To do that, run the script again, this time with the argument
`enable-once` or `enable-always`. As these names suggest, if you
specify `enable-once` then no key will be required for the next reboot
but subsequent reboots will require it.

You can specify `--default-kernel` with `enable-once` or
`enable-always` to only enable for the default kernel that grub will
use the next time it boots, or you can specify `--kernel-version` one
or more times to specify one or more kernel versions to enable for.
Otherwise enable and disable impact all installed kernels. The reason
why you would want to only enable for a subset of installed kernels is
because it makes enable and disable faster since fewer initial RAM
disks need to be rebuilt, and in the case of `enable-once`, this also
makes the next reboot finish faster (as noted below).

To disable keyless entry, run the script again with the argument
`disable`. To remove the configuration from your system as if you had
never set it up, run the script with the argument `unconfigure`.

If you end up in a bad state where keyless entry is partially enabled
and you want to restore a known state, run the script with the
argument `recover`, which disables keyless entry even if it isn't
fully enabled and rebuilds all the initial RAM disks.

### Important `enable-once` notes

* Your initial ramdisks are regenerated as part of the reboot process,
  so while the next reboot will not require a key to be entered by
  hand, the reboot will take somewhat longer than usual.

* Because the script configures `/etc/rc.local` to invoke itself
  during reboot, it's important to run the script from its installed
  location as specified above.

Credits
-------

Thank you to John Hancock <<john.m.hancock@gmail.com>> for helping to
make the script more secure by adding logic to use a temporary key
when enabling keyless entry along with a permanent key that's only
accessible on the encrypted filesystem.

Copyright
---------

Copyright 2022 Jonathan Kamens <<jik@kamens.us>>.

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or (at
your option) any later version.

This program is distributed in the hope that it will be useful, but
WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU
General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program. If not, see <<https://www.gnu.org/licenses/>>.
