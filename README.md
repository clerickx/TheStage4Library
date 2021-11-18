# TheStage4Library
An whole Library featuring many precompiled desktops from Gentoo Linux and other distros.

Please see TheStage4Library introduction Documentation for more detail and introduction.

Please report issues and make requests if you have any also my discord is Xinc#0116 contact me there for anything.

## How to install the stage4's

Hello so i assume you downloaded the stage4 from the tags section i hope.

Well then open the terminal where the file is and then mount the ROOT only to /mnt/gentoo then run mv stage4** /mnt/gentoo

If you want the file to be saved in /mnt/gentoo exactly than please run the browser as root and set it to ask you where to save.

## Installation

Mount root partition recommended /mnt/gentoo
```
cd /mnt/gentoo
```


###### **Download stage4**
Hello so i do recommend using this piece of software
https://github.com/tonikelope/megabasterd
and just give it an direct link and the path simple

###### **Unpack stage4**
```
tar -xJpvf file.tar.xz
```

###### **Mounting the necessary filesystems**
```
mount --types proc /proc /mnt/gentoo/proc
mount --rbind /sys /mnt/gentoo/sys
mount --make-rslave /mnt/gentoo/sys
mount --rbind /dev /mnt/gentoo/dev
mount --make-rslave /mnt/gentoo/dev
```

###### **Note: only when using non-gentoo media**
which you probably will not be using. as far as i know the one mega doesn't allow direct link downloads
```
test -L /dev/shm && rm /dev/shm && mkdir /dev/shm
mount --types tmpfs --options nosuid,nodev,noexec shm /dev/shm
chmod 1777 /dev/shm
```

###### **Entering the new environment**
```
chroot /mnt/gentoo /bin/bash
source /etc/profile
export PS1="(chroot) ${PS1}"
```

then while in chroot run source /etc/profile and then mount /dev/xxx /boot/efi and then nano /etc/fstab and change whatever you need then exit out and run grub-install && grub-mkconfig -o /boot/grub/grub.cfg and then nano /etc/portage/make.conf and change the mirrors optionally and unless your on amd then change the VIDEO_CARDS section also as mentioned in the README introduction and maybe change the use flags if you need to and the hostname in the introduction also and run passwd to change the password from stage4 to yours. Then your done although you may want to add an user.

NOTE:
Sometimes in the /etc/portage/make.conf the makeopts will be MAKEOPTS="-j8" change the 8 to your number of cores or change the entire thing to MAKEOPTS="-j$(nproc)" to do it automatically sorry it hard to remember all of this.

