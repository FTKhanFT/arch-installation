<div align="center">
  <p>
    <h1>
      <a href="#readme">
        <img src="https://www.archlinux.org/static/logos/archlinux-logo-dark-1200dpi.b42bd35d5916.png" alt="Arch Logo" />
      </a>
      <br />
      Arch Linux
    </h1>
    <h2>Installation instructions</h2>
  </p>
  <p>
    <a href="https://github.com/D3S0X/arch-installation/">
      <img src="https://img.shields.io/github/last-commit/D3S0X/arch-installation.svg?style=for-the-badge&label=Last%20update" alt="Last Commit" />
    </a>
  </p>
</div>

> This tutorial is inspired by <https://sourceforge.net/projects/ezos/files/ArchStuff/>

## Index
- [1. Live Setup](#1-live-setup)
    - [Set keyboard layout](#set-keyboard-layout)
    - [If WiFi install](#if-wifi-install)
    - [Sync time](#sync-time)
    - [Check if booted in BIOS or UEFI](#check-if-booted-in-bios-or-uefi)
- [2. Partitioning](#2-partitioning)
  - [Partitioning](#partitioning)
    - [Start partitioning tool](#start-partitioning-tool)
    - [Create partitions](#create-partitions)
      - [Decide partition table type](#decide-partition-table-type)
      - [GPT (UEFI)](#gpt-uefi)
      - [DOS (BIOS)](#dos-bios)
    - [Size recommendations](#size-recommendations)
      - [EFI system](#efi-system)
      - [Swap](#swap)
  - [Formatting partitions](#formatting-partitions)
- [3. Mounting accordingly](#3-mounting-accordingly)
- [4. Base installation](#4-base-installation)
  - [Rank the mirrors before for faster downloads](#rank-the-mirrors-before-for-faster-downloads)
  - [Start the installation](#start-the-installation)
    - [Create filesystem table](#create-filesystem-table)
    - [Change root](#change-root)
- [5. Install Bootloader](#5-install-bootloader)
- [6. Setup time/date and languages](#6-setup-timedate-and-languages)
  - [Setup hostname](#setup-hostname)
  - [Setup locale](#setup-locale)
  - [Setup time & date](#setup-time--date)
  - [Setup multilib](#setup-multilib)
- [7. Setup users](#7-setup-users)
  - [Setup users](#setup-users)
    - [Set root password](#set-root-password)
    - [Add your user](#add-your-user)
    - [Enable sudo](#enable-sudo)
- [8. Install some useful packages](#8-install-some-useful-packages)
  - [General packages](#general-packages)
  - [Printer support](#printer-support)
    - [General packages:](#general-packages)
    - [UI for HP Printers:](#ui-for-hp-printers)
  - [Display Server:](#display-server)
- [9. Install Desktop Environment](#9-install-desktop-environment)
    - [LXDE:](#lxde)
    - [LXQt:](#lxqt)
    - [GNOME:](#gnome)
    - [Cinnamon:](#cinnamon)
    - [KDE Plasma:](#kde-plasma)
    - [Xfce:](#xfce)
    - [Budgie:](#budgie)
    - [Mate:](#mate)
    - [Deepin:](#deepin)
- [10. Install and enable Desktop Manager and some other useful stuff](#10-install-and-enable-desktop-manager-and-some-other-useful-stuff)
  - [Display Manager](#display-manager)
    - [LXDM (Included in LXDE)](#lxdm-included-in-lxde)
    - [SDDM (Included in KDE Plasma)](#sddm-included-in-kde-plasma)
    - [GDM (Included with GNOME)](#gdm-included-with-gnome)
    - [LightDM](#lightdm)
  - [Graphics Driver](#graphics-driver)
    - [Open Source drivers:](#open-source-drivers)
    - [Nvidia proprietary driver:](#nvidia-proprietary-driver)
    - [AMD Utils:](#amd-utils)
  - [Networking](#networking)
    - [If you use WiFi:](#if-you-use-wifi)
  - [Some archive and file system utils](#some-archive-and-file-system-utils)
  - [Sound](#sound)
  - [Other shells](#other-shells)
    - [zsh (Z Shell)](#zsh-z-shell)
    - [fish (Friendly interactive shell)](#fish-friendly-interactive-shell)
- [11. Reboot](#11-reboot)
- [12. Post installation](#12-post-installation)
  - [Set X11 Keymap](#set-x11-keymap)
  - [WiFi](#wifi)
  - [Oh my zsh](#oh-my-zsh)
  - [AUR Setup](#aur-setup)
  - [If not all user dir's are present](#if-not-all-user-dirs-are-present)
  - [If you want a graphical package manager](#if-you-want-a-graphical-package-manager)
  - [If you use a GTK desktop and want Qt apps to use your GTK Theme](#if-you-use-a-gtk-desktop-and-want-qt-apps-to-use-your-gtk-theme)
  - [If you want to read APFS Partitions](#if-you-want-to-read-apfs-partitions)
  - [Graphics card configuration tool](#graphics-card-configuration-tool)
    - [AMD](#amd)
    - [NVIDIA](#nvidia)
  - [Fonts:](#fonts)
    - [General Fonts](#general-fonts)
    - [Nerd Fonts](#nerd-fonts)
    - [Windows Fonts](#windows-fonts)
    - [macOS Fonts](#macos-fonts)
  - [Nano syntax highlighting](#nano-syntax-highlighting)
  - [Auto clean package cache](#auto-clean-package-cache)
- [13. Some fixes and tweaks](#13-some-fixes-and-tweaks)
  - [Compability tweaks](#compability-tweaks)
    - [Spotify local files](#spotify-local-files)
  - [Fix on shutdown "Failed to start user manager service for user 174" (sddm)](#fix-on-shutdown-%22failed-to-start-user-manager-service-for-user-174%22-sddm)
  - [Force Google Emoji](#force-google-emoji)
  - [Desktop icons for nemo](#desktop-icons-for-nemo)

# 1. Live Setup
### Set keyboard layout
⌨️ The default keymap is US. Available layouts can be listed with:
```
ls /usr/share/kbd/keymaps/**/*.map.gz
```
```
loadkeys de-latin1
```

### If WiFi install
📶 Use this tool to connect to your network
```
wifi-menu
```

### Sync time
🕒 Ensure the system clock is accurate
```
timedatectl set-ntp true
```

### Check if booted in BIOS or UEFI
```
ls /sys/firmware/efi/efivars
```
If the directory does not exist, the system may be booted in Legacy BIOS Mode

# 2. Partitioning

## Partitioning

### Start partitioning tool
BIOS:
```
fdisk /dev/sdX
```
UEFI:
```
gdisk /dev/sdX
```
Universal + Graphical:
```
cfdisk /dev/sdX
```

### Create partitions

#### Decide partition table type
 - BIOS: You can use both but this guide uses DOS
 - UEFI: You need to use GPT

#### GPT (UEFI)

| Needed | Partition | Partition type       | Mount point   |
|--------|-----------|----------------------|---------------|
| ✔️      | /dev/sdXY | EFI system partition | /mnt/boot/EFI |
| ❌      | /dev/sdXY | Linux swap           | -             |
| ✔️      | /dev/sdXY | Linux                | /mnt          |
| ❌      | /dev/sdXY | Linux                | /mnt/home     |

#### DOS (BIOS)

| Needed | Partition | Partition type | Mount point | Flags    |
|--------|-----------|----------------|-------------|----------|
| ❌      | /dev/sdXY | Linux swap     | -           | -        |
| ✔️      | /dev/sdXY | Linux          | /mnt        | Bootable |
| ❌      | /dev/sdXY | Linux          | /mnt/home   | -        |

### Size recommendations

#### EFI system
- At least: 150MB
- Recommended: 300MB

#### Swap

Taken from <https://docs.voidlinux.org/installation/live-images/partitions.html>

| System RAM | Recommended swap space | Swap space if using hibernation |
|------------|------------------------|---------------------------------|
| < 2GB      | 2x the amount of RAM   | 3x the amount of RAM            |
| 2-8GB      | Equal to amount of RAM | 2x the amount of RAM            |
| 8-64GB     | At least 4GB           | 1.5x the amount of RAM          |
| 64GB       | At least 4GB           | Hibernation not recommended     |

## Formatting partitions

EFI system partition:
```
mkfs.fat -F32 -n EFI /dev/sdXY
```

💽 Create root filesystem:
```
mkfs.ext4 -L ROOT /dev/sdXY
```

🏠 If you use a separate home partition:
```
mkfs.ext4 -L HOME /dev/sdXY
```

Create Swap:
```
mkswap -L SWAP /dev/sdXY
swapon /dev/sdXY
```

# 3. Mounting accordingly
💽 Mount root filesystem:
```
mount /dev/sdXY /mnt
```

UEFI:
```
mkdir -p /mnt/boot/EFI
mount /dev/sdXY /mnt/boot/EFI
```

🏠 If seperate home partiton:
```
mkdir /mnt/home
mount /dev/sdXY /mnt/home
```

# 4. Base installation

## Rank the mirrors before for faster downloads
📊 This will rank the mirrorlist. You may replace United States with your country
```
pacman -Sy archlinux-keyring reflector
reflector --country 'United States' --age 15 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```

## Start the installation
⏳ This will install the system and may take a while
```
pacstrap /mnt base base-devel linux linux-firmware linux-lts sysfsutils usbutils e2fsprogs inetutils netctl nano less which man-db man-pages
```

### Create filesystem table
Identify by UUID (better):
```
genfstab -U /mnt >> /mnt/etc/fstab
```
Identify by Labels:
```
genfstab -L /mnt >> /mnt/etc/fstab
```

### Change root
```
arch-chroot /mnt
```

# 5. Install Bootloader
▶️ UEFI:
```
pacman -S grub os-prober efibootmgr dosfstools mtools fatresize
grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck
grub-mkconfig -o /boot/grub/grub.cfg
```

▶️ BIOS:
```
pacman -S grub os-prober
grub-install --target=i386-pc --recheck /dev/sdX
grub-mkconfig -o /boot/grub/grub.cfg
```

💾 Create initial ramdisk for LTS kernel
```
mkinitcpio -p linux-lts
```

# 6. Setup time/date and languages

## Setup hostname
📛 This will the name of your PC on your network
```
echo myhostname > /etc/hostname
nano /etc/hosts
```
Add these lines
```
127.0.0.1   localhost
::1         localhost
127.0.1.1   myhostname.localdomain  myhostname
```

## Setup locale
🌐 Uncomment all languages you need
```
nano /etc/locale.gen
```
Generate locales
```
locale-gen
```
🔘 Set locale
```
echo LANG=en_US.UTF-8 >> /etc/locale.conf
export LANG=en_US.UTF-8
```
⌨️ Set tty keymap
```
echo KEYMAP=de-latin1 > /etc/vconsole.conf
```

## Setup time & date

📅 You tab-complete your stuff after zoneinfo
```
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
hwclock --systohc --utc
```

## Setup multilib
```
nano /etc/pacman.conf
```
Uncomment multilib (🍬 and add ILoveCandy to Misc section)
```
pacman -Syu
```

# 7. Setup users

## Setup users

### Set root password
🔑 Use a strong and complicated password
```
passwd
```

### Add your user
🧑 This will be your user you use to log in
```
useradd -m -G users,wheel,audio,video,storage,power,input,optical,sys,log,network,floppy,scanner,rfkill,lp,adm -s /bin/bash yourusername
passwd yourusername
```
If you want to force your user to change password after first login:
```
chage -d 0 yourusername
```

### Enable sudo
```
EDITOR=nano visudo
```
Uncomment ```%wheel ALL=(ALL) ALL```

# 8. Install some useful packages

## General packages
```
pacman -S linux-headers linux-lts-headers dkms
pacman -S jshon expac git wget acpid avahi xdotool pacman-contrib net-tools
systemctl enable acpid avahi-daemon systemd-timesyncd
```

## Printer support
🖨️ Add some packages needed for printing
### General packages:
```
pacman -S system-config-printer foomatic-db foomatic-db-engine gutenprint gsfonts cups cups-pdf cups-filters sane simple-scan
systemctl enable org.cups.cupsd.service saned.socket
```
### UI for HP Printers:
```
pacman -S hplip
```

## Display Server:
🖥️ Xorg is the display server we will use
```
pacman -S xorg-server xorg-xinit xorg-xrandr xorg-xfontsel xorg-xkill
```

# 9. Install Desktop Environment

🗔 You need to select a desktop environment

- For beginners coming from Windows I recommend KDE Plasma.
- For a very resource friendy desktop I recommend Xfce
- The instructions for KDE Plasma are tested by me because I use it. Others should work but you may need some extra packages for productive use

### LXDE:
```
pacman -S lxde
```
### LXQt:
```
pacman -S lxqt breeze-icons pcmanfm-qt qterminal lxqt-sudo polkit-qt5
```
### GNOME:
```
pacman -S gnome gnome-extra
```
### Cinnamon:
```
pacman -S cinnamon nemo-fileroller
```
### KDE Plasma:
```
pacman -S plasma plasma-wayland-session konsole dolphin gwenview ark kate okular
```
### Xfce:
```
pacman -S xfce4 xfce4-goodies
```
### Budgie:
```
pacman -S budgie-desktop gnome
```
### Mate:
```
pacman -S mate mate-extra
```
### Deepin:
```
pacman -S deepin deepin-extra
nano /etc/lightdm/lightdm.conf
greeter-session=lightdm-deepin-greeter
```

# 10.  Install and enable Desktop Manager and some other useful stuff

## Display Manager
🖥️ A display manager is basically your login screen where you enter your user details and select your Desktop Environment

### LXDM (Included in LXDE)
```
pacman -S lxdm
systemctl enable lxdm
```

### SDDM (Included in KDE Plasma)
```
pacman -S sddm
systemctl enable sddm
```

### GDM (Included with GNOME)
```
pacman -S gdm
systemctl enable gdm
```

### LightDM
```
pacman -S lightdm lightdm-gtk-greeter
systemctl enable lightdm
```

## Graphics Driver

### Open Source drivers:
```
pacman -S xorg-drivers mesa lib32-mesa
```
### Nvidia proprietary driver:
```
pacman -S nvidia lib32-nvidia-utils
```
### AMD Utils:
```
pacman -S vulkan-radeon lib32-vulkan-radeon
```

## Networking
🖧 This are essential networking tools
```
pacman -S networkmanager networkmanager-openvpn networkmanager-pptp dnsmasq
systemctl enable NetworkManager
```
### If you use WiFi:
📶 This are essential tools if you connect to the internet via WiFi
```
pacman -S wireless_tools wpa_supplicant ifplugd dialog
systemctl enable net-auto-wireless
```

## Some archive and file system utils
🗄️ Important tools for archives and file systems
```
pacman -S p7zip unrar unarchiver unzip unace xz rsync
pacman -S nfs-utils cifs-utils ntfs-3g exfat-utils
```

## Sound
🔊 Some essential packages for sound
```
pacman -S alsa-utils pulseaudio-alsa pulseaudio-equalizer pulseaudio-jack
```
Control app for GTK Desktop:
```
pacman -S pavucontrol
```
Control app for Qt Desktop:
```
pacman -S pavucontrol-qt
```

🔇 PulseAudio fix notifications sounds muting some media players
```
nano /etc/pulse/default.pa
```
Comment out ```# load-module module-role-cork```

## Other shells

🐚 You may want to use another shell than bash

### zsh (Z Shell)
```
pacman -S zsh zsh-completions zsh-syntax-highlighting
chsh -s /usr/bin/zsh yourusername
```

### fish (Friendly interactive shell)
```
pacman -S fish
chsh -s /usr/bin/fish yourusername
```

# 11. Reboot
```
exit
umount -R /mnt
telinit 6
```

# 12. Post installation

## Set X11 Keymap
⌨️ It is recommended to set this to your keymap. Some Display Managers and Desktops use this
```
localectl set-x11-keymap de
```

## WiFi
📶 You may use the ```nmtui``` to configure your network profile

## Oh my zsh
🤖 A handy framework for managing your zsh configuration
```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## AUR Setup
The Arch User Repository is a community-driven repository for Arch users. Yay is a pacman wrapper that allows installing AUR packages
```
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
cd .. && rm -r yay
```

## If not all user dir's are present
📁 This will create your default folders (Downloads, Pictures, Documents, etc...)
```
yay -S xdg-user-dirs
xdg-user-dirs-update
```

## If you want a graphical package manager
📦 I recommend only to use ```yay``` to update and install packages but (especially if you are a beginner) you may want a graphical package manager
- Simple GTK: ```yay -S gnome-packagekit```
- Simple Qt: ```yay -S apper```
- Complex GTK: ```yay -S pamac-aur```
- Complex Qt: ```yay -S octopi```

## If you use a GTK desktop and want Qt apps to use your GTK Theme
🧮 This may not look good with every GTK Theme
```
yay -S qt5-styleplugins
echo "export QT_QPA_PLATFORMTHEME=gtk2" >> ~/.profile
```

## If you want to read APFS Partitions
💽 If you have a Hackintosh installation you can use this to access your files from it
```
yay -S linux-apfs-dkms-git
```

## Graphics card configuration tool

### AMD
```
yay -S radeon-profile-git radeon-profile-daemon-git
systemctl enable --now radeon-profile-daemon
```

### NVIDIA
```
yay -S nvidia-settings
```

## Fonts:

### General Fonts
🗛 This are some essential font packages
```
yay -S ttf-dejavu ttf-opensans font-mathematica noto-fonts-emoji freetype2 terminus-font ttf-bitstream-vera ttf-dejavu ttf-droid ttf-fira-mono ttf-fira-sans ttf-freefont ttf-inconsolata ttf-liberation ttf-linux-libertine powerline-fonts
```

### Nerd Fonts
🗚 Fonts patched with a high number glyphs (icons) included (You may want this if you use powerline)
```
yay -S nerd-fonts-complete
```

### Windows Fonts
🗛 If you need the Windows/Microsoft fonts (f.e. for Office Suites)
```
git clone https://aur.archlinux.org/ttf-ms-win10.git
cd ttf-ms-win10
READ PKGBUILD and copy all windows files into the directory and run makepkg -si
```

### macOS Fonts
🗚 If you want the San Francisco Font by Apple
```
yay -S otf-san-francisco-pro
```

## Nano syntax highlighting
📃 This will add syntax highlighting to the nano text editor
```
yay -S nano-syntax-highlighting
```

## Auto clean package cache
🗑️ This will clear the package cache to only keep 1 version after every action

Taken from <https://sourceforge.net/projects/ezos/files/ArchStuff/>
```
sudo mkdir /etc/pacman.d/hooks
sudo nano /etc/pacman.d/hooks/clean_package_cache.hook
```

```
[Trigger]
Operation = Upgrade
Operation = Install
Operation = Remove
Type = Package
Target = *
[Action]
Description = Cleaning pacman cache...
When = PostTransaction
Exec = /usr/bin/paccache -rk 1
```

# 13. Some fixes and tweaks

## Compability tweaks
🐛 This will fix some bugs and compability issues
```
sudo ln -sf /usr/lib/libncursesw.so.6 /usr/lib/libtinfo.so.5
```
### Spotify local files
```
yay -S ffmpeg-compat-57
```
```
yay -S ffmpeg
```

## Fix on shutdown "Failed to start user manager service for user 174" (sddm)
```
sudo chage --expiredate -1 sddm
```

## Force Google Emoji
```
sudo nano /etc/fonts/conf.d/50-user.conf
```
```xml
<fontconfig>
  <match target="pattern">
    <edit name="family" mode="prepend_first">
      <string>Icons</string>
    </edit>
  </match>
    
  <match target="pattern">
    <edit name="family" mode="prepend_first">
      <string>Noto Color Emoji</string>
    </edit>
  </match>
</fontconfig>
```

## Desktop icons for nemo
```
gsettings set org.nemo.desktop show-desktop-icons true
```