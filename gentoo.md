```
mkfs.vfat -F 32 /dev/nvme0n1p1
```

```mkfs.ext4 /dev/nvme0n1p3```
```mkswap /dev/nvme0n1p2```

```mkdir --parents /mnt/gentoo```
```mount /dev/nvme0n1p3 /mnt/gentoo```
```date```
```cd /mnt/gentoo```
```wget```
```tar xpvf stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner```
```nano /mnt/gentoo/etc/portage/make.conf```

```
# These settings were set by the catalyst build script that automatically
# built this stage.
# Please consult /usr/share/portage/config/make.conf.example for a more
# detailed example.
COMMON_FLAGS="-march=native -mtune=generic -O2 -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
MAKEOPTS="-j17 -l16"

USE="wayland pipewire pulseaudio gtk+ gtk qt5 qt6 dbus unicode"
VIDEO_CARDS="amdgpu radeonsi radeon"
ACCEPT_LICENSE="*"

# NOTE: This stage was built with the bindist Use flag enabled

# This sets the language of build output to English.
# Please keep this setting intact when reporting bugs.
LC_MESSAGES=C.utf8

GENTOO_MIRRORS="https://ftp.uni-stuttgart.de/gentoo-distfiles/ \
    https://gentoo-mirror.alexxy.name/ \
    http://gentoo-mirror.alexxy.name/ \
    http://mirror.mephi.ru/gentoo-distfiles/ \
    ftp://mirror.mephi.ru/gentoo-distfiles/ \
    rsync://mirror.mephi.ru/gentoo-distfiles/ \
    https://mirror.yandex.ru/gentoo-distfiles/ \
    http://mirror.yandex.ru/gentoo-distfiles/ \
    ftp://mirror.yandex.ru/gentoo-distfiles/"
GRUB_PLATFORMS="efi-64"```

```mirrorselect -i -o >> /mnt/gentoo/etc/portage/make.conf```
```mkdir --parents /mnt/gentoo/etc/portage/repos.conf```
```cp /mnt/gentoo/usr/share/portage/config/repos.conf /mnt/gentoo/etc/portage/repos.conf/gentoo.conf```
```cp --dereference /etc/resolv.conf /mnt/gentoo/etc/```

```mount --types proc /proc /mnt/gentoo/proc \
  --rbind /sys /mnt/gentoo/sys \
  --make-rslave /mnt/gentoo/sys \
  --rbind /dev /mnt/gentoo/dev \
  --make-rslave /mnt/gentoo/dev \
  --bind /run /mnt/gentoo/run \
  --make-slave /mnt/gentoo/run
```
```chroot /mnt/gentoo /bin/bash -c "source /etc/profile; export PS1=\"(chroot) \$PS1\""```

```mkdir /efi```
```mount /dev/nvme0n1p1 /efi```
```emerge-webrsync```
```emerge --sync```
```eselect news read```


```eselect profile list```

```eselect profile set```

```emerge --ask --verbose --update --deep --newuse @world```

```emerge --ask app-portage/cpuid2cpuflags```
```echo "*/* $(cpuid2cpuflags)" > /etc/portage/package.use/00cpu-flags```

```ln -sf ../usr/share/zoneinfo/Europe/Moscow /etc/localtime```
```nvim /etc/locale.gen```
```locale-gen```
```eselect locale list```
```eselect locale set```
```env-update && source /etc/profile && export PS1="(chroot) ${PS1}"```
```emerge --ask -q sys-kernel/linux-firmware```
```emerge --ask -q sys-firmware/intel-microcode```
```/etc/portage/package.use/installkernel```
```sys-kernel/installkernel dracut```
```emerge --ask sys-kernel/gentoo-kernel-bin```
```emerge --depclean```
```emerge --prune sys-kernel/gentoo-kernel sys-kernel/gentoo-kernel-bin```
```nvim /etc/fstab```
```
# /dev/nvme0n1p3
UUID=a2b1ef05-9703-40e5-9bb5-f6068a8c1867   /       ext4    defaults,noatime     0 1

# /dev/nvme0n1p1
UUID=F241-CB23                              /boot   vfat    defaults            0 2

# /dev/nvme0n1p2
UUID=d25a4f9d-057b-4a15-9c45-0dbe80a90260   none    swap    defaults            0 0
```

```/etc/hostname```
```emerge --ask net-misc/networkmanager```

```passwd```
```systemd-machine-id-setup```
```systemd-firstboot --prompt```
```ystemctl preset-all --preset-mode=enable-only```
```systemctl preset-all```
```systemctl enable NetworkManager```

```emerge --ask -q sys-apps/mlocate```
```systemctl enable sshd```
```emerge --ask -q app-shells/bash-completion```

```emerge --ask -q sys-fs/e2fsprogs sys-fs/dosfstools```
```emerge --ask -q sys-block/io-scheduler-udev-rules```

```echo 'GRUB_PLATFORMS="efi-64"' >> /etc/portage/make.conf```
```emerge --ask -q sys-boot/grub sys-boot/os-prober app-admin/sudo```

```sudo grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=gentoo --removable --recheck```
```grub-mkconfig -o /boot/grub/grub.cfg```

```
useradd -m -G users,wheel,audio,usb,portage -s /bin/bash ilia
passwd ilia
EDITOR=nvim visudo
```

```
rm /stage3-*.tar.*
```

```timedatectl```
```sudo timedatectl set-timezone Europe/Moscow```
```sudo timedatectl set-ntp true```
```timedatectl```
```sudo systemctl restart systemd-timesyncd```

```
127.0.0.1   localhost
::1         localhost
127.0.1.1   oivoss.localdomain oivoss
```

```
emerge --ask -q app-portage/eix
emerge --ask -q app-portage/gentoolkit
```

```
exit
cd
umount -l /mnt/gentoo/dev{/shm,/pts,}
umount -R /mnt/gentoo
reboot
```


```
emerge --sync
emerge-webrsync
emerge --deselect PACKAGE_NAME
emerge --update --deep --newuse -q @world
emerge --depclean
eclean distfiles
eclean packages
```





