# chef-zero-16.04
[Chef-Zero inside Ubuntu 16.04 Virtual box](https://app.vagrantup.com/taqtiqa/boxes/chef-zero-16.04)

## Requirements
Vagrant 
Internet connection

## Usage
Example `Vagrantfile` contents:
```ruby
VAGRANTFILE_API_VERSION = '2'
 
Vagrant.require_version '>= 1.5.0'
 
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.define 'chef-zero-16.04-vm' do |node|
    node.vm.box         = 'taqtiqa/chef-zero-16.04'
    node.vm.box_version = '14.3.37'
    node.vm.hostname    = 'chef-zero-16.04'
    node.vm.network :private_network, ip: '33.33.33.10'
  end
 
  config.vm.provider :virtualbox do |virtualbox|
    virtualbox.name   = 'chef-zero-16.04'
    virtualbox.memory = 1024
    virtualbox.cpus   = 1
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
```
## Build instructions
```bash
vagrant package --base chef-zero-16.04 --output /tmp/chef-zero-16.04.box
```
