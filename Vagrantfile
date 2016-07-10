# -*- mode: ruby -*-
# # vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"
  (1..3).each do |i|
    config.vm.define "one#{i}" do |node|
      config.hostmanager.enabled = true
      node.vm.hostname = "one#{i}"
      node.vm.network :private_network, ip: "192.168.125.#{i + 30}"
      (1..3).each do |n|
        node.vm.network :private_network,
                        :libvirt__dhcp_enabled => false,
                        :libvirt__forward_mode => 'none',
                        ip: "192.168.#{n + 125}.#{i + 30}"
        end
        node.vm.provider :libvirt do |domain|
          domain.memory = 3072
          domain.cpus = 2
          domain.nested = true
          domain.volume_cache = 'none'
 #         domain.machine_virtual_size = 10
          domain.storage :file, :size => '5G' #  play with gluster
          domain.storage :file, :size => '5G' #  play with gluster
      end

      node.vm.provision :ansible do |ansible|
         ansible.playbook = "site.yml"
      end

      node.vm.provision :ansible do |ansible|
         ansible.playbook = "site_post_tasks.yml"
      end
    end
  end
end
