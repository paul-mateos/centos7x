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

### Validate the Template
```bash
packer validate centos-7.3-x86_64.json 
Template validated successfully.
```

### Inspect the Template
```bash
packer inspect centos-7.3-x86_64.json 
Optional variables and their defaults:

  box_basename      = centos-7.3
  box_dir           = {{env `VAGRANT_BOXES`}}
  box_os            = rh
  build_dir         = {{env `PACKER_BUILD_DIR`}}
  build_timestamp   = {{isotime "20060102150405"}}
  cpus              = 1
  disk_size         = 40960
  git_revision      = __unknown_git_revision__
  headless          = 
  http_proxy        = {{env `http_proxy`}}
  https_proxy       = {{env `https_proxy`}}
  iso_checksum      = c455ee948e872ad2194bdddd39045b83634e8613249182b88f549bb2319d97eb
  iso_checksum_type = sha256
  iso_name          = CentOS-7-x86_64-DVD-1611.iso
  ks_path           = rhel-7/ks.cfg
  memory            = 1024
  metadata          = floppy/dummy_metadata.json
  mirror            = http://mirrors.kernel.org/centos
  mirror_directory  = 7.3.1611/isos/x86_64
  name              = base-centos
  no_proxy          = {{env `no_proxy`}}
  template          = centos-7.3-x86_64
  version           = TIMESTAMP

Builders:

  virtualbox-iso
  vmware-iso    

Provisioners:

  file
  shell
```

All of this looks good, for now. This stuff will change over the next few months.

### Build from the Template
This Packer template will build in both VirtualBox and VMware. If one isn't specified then both will build in parallel. For now, we're really only interested in the VMware build. To kick this off:

```bash
packer build -only=vmware-iso centos-7.3-x86_64.json
```

## Outputs
The Packer build output directory:

```bash
tree ~/vms/packer/builds/
~/vms/packer/builds/
└── rh
    ├── 564da05d-50ba-9eab-9462-090c8139dd45.vmem
    ├── 564da05d-50ba-9eab-9462-090c8139dd45.vmem.lck
    │   └── M19014.lck
    ├── centos-7.3-x86_64.nvram
    ├── centos-7.3-x86_64.plist
    ├── centos-7.3-x86_64.vmsd
    ├── centos-7.3-x86_64.vmx
    ├── centos-7.3-x86_64.vmx.lck
    │   └── M18280.lck
    ├── centos-7.3-x86_64.vmxf
    ├── disk-s001.vmdk
    ├── disk-s002.vmdk
    ├── disk-s003.vmdk
    ├── disk-s004.vmdk
    ├── disk-s005.vmdk
    ├── disk-s006.vmdk
    ├── disk-s007.vmdk
    ├── disk-s008.vmdk
    ├── disk-s009.vmdk
    ├── disk-s010.vmdk
    ├── disk-s011.vmdk
    ├── disk.vmdk
    ├── disk.vmdk.lck
    │   └── M21475.lck
    ├── startMenu.plist
    └── vmware.log

```
This gets cleaned up after each build.

The Vagrant Box output directory:

```bash
tree ~/vms/vagrant/boxes/rh/
~/vms/vagrant/boxes/rh/
└── centos-7.3-vmware.box

```

This is the baseline.

[chef/bento]:https://github.com/chef/bento
[Ansible]:https://github.com/todd-dsm/ansible-scratch