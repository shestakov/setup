# Setting up a new workspace
## Installing Windows and Linux
Install Windows **before** Linux, as it uses an existing EFI partition, but we need to keep OSes fully separeted.
An alternative is to backup and then delete and restore an existing EFI partition.
Windows can also be safely reinstalled, given the original Winsows' EFI partition left intact.

## Installing Linux
1. Leave Secure Boot ON and TPM OFF
2. Partitions: 128 MB UEFI, 2GB-128MB /boot, 120 for w, 100 for s, the rest for the LUKSed system
3. `sudo adduser $USER vboxusers`
4. `sudo adduser $USER disk`
5. `sudo VBoxManage internalcommands createrawvmdk -filename "~/VMs/windows.vmdk" -rawdisk /dev/sda` (or `sudo VBoxManage internalcommands createrawvmdk -filename "~/VMs/windows.vmdk" -rawdisk /dev/nvme0n1 -partitions 1,2,3,4,5,6`, refer `gparted`)
6.  `sudo chown $USER:$USER ~/VMs/windows*`
7.  Actually create a VM (Win10, 4096MB RAM, 3D Acceleration and 256 MB VRAM, EFI, mark drive as an SSD)
8. `VBoxManage setextradata "Windows" "CustomVideoMode1" "1920x1080x24"`
9. `VBoxManage setextradata "Windows" "CustomVideoMode2" "2560x1440x24"`

## Setting up MSI GF63 drivers:
1. For Intel WiFi driver on Win10 `Win64\Installer\setup.xml` and `Win64\Installer\wirelesssetup.exe` must be moved to `Win64\Installer\extensions` and started there.
