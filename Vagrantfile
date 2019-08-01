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
  # config.vm.box_check_update = false

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

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    # Create admins group. somewhat redundant, just for demo
    groupadd myadmins

    # Add vagrant to admins (so it can SSH)
    usermod -a -G myadmins vagrant

    # Passwordless sudo for admins
    echo "%myadmins ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/myadmins

    # Allow SSH access for admins (and nobody else)
    grep "AllowGroups myadmins" /etc/ssh/sshd_config || echo "AllowGroups myadmins" >> /etc/ssh/sshd_config

    # Create test user 1: member of myadmins group
    useradd --shell /bin/bash --user-group --create-home --groups myadmins john
    mkdir -p /home/john/.ssh
    cp /home/vagrant/.ssh/authorized_keys /home/john/.ssh/authorized_keys
    chown -R john:john /home/john/
    chmod 0700 /home/john/.ssh

    # Create test user 2: not member of myadmins group
    useradd --shell /bin/bash --user-group --create-home notjohn
    mkdir -p /home/notjohn/.ssh
    cp /home/vagrant/.ssh/authorized_keys /home/notjohn/.ssh/authorized_keys
    chown -R notjohn:notjohn /home/notjohn/
    chmod 0700 /home/notjohn/.ssh

    # Reload sshd
    service sshd reload

  SHELL
end
