# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.
  
  config.vm.hostname = "combine"

  config.vm.box = "bento/ubuntu-18.04"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 16384
    vb.cpus = 2
    config.vm.network "private_network", ip: "192.168.45.10"
  end

  config.vm.provision :ansible do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.verbose = '-vvvvv'
  end

  # sets shared dir
  shared_dir = "/vagrant"  
end
