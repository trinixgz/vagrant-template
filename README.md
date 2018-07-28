# Vagrant Template
Template for Vagrant projects, using a YAML file to manage all of the 
guest machines in VirtualBox
## Defining Guest Machines 
Guest machines can be defined in the [`guest_machines.yml`](guest_machines.yml) file  
The guest `name`, `box`, `cpus`, `memory` and `private_ip` must be defined:
```yaml
- name: default
  box: centos/7
  cpus: 2
  memory: 4096 
  private_ip: 10.0.10.10
```
## Guest Machine Properties
### Required
- **name**  
	The name is how you can interact with the guest via Vagrant, 
	for example to only start the virtual machine called `default` 
	you could run `vagrant up default`
- **box**  
	The name of the box to be provisioned on the guest machine 
	that is either installed locally or downloaded
	from the [Vagrant Cloud](https://app.vagrantup.com/boxes/search)
- **cpus**  
	Amount of cpus to assign to the guest machine
- **memory**  
	Amount of RAM in Megabytes to assign to the guest machine
- **private_ip**  
	Private IP address of the guest machine, 
	which will allow connectivity from the host
### Optional
- **synced_folders**  
	Folders to syncronise to the guest machine from the host, 
	this could be useful for number of reasons, such as syncing
	a local maven repository:
	```yaml
	---
	- name: default
	  box: centos/7
	  cpus: 2
	  memory: 4096 
	  private_ip: 10.0.10.10
	  synced_folders:
      - host: ~/.m2
        guest: /home/vagrant/.m2
	...
	```
	**Note**: for synced folders the VirtualBox Guest Additions
	plugin must be installed for Vagrant:
	```bash
	vagrant plugin install vagrant-vbguest
	```	
- **package_manager**  
	The supported package managers are `yum`, `apt` and `apk`
	when you choose a package, a global package update will be run
	```yaml
	package_manager: yum
	```
- **packages**  
	Packages that will be installed using the `package_manager` that has been selected
	```yaml
	packages:
	- wget
	- unzip
	- vim
	```
- **scripts**  
	Scripts may be created in the `vagrant_scripts` folder so that
	they can be accessed. To run a script, include the name of the
	file in the scripts list for the guest:
	```yaml
	scripts:
	- startup
	- configure_firewall.sh
	```
