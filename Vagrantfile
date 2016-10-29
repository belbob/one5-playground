# -*- mode: ruby -*-
# # vi: set ft=ruby :

VAGRANT_ONE_VER = "5.2"
VAGRANT_SELINUX = "permissive"

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
          (1..3).each do |disks|
            domain.storage :file, :size => '5G' #  play with gluster, raid,..
          end
      end

      node.vm.provision :ansible do |ansible|
         ansible.extra_vars = {
            one_ver: VAGRANT_ONE_VER,
            selinux_state: VAGRANT_SELINUX
         }
         ansible.playbook = "site.yml"
      end

      node.vm.provision :ansible do |ansible|
         ansible.playbook = "site_post_tasks.yml"
      end
    end
  end
end
