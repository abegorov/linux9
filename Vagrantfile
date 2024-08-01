# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV["LC_ALL"] = "en_US.UTF8"

MACHINES = {
  :'nfs-server' => {
    :box => 'generic/centos9s',
    :cpus => 1,
    :memory => 512,
    :networks => [
      [ :private_network, { :ip => '192.168.56.10' } ]
    ]
  },
  :'nfs-client' => {
    :box => 'generic/centos9s',
    :cpus => 1,
    :memory => 512,
    :networks => [
      [ :private_network, { :ip => '192.168.56.11' } ]
    ]
  }
}

Vagrant.configure("2") do |config|
  MACHINES.each do |host_name, host_config|
    config.vm.define host_name do |host|
      host.vm.box = host_config[:box]
      host.vm.host_name = host_name.to_s

      host.vm.provider :virtualbox do |vb|
        vb.cpus = host_config[:cpus]
        vb.memory = host_config[:memory]
      end

      host_config[:networks].each do |network|
        host.vm.network(network[0], **network[1])
      end

      if MACHINES.keys.last == host_name
        host.vm.provision :ansible do |ansible|
          ansible.playbook = 'playbook.yml'
          ansible.limit = 'all'
          ansible.compatibility_mode = '2.0'
          ansible.raw_arguments = ['--diff']
        end
      end
    end
  end
end
