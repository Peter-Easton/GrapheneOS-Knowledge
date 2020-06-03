# Setting up your host
Before reading this section I recommend you check out "Your GrapheneOS Shopping List" to see what you're going to need to buy.

This section's only going to cover what you'd need to do to start building. Learning to build the operating system is a fundamental (non-optional) step towards becoming a maintainer, and that's huge. 

## Installing Arch
There's only one architecture, and two recommended Linux distributions for building GrapheneOS: Debian 10 Buster, and Arch, both on the x86_64 processor. Building on MacOS and Windows currently isn't supported. The GrapheneOS project recommends Arch for its practice of using as few modifications to software provided to it from its upstream providers and remaining current with upstream. This causes fewer headaches in the long run. 

Installing Arch requires some working knowledge of the commandline to first enter a virtual Live environment with a command shell, set up the disks, networking, download the packages via `pacstrap` and choose items to install. This requires some familiarity with the commandline. The process is extensively documented on the Arch Wiki, and so won't be repeated here, with a few additional hints.

During your `arch-chroot` session while operating in the LiveCD installer, it's important to add a few extra packages.  tutorial assumes that you are either running your build station as a headless build server with no graphical interface and simply access it over a remote shell once it's been set up and provisioned. While you are booted into the `arch-chroot` consider adding the following packages and enabling them via `systemctl` so they initialize at boot.
```
pacman -Syy openssh dhcpcd
systemctl enable sshd
systemctl enable dhcpcd
```
Proceed with the rest of the installation as normal.

**Attention!** *Your computer will inevitably at one point or another need to handle signing keys, which must be handled by the host. As Solid State Drives can retain their data for very long periods of time and conventional means of erasing HDDs are ineffective on SSDs, you should install with full disk encryption and encrypted swap or no swap at all as a last-line of defense against if the drive is lost, or stolen.*

### Adding Multilib
You'll first want to enable multilib for Arch, which is contained in `/etc/pacman.conf` Scroll to near the bottom, where you'll find the following lines that have been commented out. You should likely use a text editor like `nano` or `vi` to do this. 
```
#[multilib]
#Include = /etc/pacman.d/mirrorlist
```
Simply uncomment these two lines to look like this: 
```
[multilib]
Include = /etc/pacman.d/mirrorlist
```
This should enable multilib for your package repositories. 

### Adding the dependencies
It should be noted that you'll be using Yay to install other packages, you'll need to be sure you uncommented multilib first, or some of the packages won't be found.

#### First Step
Install some of the rest of the dependencies
```
sudo pacman -S --needed autoconf automake bc binutils bison ccache dhcpcd fakeroot flex gcc git gperf groff jdk-openjdk jre-openjdk jre-openjdk-headless lib32-gcc-libs lib32-ncurses5-compat-libs libtool libxslt m4 make nano ninja ncurses5-compat-libs net-tools nsjail openssh patch perl-switch pkgconf python2-protobuf python2-virtualenv repo rsync schedtool sdl squashfs-tools sudo texinfo unzip wxgtk2 zip
```

If you are running with a graphical interface, you may require `lib32-mesa`. Since this tutorial assumes you are building on a detached, headless build server, this likely won't be necessary.

#### Using yay, or Yet Another Yogurt.
You'll need to use yay to install some of the other dependencies which are maintained by the Arch community. First, you'll need to obtain it, and make a package out of it using the following commands. You should likely do this as a regular user. It isn't recommended to run `yay` as root but instead to use sudo to give it privilege when it's needed. If you haven't already, you should switch to a regular user using adduser, set a password for it, and add the new user account to the group `wheel` with `usermod -a -G wheel [my-username]`. You may need to uncomment the line to allow the usergroup `wheel` to run processes as root using `visudo`.

First, we'll set up yay. Yay is not a core part of the arch repositories, but is maintained by the Arch User Repositories, and is installed by cloning it using git and the `makepackage` tool. The command sequence is: 
```
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```
Once that's done, use the following command to fetch the rest of the dependencies. Again, ensure multilib has already been uncommented, or it will fail. 
```
yay -S --needed base-devel multilib-devel gcc repo gperf sdl wxgtk2 squashfs-tools curl ncurses zlib schedtool perl-switch zip unzip libxslt bc rsync ccache lib32-zlib lib32-ncurses lib32-readline ncurses5-compat-libs lib32-ncurses5-compat-libs ttf-ms-fonts
```

#### Installing the fonts.
Android requires some fonts that aren't normally in the repositories. If yay can't install them via yay, that's no problem, just simply proceed to install them with: 
```
git clone https://aur.archlinux.org/ttf-ms-fonts.git
cd ttf-ms-fonts
makepkg -si
```

#### Installing Google's Depot tools.

Google provide tools written in python to allow for easier management of the projects. To fetch the depot tools, which include the `repo` and `gclient` tool, you'll need to get them from Google and add them to your command path. The easiest way to do that is simply by using the following command structure:
```
mkdir ~/Applications/
cd ~/Applications/
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git && cd depot_tools
export PATH=$PATH:$(pwd)
echo "export PATH="$(echo PATH) >> ~/.bashrc
```

### Installing Android Tools
Arch is one of the very few distributions that correctly numbers, versions, and packages up-to-date versions of the Android Debug Tools and Platform Tools. You can install them and the udev rules directly using:
```
pacman -S --needed android-tools android-udev
```
You may need to restart your computer after doing so. Note that you may need to whitelist your user account in `adbusers` if you want to use `adb` remotely over ssh.

Congratulations. You're ready to begin building GrapheneOS on your headless build server. 
