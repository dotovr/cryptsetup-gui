cryptsetup-gui is a fairly simple application for unlocking and mounting LUKS
paritions with a GUI. The intended use is for mounting an encrypted home
partition after login, but it can be used for any encrypted partition.

It is invoked with the name of an encrypted partition, looks up that name in
/etc/crypttab to find the device, prompts the user for a password, decrypts the
device and mounts it according to /etc/fstab.

### Mounting /home on login ###
In order to enable the mounting of /home AFTER a user has logged in, we employ a
little trick:

  1. The user's home directory is still set to /home/#username#.
  2. We create this directory when /home is unmounted
  3. We add a .xinitrc file that calls cryptsetup-gui, mounts /home on top of
     us, and then executes the "real" user .xinitrc

The included xinitrc file should take care of this, and is installed into
/etc/skel. It defaults to the name of the encrypted home partition to be
*chome*, but this can be changed by modifying the file after copying it to your
unmounted home directory.

### Security ###
**Note:** The binary cryptsetup-gui installed into /usr/bin has the setuid
flag enabled and is owned by root. This is so that it can be invoked by users
and still be permitted to unlock a device. This is done on your own risk!

The application only accepts a single parameter, the name of the encrypted
device, and this is enforced to be only a-z. Thus, I do not believe there to be
any risk of that causing a security risk with setuid. The file does execute two
shell commands using cryptsetup and luks, however the parameters for these are
read from /etc/crypttab and /etc/fstab. Thus, **should a malicious user be able
to modify either of these files, he/she will be able to use cryptsetup-gui to
run arbitrary commands as root**. That said, if they can edit those files, you
already have a security breach.

### License ###
*It's the standard MIT license.*

Copyright (C) 2012 Jon Gjengset

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
of the Software, and to permit persons to whom the Software is furnished to do
so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
<!-- vim:set textwidth=80: -->
