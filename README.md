I'm not the author of this driver. 
This is a modified version for Fedora.

```
sudo dnf install dkms kernel-devel kernel-headers
sudo dnf groupinstall "Development Tools" "Development Libraries"

git clone https://github.com/Swoogan/aziokbd.git
cd aziokbd
sudo ./install.sh dkms
```

Modify the generic '/etc/default/grub' configuration file:
```
GRUB_CMDLINE_LINUX_DEFAULT='usbhid.quirks=0x0c45:0x7603:0x4'  # if you find that 0x4 doesn't work, try 0x7
```
```
sudo grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg   # for UEFI systems
sudo grub2-mkconfig -o /boot/grub2/grub.cfg            # for BIOS systems
```

Create a file '/etc/dracut.conf.d/myflags.conf':
```
add_drivers+=" aziokbd "
```
```
sudo depmod -a 
sudo dracut --force
sudo lsinitrd /boot/initramfs-6.1.9-200.fc37.x86_64.img | grep -i aziokbd   # check
```
