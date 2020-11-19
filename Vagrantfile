# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "centos/7"

  config.vm.provider :virtualbox do |v|
    v.memory = 2048
    v.cpus = 1
    v.linked_clone = true
  end

  boxes = [
    { :name => "es1", :ip => "192.168.33.71" },
    { :name => "es2", :ip => "192.168.33.72" },
    { :name => "es3", :ip => "192.168.33.73" },
    { :name => "es4", :ip => "192.168.33.74" },
    { :name => "es5", :ip => "192.168.33.75" }
  ]

  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]
      config.vm.network :private_network, ip: opts[:ip]

      if opts[:name] == "es5"
        config.vm.provision "ansible" do |ansible|
          ansible.compatibility_mode = "2.0"
          ansible.playbook = "main.yml"
          ansible.limit = "all"
          ansible.become = true
          ansible.groups = {
            "elasticsearch" => ["es1", "es2", "es3", "es4", "es5"],
            "elasticsearch:vars" => {
              elasticsearch_heap_size_min: "1g",
              elasticsearch_heap_size_max: "1g"
            },
            "elasticsearch_master" => ["es1", "es2", "es3", "es4"],
            "elasticsearch_master:vars" => {
              master_role: "true",
	      data_role: "true"
            },
            "elasticsearch_arbiter" => ["es5"],
            "elasticsearch_arbiter:vars" => {
              master_role: "false",
              data_role: "false"
            }
          }
        end
      end
    end
  end

end
