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
- [https://wiki.artixlinux.org/Main/Installation](https://wiki.artixlinux.org/Main/Installation)
- [https://wiki.artixlinux.org/Main/InstallationWithFullDiskEncryption](https://wiki.artixlinux.org/Main/InstallationWithFullDiskEncryption)
- [https://wiki.archlinux.org/title/installation_guide](https://wiki.archlinux.org/title/installation_guide)
- [https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system#LVM_on_LUKS](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system#LVM_on_LUKS)


# Download.
You can download this script raw from github.
```console
curl -LO https://raw.githubusercontent.com/llpnicky/artix-luks-base-install/master/artix-luks-base-install
```

# Usage.
Run the script and use keys to navigate through the menu to change the default variables.
```console
$ ./artix-luks-base-install
```

# Internet setup with Connmanctl.
A cable is always recommended, alternatively:
```console
connmanctl
> agent on
> enable wifi
> scan wifi
...
Scan Completed
> services
... A long list of services SSIDS & codes ...

> connect <NETWORK ID>
Enter Password: ******
...
Connected
> exit
```

# Summary.
You should read the script, it is well documented and should be easy to understand.
Anything unclear can be found in the listed articles above.

If you have suggestions on improving the script, or need help with the
installation. Feel free to contribute/open issues.
