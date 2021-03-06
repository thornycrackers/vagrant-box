# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "ubuntu/trusty64"

  # Sync folder
  config.vm.synced_folder "salt/srv/", "/srv/salt/"

  # Provision via salt
  config.vm.provision :salt do |salt|
      # Allow pip based installations for tornado >= 4.0
      salt.bootstrap_options = "-F -c /tmp -P"

      # Better output
      salt.verbose = true
      salt.colorize = true
      
      # Install with git to specify version
      salt.install_type = "git"
      salt.install_args = "v2015.8.0"
  end

  # Create Salt Minion
  config.vm.provision "shell" do |s|
      s.path = "salt/setup.sh"
      s.privileged = true
  end
  
  # Run Highstate
  config.vm.provision :shell, inline: 'sudo salt-call state.highstate'

  config.vm.provider :virtualbox do |virtualbox|
    # Helps OSX provision multiple cores
    virtualbox.customize ["modifyvm", :id, "--ioapic", "on"  ]
    # allocate 8GB RAM
    virtualbox.customize ["modifyvm", :id, "--memory", "8192"] 
    # allocate max 50% CPU
    virtualbox.customize ["modifyvm", :id, "--cpuexecutioncap", "75"]
    # Give acces to multiple cpus
    virtualbox.customize ["modifyvm", :id, "--cpus", "8"]
  end

  # Needed for NFS
  config.vm.network :private_network, ip: "10.11.12.13"
  # Use NFS for shared folders, much better performance BUT doesn't work on windows
  config.vm.synced_folder '.', '/vagrant', type: "nfs", mount_options: ['actimeo=2']

  # Let Vagrant get a public ip
  #config.vm.network :public_network
  
  # Setting a default interface 
  config.vm.network "public_network", bridge: 'en0: Wi-Fi (AirPort)'
  
end
