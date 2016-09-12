# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.network :forwarded_port, guest: 8000, host: 8000

  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
  end

  config.vm.provision "docker", {}
end
