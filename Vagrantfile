# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  config.vm.hostname = 'local.io'

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network :forwarded_port, guest: 443, host: 8443
  config.vm.network :forwarded_port, guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.56.101"

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  config.ssh.forward_agent = true  

  # Declare shared folder with Vagrant syntax
  config.vm.synced_folder '/path/in/host', '/temporary/path', type: "nfs", nfs_version: "4", nfs_udp: false
  
   # Use vagrant-bindfs to re-mount folder
  config.bindfs.bind_folder "/temporary/path", "/final/path/in/guest", owner: "serverpilot", group: "serverpilot", o: "nonempty"

  config.vm.provision "shell", inline: "apt-get update && apt-get install -y curl"
end
