Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/bionic64"
  config.vm.box_check_update = false
  config.ssh.insert_key = false
  config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'" 
  
  # Ansible Control Machine
  config.vm.define "control", primary: true do |control|
	  control.vm.hostname = "Control"
	  control.vm.network "private_network", ip: "192.168.33.50"
	  control.vm.synced_folder ".", "/home/vagrant/ansible", id: "vagrant-root", disabled: false

	  control.vm.provider "virtualbox" do |vb|
		     # Do not load the command line GUI
		     vb.gui = false

		     # Virtual Machine Name
		     vb.name = "Control"

		     # Network settings
		     vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
	             vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]

		     # Use VBoxManage to customize the VM
		     vb.customize ["modifyvm", :id, "--memory", "512"]
	  end
      
	  control.vm.provision "shell", path: "provision.sh"
	  control.vm.provision "shell", inline: "cp /home/vagrant/ansible/hosts /etc/hosts"
  end

  # Web Server Configuration
  config.vm.define "web1", autostart: false do |web|
	  web.vm.hostname = "web1"
	  web.vm.network "private_network", ip: "192.168.33.100"
	  web.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true

	  web.vm.provider "virtualbox" do |vb|
		  # Do not load the command line GUI
		  vb.gui = false

		  # Virtual Machine Name
		  vb.name = "web1"

		  # Network settings
		  vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
		  vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]

		  # Use VBoxManage to customize the VM
		  vb.customize ["modifyvm", :id, "--memory", "1024"]
	  
	  end
	  web.vm.provision "file", source: "hosts" , destination: "/tmp/hosts"
	  web.vm.provision "shell", inline: "cp /tmp/hosts /etc/hosts"
  end

  config.vm.define "web2", autostart: false do |web|
	  web.vm.hostname = "web2"
	  web.vm.network "private_network", ip: "192.168.33.101"
	  web.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true

	  web.vm.provider "virtualbox" do |vb|
		  # Do not load the command line GUI
		  vb.gui = false

		  # Virtual Machine Name
		  vb.name = "web2"

		  # Network settings
		  vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
		  vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]

		  # Use VBoxManage to customize the VM
		  vb.customize ["modifyvm", :id, "--memory", "1024"]
	  
	  end
	  web.vm.provision "file", source: "hosts" , destination: "/tmp/hosts"
	  web.vm.provision "shell", inline: "cp /tmp/hosts /etc/hosts"
  end

  config.vm.define "web3", autostart: false do |web|
	  web.vm.box = "centos/7"
	  web.vm.hostname = "web3"
	  web.vm.network "private_network", ip: "192.168.33.103"
	  web.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true

	  web.vm.provider "virtualbox" do |vb|
		  # Do not load the command line GUI
		  vb.gui = false

		  # Virtual Machine Name
		  vb.name = "web3"

		  # Network settings
		  vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
		  vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]

		  # Use VBoxManage to customize the VM
		  vb.customize ["modifyvm", :id, "--memory", "1024"]
	  
	  end
	  web.vm.provision "file", source: "hosts" , destination: "/tmp/hosts"
	  web.vm.provision "shell", inline: "cp /tmp/hosts /etc/hosts"
  end

  # Database Server Configuration
  config.vm.define "database", autostart: false do |database|
	  database.vm.hostname = "database"
	  database.vm.network "private_network", ip: "192.168.33.200"
	  database.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true

	  database.vm.provider "virtualbox" do |vb|
		  # Do not load the command line GUI
		  vb.gui = false

		  # Virtual Machine Name
		  vb.name = "database"

		  # Network settings
		  vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
		  vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]

		  # Use VBoxManage to customize the VM
		  vb.customize ["modifyvm", :id, "--memory", "1024"]
	  
	  end
	  database.vm.provision "file", source: "hosts" , destination: "/tmp/hosts"
	  database.vm.provision "shell", inline: "cp /tmp/hosts /etc/hosts"
  end

  config.vm.define "loadbalancing", autostart: false do |lb|
	  lb.vm.hostname = "loadbalancing"
	  lb.vm.network "private_network", ip: "192.168.33.30"
	  lb.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true

	  lb.vm.provider "virtualbox" do |vb|
		  # Do not load the command line GUI
		  vb.gui = false

		  # Virtual Machine Name
		  vb.name = "loadbalancing"

		  # Network settings
		  vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
		  vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]

		  # Use VBoxManage to customize the VM
		  vb.customize ["modifyvm", :id, "--memory", "1024"]
	  
	  end
	  lb.vm.provision "file", source: "hosts" , destination: "/tmp/hosts"
	  lb.vm.provision "shell", inline: "cp /tmp/hosts /etc/hosts"
  end

end
