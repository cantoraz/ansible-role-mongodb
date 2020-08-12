# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/bionic64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # config.vm.synced_folder '.vagrant/public', '/mnt/dskb-web/public',
  #                         owner: 'www-data', group: 'www-data',
  #                         mount_options: %w[dmode=0775,fmode=0664]

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
    vb.cpus = 1
    vb.linked_clone = true
    # CAUTION
    # Invoke this directive ONLY ONCE before attaching additional disks,
    # then comment out it for subsequent using.
    # Vagrant Disks feature has a quirk that additional disks are always
    # attached to the controller named `SATA Controller'. The Ubuntu Official
    # box only has the SCSI controller. While VirtualBox boots up from SATA
    # disks in preference over SCSI disks. So renaming the present SCSI
    # Controller to `SATA Controller' is the simplest solution.
    # vb.customize ['storagectl', :id, '--name', 'SCSI', '--rename', 'SATA Controller']
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.
  vms = %w[db1 db2 db3]
  vms.each_with_index do |host, i|
    config.vm.define host do |machine|
      machine.vm.hostname = "#{host}"
      machine.vm.network 'private_network', ip: "10.0.0.#{11 + i}", nic_type: 'virtio'

      # Only execute once the Ansible provisioner,
      # when all the machines are up and ready.
      if i == vms.length - 1
        machine.vm.provision :ansible do |ansible|
          # Disable default limit to connect to all the machines
          ansible.limit = 'all'
          ansible.playbook = 'tests/playbook.yml'
          ansible.verbose = 'vv'
        end
      end
    end
  end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL

  # config.vm.provision "ansible" do |ansible|
  #   ansible.playbook = "tests/playbook.yml"
  #   ansible.verbose = 'vv'
  # end
end
