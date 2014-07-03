# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "hashicorp/precise64"

  config.vm.define 'admin' do |node|
    node.vm.hostname = 'admin'
    node.vm.network 'private_network', ip: '172.24.0.5', intnet: true
  end

  (1..3).each do |i|
    config.vm.define "ceph#{i}" do |node|
      node.vm.hostname = "ceph#{i}"
      node.vm.network "private_network", ip: "172.24.0.#{i}", intnet: true
      config.vm.provider "virtualbox" do |v|
        v.memory = 512
        v.cpus = 1
        v.customize ['createhd', '--filename', "ceph_disk_#{i}a.vdi", '--size', 8192 ]
        v.customize ['createhd', '--filename', "ceph_disk_#{i}b.vdi", '--size', 8192 ]
        v.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', "./ceph_disk_#{i}a.vdi"] 
        v.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', "./ceph_disk_#{i}b.vdi"] 
      end
    end

  end

  # Ansible prodvisioning
  config.vm.provision "ansible" do |ansible|
    ansible.groups = {
     "admin" => ["admin"],
     "ceph" => ["ceph1", "ceph2", "ceph3"]
    }
    ansible.sudo = true
    ansible.playbook = "ansible/ceph.yml"
  end

end
