Keyless Entry
=============

Easily switch on/off keyless reboot for Linux systems with LUKS encryption

Home page: <https://github.com/jikamens/keyless-entry>

Introduction
------------

LUKS filesystem encryption on Linux Linux is great, but sometimes you
want to be able to reboot your system without needing to enter a
decryption key on reboot, e.g., if you need to reboot it remotely.
This script helps you do that.

The mechanism of this script is based on the approach that Igor Mišić
outlined in <https://askubuntu.com/a/997668>. Thank you to Igor for
leading the way!

Installation
------------

Copy the file `keyless-entry` to somewhere in your root filesystem, e.g., `/usr/local/bin`, and make sure it is executable.

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

To disable keyless entry, run the script again with the argument
`disable`. To remove the configuration from your system as if you had
never set it up, run the script with the argument `unconfigure`.

### Important `enable-once` notes

* Your initial ramdisks are regenerated as part of the reboot process,
  so while the next reboot will not require a key to be entered by
  hand, the reboot will take somewhat longer than usual.

* Because the script configures `/etc/rc.local` to invoke itself
  during reboot, it's important to run the script from its installed
  location as specified above.

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
