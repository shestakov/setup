# Setting up a new workspace
## Installing Windows and Linux
Install Windows **before** Linux, as it uses an existing EFI partition, but we need to keep OSes fully separeted.
An alternative is to backup and then delete and restore an existing EFI partition.
Windows can also be safely reinstalled, given the original Windows' EFI partition left intact.

## Installing Linux
1. Leave Secure Boot ON and TPM OFF
2. Partitions: 128 MB EFI, 2GB-128MB /boot, 120 for w, 100 for s, the rest for the LUKSed system
3. `sudo adduser $USER vboxusers && sudo adduser $USER disk`
4. `sudo VBoxManage internalcommands createrawvmdk -filename ~/VMs/windows.vmdk -rawdisk /dev/sda` (or `sudo VBoxManage internalcommands createrawvmdk -filename "~/VMs/windows.vmdk" -rawdisk /dev/nvme0n1 -partitions 1,2,3,4,5,6`, refer `gparted`)
5. `sudo chown $USER:$USER ~/VMs/windows*`
6.  Actually create a VM (Win10, 4096MB RAM, 3D Acceleration and 256 MB VRAM, EFI, mark drive as an SSD)
7. `VBoxManage setextradata "Windows" "CustomVideoMode1" "1920x1080x24"`
8. `VBoxManage setextradata "Windows" "CustomVideoMode2" "2560x1440x24"`

## Installing and signing NVIDIA drirvers
1. Secure Boot should be enabled;
2. Check if the MOK is enrolled: `mokutil --list-enrolled`
3. Import the certificate to enroll:`sudo mokutil --import /var/lib/shim-signed/mok/MOK.der`
4. `reboot` and enroll the certificate, the password has been set during the Mint 22.1 installation
5. Sign the driver: `sudo /usr/src/linux-headers-$(uname -r)/scripts/sign-file sha256 /var/lib/shim-signed/mok/MOK.priv /var/lib/shim-signed/mok/MOK.der $(modinfo -n nvidia)`
6. `reboot` again
7. Finall, `sudo modprobe nvidia`
8. Test: `nvidia-smi`

## Setting up MSI GF63 drivers on Windows:
1. For Intel WiFi driver on Win10 `Win64\Installer\setup.xml` and `Win64\Installer\wirelesssetup.exe` must be moved to `Win64\Installer\extensions` and started there.
2. Call to enable BitLoker without TPM, if needed. Go to `Run gpedit.msc > Computer Configuration > Administrative Templates > Windows Components > BitLocker Drive Encryption > OS Drives > Require additional authentication at startup`, enable the polycy, ensure TPM is not required

## Setting up Intel Wifi on Linux
1. Must switch to the Ubuntu's Hardware Enablement kernel as of Mint 22.1 in June 2025

## Software to setup on Linux
1. Double Commander (`sudo apt install doublecmd-gtk`)
2. ffmpeg (`sudo apt install ffmpeg`)
3. Docker from PPA, not default repo: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04
4. Ansible, better with modern Python
5. PostgreSQL (https://www.postgresql.org/download/linux/ubuntu/)
6. `vscode` (https://code.visualstudio.com/Download)
7. dotnet:
   * `sudo apt-get update && sudo apt-get install -y dotnet-sdk-8.0`
   * `echo 'DOTNET_CLI_TELEMETRY_OPTOUT="true"' | sudo tee -a /etc/environment && source /etc/environment && echo "$DOTNET_CLI_TELEMETRY_OPTOUT"`
9. Rider (https://www.jetbrains.com/help/rider/Installation_guide.html#standalone_linux)
10. Node Version Manager (https://raw.githubusercontent.com/nvm-sh/nvm/refs/tags/v0.40.3/install.sh)
11. Xray
  * Install: https://github.com/XTLS/Xray-install
  * Put `config.json` to `/usr/local/etc/xray`
  * Enable: `sudo systemctl enable xray`
  * Refer to: https://xtls.github.io/en/document/config.html
13. Wireguard
