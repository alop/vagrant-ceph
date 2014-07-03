#Vagrant Ceph
A quick and dirty way to setup a ceph cluster in vagrant.
Obviously requires [vagrant](http://vagrantup.com) and [virtualbox](http://virtualbox.org)
## Usage

Due to the complexity of vagrant load order and provisioning, it is important to run the initial up without provisioning, then after all the machines are up, run provision:

	vagrant up --no-provision
	vagrant provision
### Cluster layout
admin node

	ceph-deploy package
	
ceph1
	
	ceph monitor
	
ceph2-3
	
	ceph OSD
	8GB OS disk
	2*8GB Ceph data disks

### Ansible plays

* setup.yml - setup items that require root access
* ceph.yml - ceph-deploy steps run as vagrant