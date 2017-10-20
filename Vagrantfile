# -*- mode: ruby -*-
# vi: set ft=ruby :

#require necessary plugins
required_plugins = %w( vagrant-hostmanager vagrant-vbguest )
required_plugins.each do |plugin|
  system "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
end

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/xenial64" # 16.04

  # set memory to 1024m
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.cpus = "1"
  end

  # auto update guest additions
  config.vbguest.auto_update = true

  # vagrant-hostmanager is necessary to update /etc/hosts on hosts and guests
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.include_offline = true
  config.vm.network "private_network", ip: "192.168.233.101"
  config.vm.hostname = "docker.local.dev"
  #config.hostmanager.aliases = %w(alias.local.dev)

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder "ansible", "/vagrant/ansible", create: true, owner: "ubuntu", group: "ubuntu", mount_options: ["dmode=777,fmode=777"]
  config.vm.synced_folder "docker-images", "/home/ubuntu/docker-images", create: true, owner: "ubuntu", group: "ubuntu", mount_options: ["dmode=777,fmode=777"]

  # Run Ansible from the Vagrant VM
  config.vm.provision "ansible_local" do |ansible|
    ansible.install = true
    ansible.install_mode = :pip
    ansible.version = "latest"
    ansible.playbook = "ansible/playbook.yml"
    ansible.galaxy_role_file = "ansible/requirements.yml"
  end
end
