> ALIH - Arch Linux Install Helper  
> By KlausDevWalker  
> Version: 1.0.0  
> Release Date: 2019-11-16

---

## Configuring and installing Arch Linux

1. Set keyboard layout (layout are usually found at `/usr/share/kbd/keymaps/i386/qwerty/$LAYOUT.map.gz`):

```shell
# loadkeys YOUR-LAYOUT
```

- Brazilian layout: `br-abnt2`

2. Connect with a wireless connection with wifi-menu obsfuscating password:

```shell
# wifi-menu -o
```

3. Ping an URL to test connection:

```shell
# ping -c 3 google.com
```

4. Turn on time syncronization with the internet, set timezone and check time (timezones can be fount with `list-timezones` command):

```shell
# timedatectl set-ntp true
# timedatectl set-timezone YOUR-TIMEZONE
# timedatectl status
```

- Brazil capital timezone: `America/Sao_Paulo`

5. Check, choose, format and mount root partition on /mnt (`X` being the number of your partition):

```shell
# lsblk
# mkfs.ext4 /dev/sdaX
# mount /dev/sdaX/ /mnt
```

6. (UEFI only) Create directories for mounting EFI partition:

```shell
# mkdir /mnt/boot
# mkdir /mnt/boot/efi
```

7. (Optional) Create directory for home partition (separated from root, if wanted) and mount it (check it with `lsblk`):

```shell
# mkdir /mnt/home
# mount /dev/sdaX /mnt/home
```

8. (UEFI only) Mount EFI partition:

```shell
# mount /dev/sdaX /mnt/boot/efi
```

9. Update database files and install reflector:

```shell
# pacman -Sy reflector
```

10. Run `reflector` to get the fastest, recently synchronized servers in the last hour from your country from MirrorStatus:

```shell
# reflector -n 10 -l 10 -a 1 -c YOUR-COUNTRY
```

11. Install Arch base packages with pacstrap:

```shell
# pacstrap /mnt base
```

12. Generate fstab with your near-to-be working partition and check it for errors:

```shell
# genfstab -U /mnt >> /mnt/etc/fstab
# cat /mnt/etc/fstab
```

13. Change root to /mnt:

```shell
# arch-chroot /mnt
```

14. Set time zone (the same as step 4):

```shell
# ln -sf /usr/share/zoneinfo/YOUR/TIMEZONE /etc/localtime
```

- Brazil capital timezone: `/zoneinfo/America/Sao_Paulo`

15. Set the hardware clock to the system time:

```shell
# hwclock --systohc
```

16. Open `/etc/locale.gen` and uncomment needed locales `eu_US.UTF-8 UTF-8 and/or (and other needed locales), save and quit, then generate them

```shell
# nano /etc/locale.gen
# locale-gen
```

- Brazilian user: `pt_BR.UTF-8 UTF-8`
- American user: `eu_US.UTF-8 UTF-8`

17. Create hostname file and put a chosen hostname in it:

```shell
# echo "YOUR-HOSTNAME" >> /etc/hostname
```

18. Open `/etc/hosts` and insert the 3 lines below (without quotes), then save and quit:

```
# nano /etc/hosts
```

```shell
#127.0.0.1 localhost
#::1 localhost
#127.0.1.1 YOUR-HOSTNAME.localdomain YOUR-HOSTNAME
```

`Control+S, Control+X`

19. Recreate the initramfs image:

```shell
# mkinitcpio -p linux
```

20. Set root password:

```shell
# passwd
```

1.  Install `grub`, `efibootmgr`, `sudo`, `wpa_supplicant`, `ifplugd` and `dialog` (`tree`, `git`, `mc`, `tcl` and `tk` are not essential):

```shell
# pacman -S grub efibootmgr sudo wpa_supplicant ifplugd dialog tree git mc tcl tk
```

22. Install microcode for AMD **OR** Intel processor:

```shell
# pacman -S amd-ucode
```

OR

```shell
# pacman -S intel-ucode
```

23. (UEFI only) Install `grub` on EFI partition (UEFI boot):

```shell
# grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=YOUR-GRUB-ENTRY-LABEL
```

24. (Legacy Boot only) Install `grub` on the same device as the root partition (`X` being the letter of your device):

```shell
# grub-install /dev/sdX
```

25. Create `grub` directory (if it wasn't created automatically) and generate `grub.cfg`:

```shell
# mkdir /boot/grub && grub-mkconfig -o /boot/grub/grub.cfg
```

26. Add an user and a password for the create account:

```shell
# useradd -m YOUR-USERNAME
# passwd YOUR-PASSWORD
```

27. Add that user to some important groups:

```shell
# usermod -aG adm,wheel,sys,users,lp,network,power YOUR-USERNAME
```

28. (Optional) Change default CLI text editor to nano:

```shell
# export EDITOR="nano"
```

29. Enter `visudo` command and uncomment the line below to give sudo acess to created user and save it:

```shell
#%wheel ALL=(ALL) ALL
```

`Control+S`, `Control+X`

30. Exit chroot environment:

```shell
# exit
```

31. Unmount all mounted devices recursively:

```shell
# umount -R /mnt
```

32. Reboot to finish installation:

```
# reboot
```

---

## After first reboot

33. Enter user login credentials at tty:

```shell
YOUR-HOSTNAME login: YOUR-USERNAME
Password: YOUR-PASSWORD
```

34. Make keyboard layout persistent (if the choice was any but US):

```shell
# localectl set-keymap --no-convert YOUR-LAYOUT
```

- Brazilian layout: `br-abnt2`

---

### TO BE CONTINUED...
