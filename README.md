### What for this?
After some research i didnt found actually working guide to install kubernetes cluster, only guides with some working parts, so i decided to mix working parts from those playbooks into my own

### Ansible playbook to install kubernetes cluster with vagrant

This playbook will install kubernetes cluster in vagrant with one master and two nodes.

Warning! Nodes will not work after reboot, because they need static route to kubernetes cluster ip via master node(which this playbook adds temporary in kubernetes-node role).

### If you launch vagrant in bash for windows ( WSL )

If you launch vagrant in bash for windows, you should disable uartmode in Vagrant settings:

``` vb.customize ["modifyvm", :id, "--memory", "2048", "--uartmode1", "disconnected" ]```

### Tested on
Vagrant 1.9.3

Virtualbox 5.0.38
