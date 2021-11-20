# TheStage4Library
An whole Library featuring many precompiled desktops from Gentoo Linux and other distros.

Please see TheStage4Library introduction Documentation for more detail and introduction.

Please report issues and make requests if you have any also my discord is Xinc#0116 contact me there for anything.

## How to install the stage4's

Hello so i assume you downloaded the stage4 from the tags section i hope.

Well then open the terminal where the file is and then mount the ROOT only to /mnt/gentoo.

## Installation

Mount root partition recommended /mnt/gentoo
```
cd /mnt/gentoo
```


### Download stage4
Hello so i do recommend using this piece of software
https://github.com/tonikelope/megabasterd
and just give it an direct link and the path simple

Step 1. install mega basterd extract it and run it with sudo ./Megabasterd.run as it must be as root than click on new download and than put in the link of the releases than select the path which is /mnt/gentoo and than lets dance aka downloading it after it is done you can exit.

### Unpack stage4
```
tar -xJpvf file.tar.xz 

## Fixing tree being to /mnt/gentoo or /tmp/gentoo instead of .

Go into the tmp or mnt folder depending on the stage4 than mv gentoo /mnt/gentoo than rm -r tmp or gentoo than rm Gentoo*.tar.xz and than you can continue
```

### Mounting the necessary filesystems
```
mount --types proc /proc /mnt/gentoo/proc
mount --rbind /sys /mnt/gentoo/sys
mount --make-rslave /mnt/gentoo/sys
mount --rbind /dev /mnt/gentoo/dev
mount --make-rslave /mnt/gentoo/dev
```

### Note: only when using non-gentoo media
which you probably will not be using. as far as i know the one mega doesn't allow direct link downloads
```
test -L /dev/shm && rm /dev/shm && mkdir /dev/shm
mount --types tmpfs --options nosuid,nodev,noexec shm /dev/shm
chmod 1777 /dev/shm
```

### Entering the new environment
```
chroot /mnt/gentoo /bin/bash
source /etc/profile
export PS1="(chroot) ${PS1}"
```

While in chroot mount your efi partition to /boot/efi
```
mount /dev/<efipart> /boot/efi
```

### Fstab
Note: this is just a example fstab
```
# /etc/fstab: static file system information.
#
# noatime turns off atimes for increased performance (atimes normally aren't 
# needed); notail increases performance of ReiserFS (at the expense of storage 
# efficiency).  It's safe to drop the noatime options if you want and to 
# switch between notail / tail freely.
#
# The root filesystem should have a pass number of either 0 or 1.
# All other filesystems should have a pass number of 0 or greater than 1.
#
# See the manpage fstab(5) for more information.
#

# <fs>			<mountpoint>	<type>		<opts>		<dump/pass>

# NOTE: If your BOOT partition is ReiserFS, add the notail option to opts.
#
# NOTE: Even though we list ext4 as the type here, it will work with ext2/ext3
#       filesystems.  This just tells the kernel to use the ext4 driver.
#
# NOTE: You can use full paths to devices like /dev/sda3, but it is often
#       more reliable to use filesystem labels or UUIDs. See your filesystem
#       documentation for details on setting a label. To obtain the UUID, use
#       the blkid(8) command.

UUID=<uuid of efi> /boot/efi vfat umask=0077 0 2
UUID=<uuid of root>    /    <root partition type>    noatime    0 1
```

### grub
```
grub-install
grub-mkconfig -o /boot/grub/grub.cfg
```

### Make.conf
This is a important step in installing stage4's any mistake might lead to problems
```
(chroot) root# nano /etc/portage/make.conf
# These settings were set by the catalyst build script that automatically
# built this stage.
# Please consult /usr/share/portage/config/make.conf.example for a more
# detailed example.
COMMON_FLAGS="-O2 -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
USE="vulkan"
MAKEOPTS="-j8"
VIDEO_CARDS="amdgpu radeon radeonsi"
ABI_X86="64 32"
ACCEPT_LICENSE="*"
GENTOO_MIRRORS="https://mirror.bytemark.co.uk/gentoo/"


# NOTE: This stage was built with the bindist Use flag enabled
PORTDIR="/var/db/repos/gentoo"
DISTDIR="/var/cache/distfiles"
PKGDIR="/var/cache/binpkgs"

# This sets the language of build output to English.
# Please keep this setting intact when reporting bugs.
LC_MESSAGES=C
GRUB_PLATFORMS="efi-64"
```
The make.conf is all up to you you might wanna change a few stuff like mirrors videocards and makeopts

done although you may want to add an user.

NOTE:
Sometimes in the /etc/portage/make.conf the makeopts will be MAKEOPTS="-j8" change the 8 to your number of cores or change the entire thing to MAKEOPTS="-j$(nproc)" to do it automatically sorry it hard to remember all of this.

