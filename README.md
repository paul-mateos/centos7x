# centos7x
Packer build of CentOS/RHEL OS on VMware

## Requirements
* A system with Packer installed. macOS in this case.
* A _local_ hypervisor: VMware Fusion

## Design
1. To move quickly some community work will be leveraged. There is simply too much of it to rebuild and the quality is, if not the highest, very stable.
2. To that end, we'll begin with the [chef/bento] packer code.
3. Then add some tailored OS configuration with [Ansible]. This repo is currently under heavy construction and thus, embarrassing. To be available soon.

This will serve as a starting point. 

[chef/bento]:https://github.com/chef/bento
[Ansible]:https://github.com/todd-dsm/ansible-scratch