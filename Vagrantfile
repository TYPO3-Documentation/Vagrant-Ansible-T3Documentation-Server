# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.box = "debian/jessie64"
  # jenkins
  config.vm.network "forwarded_port", guest: 8580, host: 8580
  # web server
  config.vm.network "forwarded_port", guest: 80, host: 8510
  config.vm.network "private_network", ip: "192.168.85.10"

  config.vm.provider "virtualbox" do |vb|

    # Customize the amount of memory on the VM:
    vb.memory = "1024"

    # Customize the number of cpus to use in this machine:
    vb.cpus = 2

    # Customize the amount of video ram in MB:
    vb.customize ["modifyvm", :id, "--vram", "128"]
  end

  config.vm.synced_folder ".", "/vagrant", disabled: true
  # config.vm.synced_folder ".", "/vagrant", type: "rsync"
  # Store cached update packages on the host (to keep those big Latex packages).
  # This requires that the virtualbox guest additions are installed.
  config.vm.synced_folder ".hibernate/var/cache/apt/archives", "/var/cache/apt/archives", type: "nfs"

end
