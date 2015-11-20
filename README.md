Proxmox LXC template
====================

Proxmox LXC Templates using Debian Appliance Builder (DAB)

## Creates a Container Template

- Create Debian Templates: `squeeze`, `wheezy`, `jessie`
- Installs DAB
- Creates the template and makes it available to Proxmox
- Then you can create a container based on this template
- It should create Ubuntu Templates: `trusty`, `vivid` (but does not return from 'DAB Bootstrap' task)

### Debian Jessie minimal Template features

- based on Debootstrap minimal install
- adds Python for use with Ansible for example
- removes Postfix to make room for an alternate mail server
- temporarily permits root login with password for easy initialisation
  - intended to receive proper SSHD settings and keys via Ansible
- small 117 MB compressed template
- consumes about 250 MB on disk for a running container

Requirements
------------

Proxmox 4.0

Role Variables
--------------

#### variables for template creation

- `lxctemplate_make:` `true` (to create a template)
- `lxctemplate_keepcached:` `true` to keep the package cache directory

- `lxctemplate_distro: squeeze `debian-6.0`, wheezy `debian-7.0`, `jessie `debian-8.0`, `ubuntu-trusty`, `ubuntu-vivid`
- `lxctemplate_bootstrap_options:` use `--minimal` for the smallest possible image
- `lxctemplate_install:` additional packages to install in template (by default include `python` for ansible)
- `lxctemplate_remove:`  packages to remove (by default removes `postfix` to make room for an alternate mail server)

- `lxctemplate_builds:` directory where templates are built
- `lxctemplate_storage:` the new template will be moved here (default is local storage)
- `lxctemplate_dir:` name of the working directory that has the DAB template files


**This creates the dab.conf file (see `man dab`)**

- `lxctemplate_dab:`
  - `suite:` choice of `squeeze`|`wheezy`|`jessie`|`trusty`|`vivid`
  - `architecture:` `amd64` or `i386`
  - `name:` `minimal` or `standard`
  - `version:` a version number based on the OS (example for jessie `8.2-1`)
  - `maintainer:` name and email of maintainer
  - `infopage:`  link with more info about the template
  - `description_short:` example: `Debian Jessie 8.2 (minimal)`
  - `description_extended:` example: `A minimal Debian Jessie system including all required and important packages.`


#### variables for container creation

- `lxctemplate_container:` true (to create a container)
- `lxctemplate_vmid: ` `"1"` to `"255"` Container VM id
- `lxctemplate_hostname: ` hostname of the container
- `lxctemplate_password:` password of the container
- `lxctemplate_description:` the description of the container VM
- `lxctemplate_storeon:` where the container is stored (`local` is default; don't use `local` for ZFS)
- `lxctemplate_file:` filename of the container (Proxmox expects a specific file name format)

All defaults see defaults/main.yml

Example Playbook
----------------

```
    - hosts: servers
      vars_files:
      - vars/main.yml

      roles:
         - chriswayg.lxctemplate
```

__`vars/main.yml`__

```
## Example for Debian Squeeze:
lxctemplate_distro: debian-6.0
lxctemplate_dab:
  suite: squeeze
  architecture: amd64
  name: minimal
  version: 6.0-1
  maintainer: User <user@example.org>
  infopage: https://pve.proxmox.com/wiki/Debian_Appliance_Builder
  description_short: Debian Squeeze (minimal)
  description_extended: A minimal Debian Squeeze system including all required and important packages.

## Example for Debian Wheezy:
lxctemplate_distro: debian-7.0
lxctemplate_dab:
  suite: wheezy
  architecture: amd64
  name: minimal
  version: 7.0-1
  maintainer: User <user@example.org>
  infopage: https://pve.proxmox.com/wiki/Debian_Appliance_Builder
  description_short: Debian Wheezy (minimal)
  description_extended: A minimal Debian Wheezy system including all required and important packages.

## Example for Debian Jessie:
lxctemplate_distro: debian-8.0
lxctemplate_dab:
  suite: jessie
  architecture: amd64
  name: minimal
  version: 8.2-1
  maintainer: Christian Wagner <chriswayg@gmail.com>
  infopage: https://github.com/chriswayg/proxmox-templates/tree/master/debian-8.2-minimal-64
  description_short: Debian Jessie 8.2 (minimal)
  description_extended: A minimal Debian Jessie system including all required and important packages.

## Example for Ubuntu Trusty:
lxctemplate_distro: ubuntu-trusty
lxctemplate_dab:
  suite: trusty
  architecture: amd64
  name: minimal
  version: 14.04-1
  maintainer: User <user@example.org>
  infopage: https://pve.proxmox.com/wiki/Debian_Appliance_Builder
  description_short: Ubuntu Trusty (minimal)
  description_extended: A minimal Ubuntu Trusty system including all required and important packages.

## Example for Ubuntu Vivid:
lxctemplate_distro: ubuntu-vivid
lxctemplate_dab:
  suite: vivid
  architecture: amd64
  name: minimal
  version: 15.04-1
  maintainer: User <user@example.org>
  infopage: https://pve.proxmox.com/wiki/Debian_Appliance_Builder
  description_short: Ubuntu Vivid (minimal)
  description_extended: A minimal Ubuntu Vivid system including all required and important packages.
```

### References
- [Proxmox LXC templates](https://github.com/chriswayg/proxmox-templates)
- [Proxmox Linux Container](https://pve.proxmox.com/wiki/Linux_Container)
- [Proxmox Debian Appliance Builder](https://pve.proxmox.com/wiki/Debian_Appliance_Builder)
- [Proxmox Build your first DAB Appliance Template](https://pve.proxmox.com/wiki/Build_your_first_DAB_Appliance_Template)
- [Git Repo of Proxmox Templates](https://git.proxmox.com/?p=dab-pve-appliances.git;a=summary)

License
-------

GPLv2 or later
