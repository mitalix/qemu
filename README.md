# qemu
qemu documentation

Working config:

Qemu with basic network bridge and dnsmasq over ipxe


sudo ip link add name br0 type bridge    
sudo ip addr add 172.20.0.1/16 dev br0    
sudo ip link set br0 up    


systemctl status dnsmasq  
sudo systemctl start dnsmasq  
journalctl -fu dnsmasq.service  


$ grep -v ^# /etc/dnsmasq.conf | strings  
interface=br0  
bind-interfaces  
dhcp-range=172.20.0.2,172.20.0.20  
enable-tftp  
tftp-root=/srv/tftp  
log-dhcp  
pxe-service=x86PC,"PXELINUX (BIOS)",boot/pxelinux  
tftp-secure  


$ qemu-system-x86_64 -nic bridge,br=br0,model=virtio-net-pci -m 1G

in another window:  <br>
$ vncviewer :5900


$ tree /srv/tftp/  
/srv/tftp/  
├── boot  
    ├── initramfs-linux.img  
    ├── pxelinux.0  
    ├── pxelinux.cfg  
    │   └── default  
    └── vmlinuz-linux  

$ cat /srv/tftp/boot/pxelinux.cfg/default   
DEFAULT linux  
        SAY Now booting the kernel from SYSLINUX ...  
LABEL linux  
        KERNEL vmlinuz-linux  
        initrd initramfs-linux.img  
        SYSAPPEND 3  


