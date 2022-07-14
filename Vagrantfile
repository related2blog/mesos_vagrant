# -*- mode: ruby -*-
# vi: set ft=ruby :
# update centos mirrros
$script = <<-SCRIPT
cd /etc/yum.repos.d/
sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
dnf makecache
dnf -y install epel-release
sed -i 's/metalink/#metalink/g' /etc/yum.repos.d/epel*
sed -i 's|#baseurl=https://download.example/pub/|baseurl=https://mirror.init7.net/fedora/|g' /etc/yum.repos.d/epel*
dnf -y install curl gcc libffi-devel openssl-devel compat-openssl10
SCRIPT

Vagrant.configure(2) do |config|
  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus    = 2
  end
  config.vm.box = "bento/centos-8"
  config.vm.network "public_network", ip: "192.168.0.123",bridge: "wlp3s0: Wifi"
  config.vm.hostname = "node1"
  config.vm.provision "shell", inline: $script
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
  end
end
