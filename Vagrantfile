# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = '2'
 
Vagrant.require_version '>= 1.5.0'
 
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  
  config.ssh.insert_key = false

  if Vagrant.has_plugin?("vagrant-proxyconf")
    config.proxy.enabled     = true
    if ENV["http_proxy"]
      config.proxy.http      = ENV['http_proxy']
      config.apt_proxy.http  = ENV['http_proxy']
    end
    if ENV["https_proxy"]
      config.proxy.https     = ENV['https_proxy']
      config.apt_proxy.https = ENV['https_proxy']
    end
    config.proxy.no_proxy    = "localhost,127.0.0.1,192.168.56.*"
  end

  config.vm.define 'chef-zero-16.04-vm' do |node|
    node.vm.box         = 'bento/ubuntu-16.04'
    node.vm.box_version = '201808.24.0' 
    node.vm.hostname    = 'chef-zero-16.04'
  end

  config.vm.provision "fix-no-tty", type: "shell" do |s|
    s.privileged = false
    s.inline = "sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile"
  end
 
  config.vm.provider :virtualbox do |virtualbox|
    virtualbox.name   = 'chef-zero-16.04'
    virtualbox.memory = 1024
    virtualbox.cpus   = 1
    virtualbox.customize [ "modifyvm", :id, "--uart1", "0x3f8","4"]
    virtualbox.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
  end
 
  config.vm.provision :chef_zero do |chef|
    chef.version        = '14.3.37'
    chef.node_name      = 'chef-zero-16.04'
    chef.cookbooks_path = ['.chef/cookbooks']
    chef.nodes_path     = '.chef/nodes'
    chef.roles_path     = '.chef/roles'
    chef.data_bags_path = '.chef/data_bags'
  end
end
