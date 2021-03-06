监测是否支持虚拟化:
egrep '(vmx|svm)' /proc/cpuinfo  
vmx 为Intel的CPU指令集
svm 为AMD的CPU指令集

```shell
yum -y install qemu-kvm qemu-img virt-manager libvirt libvirt-python libvirt-client virt-install virt-viewer bridge-utils
```

qemu-kvm：该软件包主要包含KVM内核模块和适用于KVM的QEMU模拟器。KVM模块负责CPU和内存的调度，QEMU负责虚拟机I/O设备的模拟。<br>
qemu-img：qemu磁盘image管理器<br>
virt-install：用来创建虚拟机的命令行工具<br>
libvirt：提供libvirtd daemon来管理虚拟机和控制hypervisor<br>
libvirt-client：提供客户端API用来访问server和提供管理虚拟机命令行工具的virsh实体<br>
virt-viewer：图形控制台<br>
libvirt-daemon:libvirtd守护进程，作为客户端管理工具跟Hypervisor和虚拟机之间的桥梁。<br>
libvirt-daemon-driver-xxx:从名字来看属于libvirtd服务的驱动文件，作为libvirtd服务跟Hypervisor不同对象(如qemu模拟器，网络，存储等)间的接口。<br>
virt-manager:图形界面的KVM管理工具。<br>

```shell
systemctl enable libvirtd.service
systemctl start libvirtd.service
systemctl list-unit-files | grep libvirtd
```

检查KVM是否加载成功
lsmod | grep kvm
检查KVM是否成功安装

```
echo 1 > /proc/sys/net/ipv4/ip_forward 

grep -v ^# ifcfg-enp2s0 
TYPE=Ethernet
BOOTPROTO=none
NAME=enp2s0
UUID=5701dc63-96dc-42cd-8436-a042af4d2dc9
DEVICE=enp2s0
ONBOOT=yes
BRIDGE=br0
NM_CONTROLLED=no
 
cat ifcfg-bro 
TYPE=Bridge  
BOOTPROTO=none  
DEVICE=br0  
ONBOOT=yes  
IPADDR1=10.1.50.250 
NETMASK1=255.255.255.0  
GATEWAY1=10.1.50.1  
DNS1=10.1.2.252
NM_CONTROLLED=no

systemctl restart NetworkManager
systemctl restart network

NM_CONTROLLED参数表示该网卡是否被NetworkManager服务管理，设置为no的话就是不接管，那么之前不用停止NetworkManager服务。

brctl show             

virt-install  --name=ljtest  --ram=1024  --vcpus=1  --cdrom=/root/CentOS-7-x86_64-Minimal-1511.iso --os-type=linux --os-variant=centos7.0  --network bridge=br0 --graphics=spice  --disk path=/var/lib/libvirt/images/ljtest.dsk,size=4  

#osinfo-query os 来查询--os-variant支持的操作系统类型

-name：虚拟机的名字
-ram：内存大小MB
-vcpus：CPU个数
-cdrom：ISO镜像位置
-os-variant：OS类型，例如rhel 6，solaris
-network：网络，友情链接：创建通过Virt Manager创建桥接网络
-graphics：Guest显示设置
-disk path：磁盘位置

virt-manager

virsh list --all

kvm --version
virt-install --version
virsh --version

echo vnc_listen = "0.0.0.0" >> /etc/libvirt/qemu.conf

qemu-img create -f qcow2 /tmp/centos-7.2.qcow2 10G
virt-install --virt-type kvm --name centos-7.2 --ram 1024 \
--disk /tmp/centos-7.2.qcow2,format=qcow2 \
--network network=default \
--graphics vnc,listen=0.0.0.0 --noautoconsole \
--os-type=linux --os-variant=rhel6 \
--extra-args="console=tty0 console=ttyS0,115200n8 serial" \
--location=/root/CentOS-7-x86_64-Minimal-1511.iso

virsh connect --name qemu:///system

virsh list
```
 
