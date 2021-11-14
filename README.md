# TheStage4Library
An whole Library featuring many precompiled desktops from Gentoo Linux and other distros.

Please see TheStage4Library introduction Documentation for more detail and introduction.

Please report issues and make requests if you have any also my discord is Xinc#0116 contact me there for anything.

How to install the stage4's

Hello so i assume you downloaded the stage4 from the tags section i hope.

Well then open the terminal where the file is and then mount the ROOT only to /mnt/gentoo then run mv stage4** /mnt/gentoo

then cd /mnt/gentoo then you run tar -xJpvf file.tar.xz and then you do cd mnt/gentoo

then you run rm -r mnt && mv * /mnt/gentoo and then rm -r /mnt/gentoo/mnt/gentoo and then you chroot into the install by running the commands below.

mount --types proc /proc /mnt/gentoo/proc

mount --rbind /sys /mnt/gentoo/sys

mount --make-rslave /mnt/gentoo/sys

mount --rbind /dev /mnt/gentoo/dev

mount --make-rslave /mnt/gentoo/dev

test -L /dev/shm && rm /dev/shm && mkdir /dev/shm

mount --types tmpfs --options nosuid,nodev,noexec shm /dev/shm

chmod 1777 /dev/shm

chroot /mnt/gentoo /bin/bash

then while in chroot run source /etc/profile and then mount /dev/xxx /boot/efi and then nano /etc/fstab and change whatever you need then exit out and run grub-install && grub-mkconfig -o /boot/grub/grub.cfg and then nano /etc/portage/make.conf and change the mirrors optionally and unless your on amd then change the VIDEO_CARDS section also as mentioned in the README introduction and maybe change the use flags if you need to and the hostname in the introduction also and run passwd to change the password from stage4 to yours. Then your done although you may want to add an user

