# ALBI — Artix Linux Base Installer

A small Bash script for installing a minimal, **encrypted Artix Linux system with runit**. It installs the operating system and leaves desktop setup and dotfiles for later.

**Read the script before running it. It erases the entire disk you select.** Back up anything important first.

## What it installs

- **UEFI only** (not legacy BIOS); supports SATA, NVMe, and MMC drives.
- A 512 MiB unencrypted EFI partition for booting.
- **LUKS2 encryption** for the rest of the disk, with LVM inside it:
  - An ext4 root filesystem using the remaining space.
  - An 8 GiB swap volume.
- **Artix + runit**, GRUB, NetworkManager, sudo, and basic command-line tools.
- A user account with sudo access and Zsh as its shell.
- **Chezmoi**, if it's available in the enabled package repositories.

It **does not** install a desktop environment or apply anyone's dotfiles.

## Installation

1. Boot an **Artix runit live USB in UEFI mode** and connect to the internet.
2. Open a root shell. Check the disks with `lsblk` and make sure you've backed up the one you intend to erase.
3. Download and read the installer:

   ```sh
   curl -LO https://raw.githubusercontent.com/llpnicky/ALBI/master/artix-luks-base-install
   less artix-luks-base-install
   ```

4. Run it as root:

   ```sh
   bash artix-luks-base-install
   ```

5. Enter the **disk**, **username**, and **hostname** when asked. You must type an explicit confirmation before the disk is erased.
6. Set your **disk encryption**, **root**, and **user** passwords when prompted.
7. After installation finishes successfully, reboot and log in with your new account.

The installer does **not** reboot the computer automatically.

### Internet on the live USB

A wired connection is usually easiest. For Wi-Fi, check which network tools your Artix ISO includes. If it has `iwctl`, for example:

```sh
iwctl device list
iwctl station YOUR_DEVICE connect "YOUR_WIFI_NAME"
```

Replace `YOUR_DEVICE` with the name shown by the first command. The live ISO must have internet access to download packages.

## After reboot

ALBI only installs the base system. Configure your desktop and dotfiles separately.

If chezmoi wasn't available during installation, install it using a repository that provides it or follow the [official chezmoi instructions](https://www.chezmoi.io/install/). You can then initialize your own dotfiles repository as your regular user.

## Defaults

The script currently uses **`en_US.UTF-8`**, **`America/Los_Angeles`**, an LVM group named **`VGArtix`**, and a LUKS mapping named **`cryptlvm`**. Change these values in the script before running it if needed.

**Testing:** This revised installer has passed Bash syntax and partition-name checks, but it has **not yet been tested on a physical Artix installation**. Review it carefully and test on a spare drive or virtual machine first.

## References

- [Artix installation guide](https://wiki.artixlinux.org/Main/Installation)
- [Artix full-disk encryption](https://wiki.artixlinux.org/Main/InstallationWithFullDiskEncryption)
- [Arch installation guide](https://wiki.archlinux.org/title/Installation_guide)
- [LVM on LUKS](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system#LVM_on_LUKS)

Questions, issues, and pull requests are welcome.
