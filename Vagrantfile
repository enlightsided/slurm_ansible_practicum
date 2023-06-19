# -*- mode: ruby -*-
# vi: set ft=ruby :
 
# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.provision "file", source: "files/newkey.pub", destination: "/home/vagrant/.ssh/"
  config.vm.define "controlnode" do |controlnode|
    controlnode.vm.box = "ubuntu/focal64"
    controlnode.vm.box_url = "https://cloud-images.ubuntu.com/focal/current/focal-server-cloudimg-amd64-vagrant.box" 
    controlnode.vm.hostname = "controlnode"
    controlnode.vm.network "private_network", ip: "192.168.150.50"
    controlnode.vm.synced_folder "./ansible","/home/vagrant/ansible"
    controlnode.vm.provision "file", source: "files/newkey", destination: "/home/vagrant/.ssh/"
    controlnode.vm.provision "shell", inline: <<-SHELL
      sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config
      sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/#g' /etc/ssh/sshd_config
      service ssh restart
      echo "192.168.150.150   centos    centos" >> /etc/hosts
      sudo add-apt-repository -y ppa:ansible/ansible
      sudo apt update -y && sudo apt -y install sshpass ansible
      chmod 600 /home/vagrant/.ssh/newkey
      chmod 644 /home/vagrant/.ssh/newkey.pub
    SHELL
  end
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.define "centos" do |centos|
    centos.vm.box = "bento/centos-7.9"
    centos.vm.box_url = "https://cloud.centos.org/centos/7/vagrant/x86_64/images/CentOS-7-x86_64-Vagrant-2004_01.VirtualBox.box"
    centos.vm.box_download_options = {"ssl-no-revoke" => true}
    centos.vm.hostname = "centos"
    centos.vbguest.auto_update = false
    centos.vm.network "private_network", ip: "192.168.150.150"
    centos.vm.provision "shell", inline: <<-SHELL
      chmod 644 /home/vagrant/.ssh/newkey.pub
      cat /home/vagrant/.ssh/newkey.pub >> /home/vagrant/.ssh/authorized_keys
    SHELL
  end
end