# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos7"

  config.vm.define "master" do |kube|
    kube.vm.network :private_network, ip: "192.168.100.100"

    kube.vm.synced_folder ".", "/home/vagrant/prometheus", :create => true, :owner=> 'vagrant', :group=>'vagrant', :mount_options => ['dmode=777,fmode=777']
    kube.vm.synced_folder "../ansible", "/vagrant", :create => true, :owner=> 'vagrant', :group=>'vagrant', :mount_options => ['dmode=777,fmode=666']

    kube.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024"]
      vb.customize ["modifyvm", :id, "--usb", "on"]
    end

    if ARGV[0] == 'up'
      kube.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "prometheus_master.yml"
        ansible.limit = "server"
        ansible.inventory_path = 'inventory/localhost'
      end
    end
  end

  config.vm.define "node" do |kube|
    kube.vm.network :private_network, ip: "192.168.100.101"

    kube.vm.synced_folder ".", "/home/vagrant/prometheus", :create => true, :owner=> 'vagrant', :group=>'vagrant', :mount_options => ['dmode=777,fmode=777']
    kube.vm.synced_folder "../ansible", "/vagrant", :create => true, :owner=> 'vagrant', :group=>'vagrant', :mount_options => ['dmode=777,fmode=666']

    kube.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "512"]
      vb.customize ["modifyvm", :id, "--usb", "on"]
    end

    if ARGV[0] == 'up'
      kube.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "prometheus_node.yml"
        ansible.limit = "server"
        ansible.inventory_path = 'inventory/localhost'
      end
    end
  end
end
