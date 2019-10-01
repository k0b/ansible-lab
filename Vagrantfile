# Defines Vagrant environment
#
# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|

  # create mgmt node

  config.vm.define :mgmt do |mgmt_config|
      mgmt_config.vm.box = "xianlin/rhel-7"
      mgmt_config.vm.hostname = "mgmt"
      mgmt_config.vm.network :private_network, ip: "10.0.15.10"
      mgmt_config.vm.provider "virtualbox" do |vb, override|
        vb.memory = "256"
        vb.customize [
          "modifyvm", :id,
          "--natdnshostresolver1", "on",
          # some systems also need this:
         # "--natdnshostresolver2", "on"
        ]
      end
#      mgmt_config.vm.provision :shell, path: "bootstrap-mgmt.sh"
  end

  # create load balancer

  config.vm.define :lb do |lb_config|
      lb_config.vm.box = "bknight/consul-host"
      lb_config.vm.hostname = "lb"
      lb_config.vm.network :private_network, ip: "10.0.15.11"
      lb_config.vm.provider "virtualbox" do |vb, override|
        vb.memory = "256"
        vb.customize [
          "modifyvm", :id,
          "--natdnshostresolver1", "on",
          # some systems also need this:
          # "--natdnshostresolver2", "on"
        ]
      end
  end
  
  # create ansible tower vm
  
  config.vm.define :tower do |tower_config|
      tower_config.vm.box = "ansible/tower"
      tower_config.vm.hostname = "tower"
      tower_config.vm.network :private_network, ip: "10.0.15.20"
      tower_config.vm.provider "virtualbox" do |vb, override|
        vb.memory = "1024"
        vb.customize [
          "modifyvm", :id,
          "--natdnshostresolver1", "on"
        ]
      end
  end

  # create some ubuntu servers

  # https://docs.vagrantup.com/v2/vagrantfile/tips.html

  (1..3).each do |i|
    config.vm.define "ubuntu#{i}" do |node|
        node.vm.box = "ubuntu/trusty64"
        node.vm.hostname = "ubuntu#{i}"
        node.vm.network :private_network, ip: "10.0.15.2#{i}"
        node.vm.network "forwarded_port", guest: 80, host: "808#{i}"
        node.vm.network "forwarded_port", guest: 443, host: "844#{i}"
        node.vm.provider "virtualbox" do |vb|
          vb.memory = "256"
        end
    end
  end

  # create some rhel servers

  (1..3).each do |i|
    config.vm.define "rhel#{i}" do |node|
        node.vm.box = "generic/rhel7"
        node.vm.hostname = "rhel#{i}"
        node.vm.network :private_network, ip: "10.0.15.3#{i}"
        node.vm.network "forwarded_port", guest: 80, host: "809#{i}"
        node.vm.provider "virtualbox" do |vb|
          vb.memory = "256"
        end
    end
  end

end

