# -*- mode: ruby -*-
# vi: set ft=ruby ts=2 sw=2 tw=0 et :

role = File.basename(File.expand_path(File.dirname(__FILE__)))

vagrant_root = File.dirname(__FILE__)
ENV['ANSIBLE_ROLES_PATH'] = "#{vagrant_root}/../"

boxes = [
  {
  :name => "ubuntu1204",
  :box => "ubuntu/precise64",
  :ip => '10.0.77.11',
  :cpu => "25",
  :ram => "256"
  },
  {
  :name => "ubuntu1404",
  :box => "ubuntu/trusty64",
  :ip => '10.0.77.12',
  :cpu => "25",
  :ram => "256"
  },
  {
  :name => "debianjessie",
  :box => "debian/jessie64",
  :ip => '10.0.77.13',
  :cpu => "25",
  :ram => "256"
  },
  {
  :name => "ubuntu1604",
  :box => "ubuntu/xenial64",
  :ip => '10.0.77.14',
  :cpu => "25",
  :ram => "256"
  },
]

Vagrant.configure("2") do |config|
  #  if Vagrant.has_plugin?("vagrant-cachier")
  #    config.cache.scope = :box
  #  end

  # The symbolic link in test/roles is causing an rsync 'Too many levels of symbolic link' error for debian jessie
  config.vm.synced_folder ".", "/vagrant", type: "rsync",
  rsync__exclude: "tests/roles"

  boxes.each do |box|
    config.vm.define box[:name] do |vms|
      vms.vm.box = box[:box]
      vms.vm.box_url = box[:url]
      vms.vm.hostname = "#{box[:name]}"

      vms.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--cpuexecutioncap", box[:cpu]]
        v.customize ["modifyvm", :id, "--memory", box[:ram]]
      end

      vms.vm.network :private_network, ip: box[:ip]

      if box[:name].eql? "ubuntu1604"
        vms.vm.provision "shell",
        inline: "echo 'Installing python 2 required by Ansible.' && sudo apt-get -y install python-minimal"
      end

      vms.vm.provision :ansible do |ansible|
        ansible.playbook = "tests/vagrant.yml"
        ansible.verbose = "v"
      end
    end
  end
end
