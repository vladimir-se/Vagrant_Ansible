# -*- mode: ruby -*-
# vi: set ft=ruby :

nodes = [
  {
    :hostname => "dhcp-server",
    :ip => "192.168.99.1",
    :cpu => 1,
    :mem => 512,
    :netname => "mynet",
    :box => "debian/buster64",
    :playbook_number => "playbook1",
    :playbook => "provisioning/dhcp-server.yml"
  },
  {
    :hostname => "dhcp-client",
    :ip => "0.0.0.0",
    :cpu => 1,
    :mem => 512,
    :netname => "mynet",
    :box => "debian/buster64",
    :playbook_number => "playbook2",
    :playbook => "provisioning/dhcp-client.yml"
  },
]

Vagrant.configure("2") do |config|
  nodes.each do |node|
    config.vm.box_check_update = false
    config.vm.define node[:hostname] do |nodecfg|
      nodecfg.vm.box = node[:box]
      nodecfg.vm.hostname = node[:hostname]
      nodecfg.vm.network :private_network,
        ip: node[:ip],
        virtualbox__intnet: node[:netname]
      nodecfg.vm.provider :virtualbox do |vb|
        vb.memory = node[:mem]
        vb.cpus = node[:cpu]
      end
      nodecfg.vm.provision node[:playbook_number], type: "ansible" do |ansible|
        ansible.limit = "all"
        ansible.playbook = node[:playbook] 
      end
    end
  end
end
