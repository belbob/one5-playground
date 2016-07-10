# one5-playground
This is an Ansible project with a development environment powered by Vagrant.

This project sets up: 

* 3 virtual opennebula nodes with CentOS 7 and include :
* 4 virtual networks (1 x nat, 3 x isolated)
* 1 x opennebula-sunstone (web interface)
* 3 x opennebula-nodes
* 3 x disk per node, so you can play with e.g. glusterfs ,.. or add some disks with e.g. system-storage-manager 
* Bridging with OpenvSwitch configured for 4 nic's on every node

This project is intended as a demo/playground for a cloud-introductory , teaching git, vagrant and ansible, in de most simple way, and still useable in my classroom. Surely , this playbook can still be improved, but this way it is manageable in my classroom with my students.

## Dependencies

I use as host a Fedora 24 machine, make sure you have installed :

- vagrant
- vagrant-libvirt
- ansible (ver. 2+)
- git

And enabled nested KVM

```ShellSession
$ sudo modprobe -r kvm_intel
$ sudo modprobe kvm_intel nested=1
```
view state nested KVM

```ShellSession
$ cat /sys/module/kvm_intel/parameters/nested
```

## Getting started

When cloning, choose a more suiteable  name for the target directory!

```ShellSession
$ git clone https://github.com/belbob/one5-playground.git my-one-playground
```
After cloning, it's best to remove the `.git` directory and initialise a new repository. The history of the code is most probably irrelevant for your opennebula project...

### Installation opennebula-cluster

Open a terminal, go to a suitable directory to store this project and issue the following commands:

create the first opennebula-node, this node include also the opennebula-sunstone

```ShellSession
$ vagrant up one1
```

after finishing one1 you can start the other nodes

```ShellSession
$ vagrant up one2
```
and 
```ShellSession
$ vagrant up one3
```

### Accessing the virtual machine

* To start using the opennebula-sunstone site, go to <http://192.168.125.31:9869>
* To get access do use
    * Username: admin
    * Password: password

## some issues

### Howto change networkconfiguration:
used networks:
- vagrant-lbvirt: 192.168.121.0 - nat
- one0: 192.168.126.0 - isolated
- one1: 192.168.127.0 - isolated
- one2: 192.168.128.0 - isolated
- one3: 192.168.125.0 - nat

if this configuration is not suiteable for you, you may edit following files:

- Vagrantfile
- site_post_tasks.yml
- roles/nodes/files/export

### workaround systemd - vagrant - network.services issues

concerns: roles/ovs/tasks/main.yml

To setup the ovs-bridge I start network.service and stop them again, then I stop the NetworkManager.service. At this time the ovs bridging is fully operational.

You can check ovs configuration with de following command:

```ShellSession
$ sudo ovs-vsctl show 
```

### After a vagrant suspend or halt

You must check if /var/lib/one is mounted on node one2 and one3 and if not remount

```ShellSession
$ sudo mount /var/lib/one
```

if there is a strange behavior, you can try some things:

reload vagrant:

```ShellSession
$ vagrant reload one[x]
```

or do the provisioning again

```ShellSession
$ vagrant provision one[x]
```

or in the worst case destroy and rebuild the node

```ShellSession
$ vagrant destroy one[x]
$ vagrant up one[x]
```

and dont forget to clear your browser cache, or maybe beter, restart your browser

## Contributing

Issues, feature requests, ideas are appreciated and can be posted in the Issues section. Pull requests are also very welcome. Preferably, create a topic branch and when submitting, squash your commits into one (with a descriptive message).

## License

Licensed under the 2-clause "Simplified BSD License". See [LICENSE.md](/License.md) for details.

## Author Information

Written by Robert Keersse (robert.keersse@gmail.com)

Contributions by:

- 


