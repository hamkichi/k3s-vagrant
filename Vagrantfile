# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  
  config.vm.define "master" do |k3scluster|
      k3scluster.vm.hostname = "master.k3s"
      k3scluster.vm.synced_folder ".", "/vagrant", type:"virtualbox"
      k3scluster.vm.network :forwarded_port, id: "ssh", guest: 22, host: 2222
      k3scluster.vm.network "private_network", ip: "192.168.33.11", virtualbox__intnet: true
      k3scluster.vm.provider "virtualbox" do |vb|
        vb.memory = "1024"
        vb.name = "k3s-master"
      end
      k3scluster.vm.provision "shell", inline: <<-SHELL
        curl -sfL https://get.k3s.io | sh -

        NODE_TOKEN="/var/lib/rancher/k3s/server/node-token"
        while [ ! -e ${NODE_TOKEN} ]
        do
            sleep 1
        done
        cat ${NODE_TOKEN}
        cp ${NODE_TOKEN} /vagrant/
      SHELL
  end
  
  config.vm.define "node01" do |k3scluster|
      k3scluster.vm.hostname = "node01.k3s"
      k3scluster.vm.synced_folder ".", "/vagrant", type:"virtualbox"
      k3scluster.vm.network :forwarded_port, id: "ssh", guest: 22, host: 2223
      k3scluster.vm.network "private_network", ip: "192.168.33.12", virtualbox__intnet: true
      k3scluster.vm.provider "virtualbox" do |vb|
        vb.memory = "1024"
        vb.name = "k3s-node01"
      end
      k3scluster.vm.provision "shell", inline: <<-SHELL
        curl -sfL https://get.k3s.io | sh -
        systemctl stop k3s
        sed -i -e "s#k3s server#k3s agent --server https://192.168.33.11:6443 --token $(cat /vagrant/node-token)#g" /etc/systemd/system/k3s.service
        systemctl daemon-reload
        systemctl start k3s
      SHELL
  end
 
  config.vm.define "node02" do |k3scluster|
      k3scluster.vm.hostname = "node02.k3s"
      k3scluster.vm.synced_folder ".", "/vagrant", type:"virtualbox"

      k3scluster.vm.network :forwarded_port, id: "ssh", guest: 22, host: 2224
      k3scluster.vm.network "private_network", ip: "192.168.33.13", virtualbox__intnet: true
      k3scluster.vm.provider "virtualbox" do |vb|
        vb.memory = "1024"        
        vb.name = "k3s-node02"
      end
      k3scluster.vm.provision "shell", inline: <<-SHELL
        curl -sfL https://get.k3s.io | sh -
        systemctl stop k3s
        sed -i -e "s#k3s server#k3s agent --server https://192.168.33.11:6443 --token $(cat /vagrant/node-token)#g" /etc/systemd/system/k3s.service
        systemctl daemon-reload
        systemctl start k3s
        rm -f /vagrant/node-token
      SHELL
  end

end