# -*- mode: ruby -*-
# vi: set ft=ruby :

ip_address = "10.11.2.100"
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Configure cached packages to be shared between instances of the same base box.
  if Vagrant.has_plugin?("vagrant-berkshelf")
      config.berkshelf.enabled = true
  end

  config.omnibus.chef_version = :latest
  config.dnsmasq.domain = 'squid.sandbox'
  # overwrite default location for /etc/dnsmasq.conf
  # brew_prefix = `brew --prefix`.strip
  config.dnsmasq.dnsmasqconf = '/usr/local/etc/dnsmasq.conf'

  # command for reloading dnsmasq after config changes
  config.dnsmasq.reload_command = 'sudo launchctl unload /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist; sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist'
  config.dnsmasq.keep_resolver_on_destroy = true
  config.dnsmasq.ip = ip_address

  # Configure cached packages to be shared between instances of the same base box.
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

  config.vm.box_check_update = false
  config.vm.box = "parallels/centos-6.5"

	config.vm.define :squid_server do |squidsrv| 
    
    config.vm.provider :virtualbox do |vb|
      vb.gui = true
    end

    #config.vm.box_url = "http://files.vagrantup.com/parallels/rhel-6.4-x86_64"
    config.vm.network :private_network, :ip => ip_address
    config.vm.hostname = "squid"
        
    config.vm.provision :chef_solo do |chef|
      chef.add_recipe "squid-cookbook"
    end
  end
end
