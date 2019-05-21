# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  config.vm.hostname = "combine"

  config.vm.box = "bento/ubuntu-18.04"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 4096
    vb.cpus = 2
    config.vm.network "private_network", ip: "192.168.45.10"
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.verbose = '-vvvvv'
    ansible.galaxy_role_file = "requirements.yml"
    ansible.extra_vars = {
        elasticsearch_heap_size: "512m",
        spark_driver_memory: "spark.driver.memory 1024m", # suggested 2048m+ for larger sets, which requires bumping vb.memory above as well from 4096 (4gb) to 6144 (6gb)
        livy_spark_master: "local[*]"
    }

  end

  # sets shared dir
  shared_dir = "/vagrant"
end
