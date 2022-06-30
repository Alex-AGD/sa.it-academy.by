# Vagrant
## 05.Vagrant

### [Ubuntu link vagrant cloud](https://app.vagrantup.com/bonvoyajer/boxes/ubuntu)
### [Centos link vagrant cloud](https://app.vagrantup.com/bonvoyajer/boxes/centos)

### Pipeline Log
```bash
$ubuntu_script = <<-SCRIPT
apt-get update
apt-get install -yqq vim git wget curl nginx
systemctl enable nginx
systemctl start nginx
sudo chmod 666 /var/www/html/index.nginx-debian.html
sudo echo "<html><head><title>NGINX</title></head><body><h1>Test page</h1><h2>ubuntu</h2></$
SCRIPT

$centos_script = <<-SCRIPT
cd /etc/yum.repos.d/
sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.rep$
yum -y update
yum install -y vim git wget curl nginx
sudo systemctl enable nginx
sudo systemctl start nginx
sudo chmod 666 /usr/share/nginx/html/index.html
sudo echo "<html><head><title>NGINX</title></head><body><h1>Test page</h1><h2>centos</h2></$
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.define "host1" do |ubuntu|
    ubuntu.vm.box = "ubuntu/focal64"
    ubuntu.vm.network "forwarded_port", guest: 80, host: 8081
    ubuntu.vm.network "private_network", ip: "192.168.56.1"
    ubuntu.vm.provider "virtualbox" do |vb|
       vb.gui = false
       vb.memory = "1024"
       vb.cpus = "2"
    end
    ubuntu.vm.provision "shell",
        inline: $ubuntu_script
  end
  config.vm.define "host2" do |centos|
    centos.vm.box = "centos/8"
    centos.vm.network "forwarded_port", guest: 80, host: 8082
    centos.vm.network "private_network", ip: "192.168.56.2"
    centos.vm.provider "virtualbox" do |vb|
       vb.gui = false
       vb.memory = "1024"
    end
    centos.vm.provision "shell",
Vagrant.configure("2") do |config|
  config.vm.define "host1" do |ubuntu|
    ubuntu.vm.box = "ubuntu/focal64"
    ubuntu.vm.network "forwarded_port", guest: 80, host: 8081
    ubuntu.vm.network "private_network", ip: "192.168.56.1"
    ubuntu.vm.provider "virtualbox" do |vb|
       vb.gui = false
       vb.memory = "1024"
       vb.cpus = "2"
    end
    ubuntu.vm.provision "shell",
        inline: $ubuntu_script
  end
  config.vm.define "host2" do |centos|
    centos.vm.box = "centos/8"
    centos.vm.network "forwarded_port", guest: 80, host: 8082
    centos.vm.network "private_network", ip: "192.168.56.2"
    centos.vm.provider "virtualbox" do |vb|
       vb.gui = false
       vb.memory = "1024"
    end
    centos.vm.provision "shell",
        inline: $centos_script
  end
end
```
