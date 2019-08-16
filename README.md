<pre>
Step 1: Set static IP in your server PC.
Step 2:Install DNSMASQ Server
  sudo apt update
  sudo apt install dnsmasq
Step 3:Configure DNSMASQ server
  sudo nano /etc/dnsmasq.conf
Step 4:Make directory for tftp server
  sudo mkdir -p /netboot/tftp
Step 4:Restart and check status of DNSMASQ
  sudo systemctl restart dnsmasq
  systemctl status dnsmasq.service
Step 5:Install NFS server
  sudo apt install -y nfs-kernel-server
Step 6:Create a new directory  /netboot/nfs to mount  NFS and configure it:
  sudo mkdir /netboot/nfs
  
  open exports:
  sudo nano /etc/exports
  
  add following line:
  
  /netboot/nfs  *(ro,sync,no_wdelay,insecure_locks,no_root_squash,insecure,no_subtree_check)
  
  sudo exportfs -a
Step 7: Install PXE boot files

  sudo apt install -y syslinux pxelinux
  
  Copy the pxelinux.0 file to the /netboot/tftp:
  
  sudo cp -v /usr/lib/PXELINUX/pxelinux.0 /netboot/tftp/
  
  Copy ldlinux.c32, libcom32.c32, libutil.c32, vesamenu.c32 files to the /netboot/tftp
  
  sudo cp -v  /usr/lib/syslinux/modules/bios/{ldlinux.c32,libcom32.c32,libutil.c32,vesamenu.c32} /netboot/tftp
  
  Create PXE bootloader configuration directory /netboot/tftp/pxelinux.cfg/ 
  
  sudo mkdir /netboot/tftp/pxelinux.cfg
  
  Create PXE bootloader’s default configuration file /netboot/tftp/pxelinux.cfg/default
  
  sudo touch /netboot/tftp/pxelinux.cfg/default

Step 8:Download the Ubuntu 18.04 LTS Live Desktop ISO
  wget http://releases.ubuntu.com/18.04/ubuntu-18.04.2-desktop-amd64.iso
Step 9:Mount the ISO file in /mnt
  sudo mount -o loop ubuntu-18.04.2-desktop-amd64.iso /mnt
Step 10:Create directories for Ubuntu 18.04 LTS /netboot/nfs/ubuntu1804/ and /netboot/tftp/ubuntu1804/
  sudo mkdir -v /netboot/{nfs,tftp}/ubuntu1804
Step 11:Copy the vmlinuz and initrd files to the /netboot/tftp/ubuntu1804/
  
Step 12:Copy the contents of the ISO file to the NFS directory /netboot/nfs/ubuntu1804/ 
  sudo cp -Rfv /mnt/* /netboot/nfs/ubuntu1804/
Step 13:Copy the vmlinuz and initrd files to the /netboot/tftp/ubuntu1804/
  sudo cp -v /netboot/nfs/ubuntu1804/casper/{vmlinuz,initrd} /netboot/tftp/ubuntu1804/
Step 14:Set permission of the /netboot directory
  sudo chmod -Rfv 7777 /netboot
Step 15:Add PXE Boot Entry for Ubuntu 18.04 LTS
  sudo nano /netboot/tftp/pxelinux.cfg/default
Step 16:Unmount ISO
  sudo umount /mnt
Step 17:Now you can netboot your client server
Step 18:Some times you may not able to access your ethernet
        It shows as unmanaged network. To fix this:
        Open /etc/NetworkManager/NetworkManager.conf
        sudo nano /etc/NetworkManager/NetworkManager.conf
        update managed=false to managed=true
        Restart Network Manager
        sudo systemctl restart NetworkManager

  
  
 </pre>
# setting-up-pxe-server
