# -*- mode: ruby -*-
# vi: set ft=ruby :

IMAGE_NAME = "generic/debian9"
N = 4
HOSTNAMES = [
  "jupiter",
  "mars",
  "mercury",
  "neptune"
]

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  #config.vm.box = "generic/debian9"

  config.vm.synced_folder('.', '/vagrant', type: 'nfs', disabled: true)

  config.vm.define "earth" do |master|
    master.vm.box = IMAGE_NAME
    master.vm.provider :vmware_esxi do |esxi|
      esxi.esxi_hostname = '192.168.1.161'
      esxi.esxi_username = 'root'
      esxi.esxi_password = 'key:'
      esxi.esxi_virtual_network = [ 'VM LAN Network' ]
      esxi.guest_name = 'earth'
      esxi.guest_memsize = '2048'
      esxi.guest_numvcpus = '2'
      esxi.guest_disk_type = 'thick'
      esxi.guest_storage = [ 1024 ]
      esxi.guest_guestos = 'debian9-64'
      esxi.local_allow_overwrite = 'True'
      esxi.debug = 'true'
    end
    master.vm.hostname = "earth"
    master.vm.provision "shell",
      inline: "sudo hostnamectl set-hostname earth"
    master.vm.provision :reload
    master.vm.provision "ansible" do |ansible|
      ansible.playbook = "kube-dependencies.yml"
      ansible.extra_vars = {
        node_ip: "192.168.10.10",
      }
    end
    master.vm.provision "ansible" do |ansible|
      ansible.playbook = "kube-master.yml"
      ansible.extra_vars = {
          node_ip: "192.168.10.10",
      }
    end
  end

  (1..N).each do |i|
    config.vm.define "#{HOSTNAMES[i-1]}" do |node|
      node.vm.box = IMAGE_NAME
      node.vm.provider :vmware_esxi do |esxi|
        esxi.esxi_hostname = '192.168.1.161'
        esxi.esxi_username = 'root'
        esxi.esxi_password = 'TuxESXi001'
        esxi.esxi_virtual_network = [ 'VM LAN Network' ]
        esxi.guest_name = "#{HOSTNAMES[i-1]}"
        esxi.guest_memsize = '2048'
        esxi.guest_numvcpus = '2'
        esxi.guest_disk_type = 'thick'
        esxi.guest_storage = [ 1536 ]
        esxi.guest_guestos = 'debian9-64'
        esxi.local_allow_overwrite = 'True'
        esxi.debug = 'true'
      end
      #node.vm.hostname = "#{HOSTNAMES[i-1]}"
      node.vm.provision "shell",
        inline: "sudo hostnamectl set-hostname #{HOSTNAMES[i-1]}"
      node.vm.provision :reload
      node.vm.provision "ansible" do |ansible|
        ansible.playbook = "kube-dependencies.yml"
        ansible.extra_vars = {
          node_ip: "192.168.10.#{i + 10}",
        }
      end
      node.vm.provision "ansible" do |ansible|
        ansible.limit = "all"
        ansible.playbook = "kube-worker.yml"
        ansible.extra_vars = {
          node_ip: "192.168.10.#{i + 10}",
        }
      end
    end
  end
end