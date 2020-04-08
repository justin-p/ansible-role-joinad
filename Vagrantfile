# Require YAML module
require 'yaml'
 
# Read YAML file with box details
inventory = YAML.load_file('tests/inventory.yml')

domain_members_count = inventory['all']['children']['domain_members']['hosts'].keys.count
current_host = 1

Vagrant.configure("2") do |config|
  config.vm.define "dc" do |dc|
    inventory['all']['children']['primarydomaincontroller']['hosts'].each do |server,details|
      dc.vm.box = details['vagrant_box']
      dc.vm.hostname = server
      dc.vm.network :private_network, ip: details['ansible_host']
      inventory['all']['vars']['vagrant_ports'].each do |protocol,details|
        dc.vm.network :forwarded_port, guest: details['guest'], host: details['host'], id: protocol
      end
      dc.vm.provider :virtualbox do |v|
        v.name = File.basename(File.dirname(__FILE__)) + "_" + server + "_" + Time.now.to_i.to_s
        v.gui = false
        v.memory = 2048
        v.cpus = 2
      end
      dc.vm.provision "ansible" do |ansible|
        ansible.limit = "all"
        ansible.playbook = "tests/test-pdc.yml"
        ansible.inventory_path = "tests/inventory.yml"
        ansible.verbose = "-vvvv"
      end      
    end
  end

  inventory['all']['children']['domain_members']['hosts'].each do |server,details|
    config.vm.define server do |srv|
      srv.vm.box = details['vagrant_box']
      srv.vm.hostname = server
      srv.vm.network :private_network, ip: details['ansible_host']
      inventory['all']['vars']['vagrant_ports'].each do |protocol, details|
        srv.vm.network :forwarded_port, guest: details['guest'], host: details['host'] + current_host, id: protocol
      end
      srv.vm.provider :virtualbox do |v|
        v.name = File.basename(File.dirname(__FILE__)) + "_" + server + "_" + Time.now.to_i.to_s
        v.gui = false
        v.memory = 2048
        v.cpus = 2
      end
      srv.vm.provision "ansible" do |ansible|
        ansible.limit = "all"
        ansible.playbook = "tests/test-joinad.yml"
        ansible.inventory_path = "tests/inventory.yml"
        ansible.verbose = "-vvvv"
      end
      current_host = current_host + 1
    end  
  end
end
