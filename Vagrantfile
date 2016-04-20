# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.synced_folder "../", "/vagrant_data" # Mount directory up a level so puppet module list can find modules
  config.vm.synced_folder ".", "/vagrant"

  config.ssh.forward_x11 = true
  config.ssh.forward_agent = true

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "512"]
  end

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
    config.cache.synced_folder_opts = {
      type: :nfs,
      mount_options: ['rw', 'vers=3', 'tcp', 'nolock']
    }
  end

  config.vm.define "opensmtpd_test", primary: true, autostart: true do |server|
    server.vm.box = "debian/jessie64"
    server.vm.hostname = 'opensmtpd.arenstar.net'
    server.vm.network :private_network, ip: "192.168.56.100"
    server.vm.provision :shell, :path => "provision.sh"
    server.vm.provision :puppet, :manifests_path => ["vm","/vagrant/puppet"], :manifest_file => "setup.pp", :options => "--modulepath=/etc/puppet/modules --hiera_config /etc/hiera.yaml"
  end

end
