# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.require_version ">= 1.6.5"

# Add Vagrant hostmanager plugin support if required
system 'vagrant plugin install vagrant-hostmananger' if $hostmanager unless Vagrant.has_plugin?('vagrant-hostmanager')
# Add VMWare Fusion support if required
system 'vagrant plugin install vmware_fusion' if ENV['VAGRANT_DEFAULT_PROVIDER'] == 'vmware_fusion' unless Vagrant.has_plugin?('vmware_fusion')

system 'vagrant plugin install vagrant-berkshelf' unless Vagrant.has_plugin?('vagrant-berkshelf')
system 'vagrant plugin install vagrant-omnibus' unless Vagrant.has_plugin?('vagrant-omnibus')

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.hostname = "mcxess-freeswitch"

  config.vm.box = "chef/ubuntu-12.04"

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--memory", "2048"]
    vb.name = "mcxess-freeswitch"
  end

  config.vm.provider "virtualbox" do |vm|
    vm.vmx["memsize"] = "2048"
    vm.name = "mcxess-freeswitch"
  end

  config.vm.network :private_network, type: "dhcp"

  # H.323 Gatekeeper RAS port
  config.vm.network 'forwarded_port', guest: 1719, host: 1719, auto_correct: true
  # H.323 Call Signaling
  config.vm.network 'forwarded_port', guest: 1720, host: 1720, auto_correct: true
  # STUN service
  config.vm.network 'forwarded_port', guest: 3478, host: 3478, auto_correct: true
  config.vm.network 'forwarded_port', guest: 3479, host: 3479, auto_correct: true
  # MLP protocol server
  config.vm.network 'forwarded_port', guest: 5002, host: 5002, auto_correct: true
  # Neighborhood service
  config.vm.network 'forwarded_port', guest: 5003, host: 5003, auto_correct: true
  # SIP
  config.vm.network 'forwarded_port', guest: 5060, host: 5060, auto_correct: true
  config.vm.network 'forwarded_port', guest: 5070, host: 5070, auto_correct: true
  config.vm.network 'forwarded_port', guest: 5080, host: 5080, auto_correct: true
  # WebRTC
  config.vm.network 'forwarded_port', guest: 5066, host: 5066, auto_correct: true
  config.vm.network 'forwarded_port', guest: 7443, host: 7443, auto_correct: true

  config.berkshelf.enabled = true

  config.vm.provision :chef_solo do |chef|
    chef.json = {
      resolver: {
        nameservers: '8.8.8.8'
      }
    }

    chef.run_list = [
        "recipe[freeswitch::default]"
    ]
  end
end
