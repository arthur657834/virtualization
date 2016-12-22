virsh list --all

virsh start vm1 

virsh shutdown vm1  

virsh reboot vm1  

virsh destroy vm1  

virsh autostart vm1 

virsh autostart --disable vm1  

virsh suspend vm1  

virsh resume vm1  

virsh edit vm1  

#修改了虚拟机XML配置文件以后要求声明XML配置文件
virsh define /etc/libvirt/qemu/vm1.xml  

#声明XML配置文件，并启动虚拟机
virsh create /etc/libvirt/qemu/vm1.xml  
#取消声明的虚拟机XML配置文件 
virsh undefine vm1  

#删除虚拟机硬盘
rm -rf /var/lib/libvirt/images/vm1.img  

virsh dumpxml vm1 > /etc/libvirt/qemu/vm1_dump.xml 

#这个要另外配置
virsh console vm1

#显示虚拟机信息
virsh dominfo vm1  

#查看磁盘信息
qemu-img info /var/lib/libvirt/images/vm1.img