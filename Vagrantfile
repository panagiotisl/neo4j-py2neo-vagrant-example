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
  config.vm.box = "bento/ubuntu-18.04"

  ############################################################
  # Provision Docker with Vagrant before starting minikube
  ############################################################
  #config.vm.provision :docker do |d|
  #  d.pull_images "neo4j"
  #  d.run "neo4j",
  #    args: "-d --name neo4j -p 7474:7474 -p 7687:7687 --env NEO4J_AUTH=neo4j/m222 --env=NEO4JLABS_PLUGINS=['apoc']"
  #end

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 7474, host: 7474, host_ip: "0.0.0.0"
  config.vm.network "forwarded_port", guest: 7687, host: 7687, host_ip: "0.0.0.0"
 
 
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
  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
    vb.memory = "2048"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
  apt-get update
  apt install apt-transport-https ca-certificates curl software-properties-common -y
  curl -fsSL https://debian.neo4j.com/neotechnology.gpg.key | apt-key add -
  add-apt-repository "deb https://debian.neo4j.com stable 4.1"
  apt install neo4j -y
  echo "dbms.default_listen_address=0.0.0.0" >> /etc/neo4j/neo4j.conf
  neo4j-admin set-initial-password m222
  cd /var/lib/neo4j/plugins/
  wget https://github.com/neo4j-contrib/neo4j-apoc-procedures/releases/download/4.1.0.4/apoc-4.1.0.4-all.jar
  chown neo4j /var/lib/neo4j/plugins/apoc-4.1.0.4-all.jar
  echo "dbms.security.procedures.whitelist=apoc.coll.*,apoc.load.*,apoc.*" >> /etc/neo4j/neo4j.conf
  systemctl enable neo4j.service
  systemctl restart neo4j.service
  apt-get -y install python3-pip
  pip3 install py2neo
  pip3 install Flask
  SHELL
end
