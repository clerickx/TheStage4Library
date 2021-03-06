# TheStage4Library
Welcome to stage4 library an entire library featuring precompiled desktops from Gentoo Linux and other distro's



### Our To-Do List
Incase you were wondering what we are gonna add in our library, here is our official [trello](https://trello.com/b/nh4u8iIM/thestage4library-todo).

### Our Discord Server!
We have made our own server. If you would like to talk to people, here it [is](https://discord.gg/wBty8aMjRZ)!

### Known Issues:
```
. MAKEOPTS="-j8" instead of MAKEOPTS="-j$(nproc)"
. DM issue causing the DM not to appear (only in some stage4's)
. Tree not being .
```
### Creating your own stage4's

Simple mount the root partition only here i will do it at /mnt/gentoo and then run this 
cd to the /mnt/wherever and then do sudo tar -cJpvf /path/to/output/tarball.tar.xz *
and to extract do
sudo tar xpvf file.tar.xz --xattrs-include='*.*' --numeric-owner 

### FAQ

Welcome to TheStage4Library FAQ! Here we will be showing off the questions that you may ask and have answers for them. (Any questions requested will be added if they are good)
```
Question:
Why won't you add other unofficial init systems like runit?
Answer:
Unofficial init systems tend to be very hard to setup and the expierence is best is pretty terrible so we have no plans of adding anything apart of openRC and systemD.

Question:
Why don't you use testing packages globally?
Answer:
I don't set testing packages globally mainly due to me hating testing packages as of previous expierence although you may ask why do i have for some stage4's?
Well usually the case is to have the better expierence you will need to have testing packages globally to make it good for use.

Question:
Why do you use MAKEOPTS="-j$(nproc)" and the vulkan use flag in your /etc/portage/make.conf file?
Answer:
For the first one i use that to give the person an automatic max core detections instead of manually putting it in and for the second one i choose that as today stuff use vulkan quite a bit an good example like linux gaming although if you have any problems with the current use flags you can change them it is up to you.

Question: What kernel source does the stage4's use?
Answer:
It uses the precompiled gentoo-sources kernel to fit most users.
```
## Installation

**Note: every single step here must be done as root**

Mount root partition recommended /mnt/gentoo
```
cd /mnt/gentoo
```
### Download stage4
You must have a GUI live enviorment to download the stage4's or if possible you could use a terminal web browser that also works just it must have a copy link feature.

Anyways.

First open the link provided in your specific stage4 then right click  the download button and select copy link (depending on the web browser) then cd into /mnt/gentoo and wget URL and then your done.

### Unpack stage4
```
tar xpvf file.tar.xz --xattrs-include='*.*' --numeric-owner 
```

### Fixing Network Issue with Stage4's
```
cp --dereference /etc/resolv.conf /mnt/gentoo/etc
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
Note: This is just an example of fstab
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
This is an important step in installing stage4's any mistake might lead to problems
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
MAKEOPTS="-j$(nproc)"
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
The make.conf is all up to you although you might wanna change a few stuff like mirrors videocards and makeopts.

Youre done but you may want to add an user.

NOTE:
Sometimes in the /etc/portage/make.conf the makeopts will be MAKEOPTS="-j8" change the 8 to your number of cores or change the entire thing to MAKEOPTS="-j$(nproc)" to do it automatically sorry it's hard to remember all of this also please update @world if you change the VIDEO_CARDS in /etc/portage/make.conf
