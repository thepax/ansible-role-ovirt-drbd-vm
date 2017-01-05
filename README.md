ovirt-drbd-vm
=============

This role does the following things:
# Setups Pacemaker cluster
# Creates DRBD device
# Creates high available VM
# Installs latest CentOS 7 into VM

Note: There is no VNC/SPICE configured. VM console can be accessed via `virsh console {{ovirt_vm.name}}`

Status of running cluster: `pcs status`

Requirements
------------

Each host in cluster should have unique ovirt_cluster_nodeid defined.
Hosts with no ovirt_cluster_nodeid defined will not be included in Pacemaker cluster.
Two hosts in cluster should have ovirt_cluster_drbd_disk defined to local path to DRBD backing disk/partition.

Role Variables
--------------

    # Group name of oVirt hosts in Inventory
    ovirt_cluster_group: hosts

    # oVirt VM parameters
    ovirt_vm:
      name: oVirt
      uuid: 1b1d9643-6823-4ebc-9ac7-9dd540ced0fc
      cores: 4
      memory: 8192
      swap: 1024
      hostname: ovirt.example.com
      bridge: ovirtmgmt
      ipaddr: 192.168.1.10
      netmask: 255.255.255.0
      gateway: 192.168.1.1
      resolvers: 8.8.8.8,8.8.8.4
      mac: 52:54:00:d8:b0:a3
      password: "$1$5rCfy/eR$GtZpNpbFAzc5NiGfQ96mC."

Dependencies
------------

This role depends on the following roles:
* thepax.elrepo
* thepax.qemu-ev

Example Playbook
----------------

    - hosts: hosts
      roles:
        - thepax.ovirt-drbd-vm

Example Inventory
----------------

    [hosts]
    host1 ovirt_cluster_nodeid=1 ovirt_cluster_drbd_disk=/dev/disk/by-id/wwn-0x50014ee261603848-part3
    host2 ovirt_cluster_nodeid=2 ovirt_cluster_drbd_disk=/dev/disk/by-id/wwn-0x50014ee2b6b54be9-part3
    host3 ovirt_cluster_nodeid=3
    host4
    host5

    [hosts:vars]
    ovirt_vm:
      name: oVirt
      uuid: 1b1d9643-6823-4ebc-9ac7-9dd540ced0fc
      cores: 4
      memory: 8192
      swap: 1024
      hostname: ovirt.example.com
      bridge: ovirtmgmt
      ipaddr: 192.168.1.10
      netmask: 255.255.255.0
      gateway: 192.168.1.1
      resolvers: 8.8.8.8,8.8.8.4
      mac: 52:54:00:d8:b0:a3
      password: "$1$5rCfy/eR$GtZpNpbFAzc5NiGfQ96mC."

License
-------

GPLv3