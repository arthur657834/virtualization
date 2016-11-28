* 1 安装virtualBox,vagrant

* 2 box下载地址http://www.vagrantbox.es/

* 3 
```
vagrant box add {title} {url} <=>vagrant box add centos6.3 F:\vagrant\images\CentOS-6.3-x86_64-minimal.box

vagrant init {title}

vagrant up
```
* 4 
```
https://www.vagrantup.com/docs/cli/

vagrant box list             \# 列出当前导入的box

vagrant destory              \# ***machine

vagrant box remove [name]    \# 移除box

vagrant up [name]            \# 启动machine  

vagrant halt [name]          \# 关闭machine 

vagrant status [name]        \# 查看machine的状态

vagrant ssh [name] 

vagrant halt [name] 

vagrant suspend [name] 

vagrant ssh-config [name] 

vagrant reload  [name]

 vagrant reload --provision如果环境挂了，可以重启.如果加了--provision, 就会恢复资料库

```

* 5 
```
登录IP:127.0.0.1

连接账号：vagrant
连接密码：vagrant
su到root下密码：vagrant

打包之前最好rm -f /etc/udev/rules.d/70-persistent-net.rules 
vagrant package haproxy --output haproxy.box --vagrantfile Vagrantfile


vagrant plugin install vagrant-berkshelf
vagrant plugin install vagrant-hostmanager
vagrant plugin install vagrant-omnibus

[HOWTO] 指定要使用的vagrant box和设置VM名称

Vagrant.configure("2") do |config|      # ‘2’的意思是我所用的vagrant-1.2.2属于内部v2版 
  config.vm.define :web do |web_config| # 设置此VM名称为web
    web_config.vm.box = "apache-centos" # 指定使用已安装的名为“apache-centos”的vagrant box
  end
end

[HOWTO] 设置VM对宿主机和外部机器的网络连接

当VM只与宿主机通信时，可设置为私有网络，等效于设置virtualbox使用host-only模式网络适配器。
 config.vm.network :private_network, ip: "192.168.50.4"
当寄宿与同一宿主机上的多台VM之间也需相互通信时，设置各VM为私有网络，但将固定IP设为同一网段。
当VM需要与宿主机网络内的其他机器通信时，设置为公开网络，等效于设置virtualbox使用bridged模式网络适配器，默认DHCP获取地址。
 config.vm.network :public_network   # vagrant-1.2.2之后的版本可能改用 :bridged
 
[HOWTO] 配置并启动VM后运行后续安装脚本

追求简单，那么下面两条配置其一就够用了
 config.vm.provision :shell, :inline => "ifconfig"  # inline script

 config.vm.provision :shell, :path => "initialize.sh"  # external script
不得不复杂时， Chef / Puppet / Ansible 都是可供你用的。细致配置见 参考 。
自动化部署是好的，但如果是VM每次安装固定软件，那么就不必了。大可以将不变的软件安装配置完成后，将VM打包成为*.box文件静态化，以后以此box文件作为基础模板做进一步配置。步步为营的提升基础的实践，会加速VM搭建过程并减少配置脚本。


修改默认ssh端口
修改vm的ssh配置再修改Vagrantfile

config.vm.network :forwarded_port, guest: 10022, host: 2255
config.ssh.port = 2255          # port of host  # uncommented in step3
config.ssh.guest_port = 10022   # port of VM    # uncommented in step3

设置共享目录位置与读写权限
vagrant 默认将宿主机上Vagrantfile所载目录共享至VM上的 /vagrant 目录。


vboxsf:Virtualbox Shared Folder
    config.vm.synced_folder "path/on/host", "/absolute/path/on/vm"
    前一个参数须是宿主机上已存在的目录，若为相对目录，那是相对Vagrantfile所在目录。
    后一个参数须是VM上的绝对路径，若不存在，vagrant会在启动VM时建好，多层的目录也没关系。

NFS:
    config.vm.synced_folder "path/on/host", "/absolute/path/on/vm", :nfs => true

problem:
    The following SSH command responded with a non-zero exit status.
    sudo rm -f /etc/udev/rules.d/70-persistent-net.rules 
```