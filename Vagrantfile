# Defines our Vagrant environment
#
# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|

  # create some ubuntu servers

  # https://docs.vagrantup.com/v2/vagrantfile/tips.html

  (1..2).each do |i|
    config.vm.define "ubuntu#{i}" do |web_conf|
        web_conf.vm.box = "generic/ubuntu2010"
        web_conf.vm.hostname = "ubuntu#{i}"
        web_conf.vm.network :private_network, ip: "192.168.180.2#{i}"
        web_conf.vm.network "forwarded_port", guest: 80, host: "808#{i}"
        web_conf.vm.provider "vmware_workstation" do |vb|
          vb.vmx["memsize"] = 512
          vb.vmx["ethernet0.pcislotnumber"] = "32"
          vb.vmx["ethernet1.pcislotnumber"] = "33"
          vb.vmx["ethernet1.vnet"] = "/dev/vmnet8"
        end
    end
  end

  # create mgmt node

  config.vm.define :mgmt do |mgmt_conf|
      mgmt_conf.vm.box = "generic/centos8"
      mgmt_conf.vm.hostname = "mgmt"
      mgmt_conf.vm.network :private_network, ip: "192.168.180.10"
      mgmt_conf.vm.provider "vmware_workstation" do |vb|
        vb.vmx["memsize"] = 1024
        vb.vmx["ethernet1.vnet"] = "/dev/vmnet8"
      end
#      mgmt_config.vm.provision :shell, path: "bootstrap-mgmt.sh"
  end

  # create lb node

  config.vm.define :lb do |lb_conf|
      lb_conf.vm.box = "generic/centos8"
      lb_conf.vm.hostname = "lb"
      lb_conf.vm.network :private_network, ip: "192.168.180.100"
      lb_conf.vm.provider "vmware_workstation" do |vb|
        vb.vmx["memsize"] = 512
        vb.vmx["ethernet1.vnet"] = "/dev/vmnet8"
      end
  end

  # create some db servers

  (1..1).each do |i|
    config.vm.define "db#{i}" do |db_conf|
        db_conf.vm.box = "generic/centos8"
        db_conf.vm.hostname = "db#{i}"
        db_conf.vm.network :private_network, ip: "192.168.180.3#{i}"
        db_conf.vm.provider "vmware_workstation" do |vb|
          vb.vmx["memsize"] = 512
          vb.vmx["ethernet1.vnet"] = "/dev/vmnet8"
        end
    end
  end

end

