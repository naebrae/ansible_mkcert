# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.define :makecert01 do |makecert01_config|
    makecert01_config.vm.box = "almalinux/8"
    makecert01_config.vm.provider "virtualbox" do |makecert01_vb|
      makecert01_vb.name = "makecert01"
    end
  end

  config.vm.define :makecert02 do |makecert02_config|
    makecert02_config.vm.box = "almalinux/9"
    makecert02_config.vm.provider "virtualbox" do |makecert02_vb|
      makecert02_vb.name = "makecert02"
    end
  end

  config.vm.define :makecert03 do |makecert03_config|
    makecert03_config.vm.box = "bento/ubuntu-22.04"
    makecert03_config.vm.provider "virtualbox" do |makecert03_vb|
      makecert03_vb.name = "makecert03"
    end
  end

  config.vm.define :makecert04 do |makecert04_config|
    makecert04_config.vm.box = "debian/bullseye64"
    makecert04_config.vm.provider "virtualbox" do |makecert04_vb|
      makecert04_vb.name = "makecert04"
    end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
  end

  config.vm.synced_folder ".", "/home/vagrant/sync", type: "rsync", disabled: true
  config.vm.synced_folder ".", "/vagrant", disabled: true

end
