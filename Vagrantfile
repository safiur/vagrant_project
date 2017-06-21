# -*- mode: ruby -*-
# vi: set ft=ruby :

# base VM configuration.
Vagrant.configure("2") do |config|
   config.vm.box = "centos7"
   config.vm.box_url = "https://github.com/tommy-muehle/puppet-vagrant-boxes/releases/download/1.1.0/centos-7.0-x86_64.box" 
   config.vm.synced_folder '/opt/vagrant', '/opt/vagrant'
   config.ssh.forward_agent = true  
   config.ssh.insert_key = false
   config.vm.provider :virtualbox do |v|
     v.memory = 512
    v.cpus = 1
    end
   
#Define two VMs with static private IP addresses.
     VMs = [
      { :name => "server1", :ip => "10.7.1.2" },
      { :name => "server2", :ip => "10.7.1.3" },
                         ]
   # Provision each of the VMs.
    VMs.each do |opts|
   config.vm.define opts[:name] do |config|
   config.vm.hostname = opts[:name]
   config.vm.network :private_network, ip: opts[:ip]
   config.vm.network :public_network, bridge: "eno1", use_dhcp_assigned_default_route: true 
     if opts[:name] == "server2"
     config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbooks/vmprovision.yml"
    ansible.inventory_path = "inventory"
    ansible.limit = "all"
  end
  end
  end
  end
  # client provisioning........
config.vm.define "client" do |client|
   client.vm.hostname = "client"
   client.vm.network :private_network, ip: "10.7.1.4"
   client.vm.network :public_network, bridge: "eno1"
   client.vm.provider :virtualbox do |c|
     c.memory = 512
    c.cpus = 1
    end

   client.vm.provision "ansible" do |ansible|
   ansible.playbook = "playbooks/vmclient.yml"
   ansible.inventory_path = "inventory"
   ansible.limit = "all"
end
 end

 end
