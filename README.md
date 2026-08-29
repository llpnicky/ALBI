# artix-luks-base-install

#### You should always try to read/understand any code that you execute, especially installation scripts like these.

##### Questions(Issues)/PR's are welcome.

A fully self-contained Artix Linux base installation script that installs the
base system with an encrypted disk on /root and /swap using cryptsetup.

The installer is only configured to work with EFI+NVME systems and installs the runit
init system by default. By default it allocates 8G for /swap and
everything else for /root. This should be sufficient for most configurations.

Features:

- Simple menu to define password/user/hostname.
- NVME only.
- EFI only.
- Encrypted /root and /swap using LVM on LUKS encryption.
- Runit init system (replaces systemd).
- Configures defaults for locale, sudoers and networking.
- Well documented script.
- As little bloat as possible.

The following articles we're used for reference:

- <https://wiki.artixlinux.org/Main/Installation>
- <https://wiki.artixlinux.org/Main/InstallationWithFullDiskEncryption>
- <https://wiki.archlinux.org/title/installation_guide>
- <https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system#LVM_on_LUKS>

# Download.

You can download this script raw from github.

```
curl -LO https://raw.githubusercontent.com/llpnicky/ALBI/master/artix-luks-base-install
```

# Usage.

Run the script and use keys to navigate through the menu to change the default variables.

```
$ ./artix-luks-base-install
```

# Internet setup.

A cable is always recommended - it comes up automatically, no action needed.

For wireless: this fork no longer assumes ConnMan is present (recent Artix live
media often doesn't reliably ship it, which is what the old network check used to
depend on). Check what's actually on your ISO first:

```
command -v iwctl wpa_supplicant dhcpcd
```

Then, depending on what's there:

```
# iwd
iwctl station wlan0 connect "SSID"

# wpa_supplicant
wpa_supplicant -B -i wlan0 -c <(wpa_passphrase SSID PASSWORD) && dhcpcd wlan0
```

# Changes in this fork.

- Network check no longer depends on `connmanctl` — recent Artix live ISOs don't
  reliably ship it. The pre-flight check now does a plain `ping` to verify actual
  reachability instead of checking one specific tool's state.
- Refuses to install if the encryption, root, or user password is still the
  default `changeme`.
- The `parted` install step now fails with a clear message instead of continuing
  silently if pacman can't install it.
- Hardened the pre-install reset block: it now fully tears down a leftover
  `cryptlvm`/LVM mapping from a previous attempt (unscoped `swapoff`/`vgchange`,
  plus a `dmsetup remove --force` fallback), instead of silently swallowing a
  failed teardown and running into it later.
- Added `udevadm settle` calls after teardown and after partitioning, to close a
  suspected race between udev processing block-device events and the script
  repartitioning/reformatting in quick succession.

# Summary.

You should read the script, it is well documented and should be easy to understand.
Anything unclear can be found in the listed articles above.

If you have suggestions on improving the script, or need help with the
installation. Feel free to contribute/open issues.
