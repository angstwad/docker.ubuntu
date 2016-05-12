docker_ubuntu
========

[![Build Status](https://travis-ci.org/angstwad/docker.ubuntu.svg)](https://travis-ci.org/angstwad/docker.ubuntu)

Installs Docker on a version higher than Ubuntu 12.04.
This role differs from other roles in that it specifically follows docker.io installation instructions for each Ubuntu version, 12.04 or 13.04+.

**Note**: This role now defaults to installing the lxc-docker package, the latest package from the docker.io repository.  There have been recent changes to the "interface" of this role, so to speak, and the changes are breaking for those using this as a parameterized role.

**Example Play**:
```
---
- name: Run docker.ubuntu
  hosts: docker
  roles:
    - angstwad.docker_ubuntu
```

**Please see [this playbook](https://github.com/angstwad/ansible-docker-rackspace) as a more advanced example of how to utilize this role.**

Applying the role to servers is pretty simple:
```
- name: Install Docker on Rax Server
  hosts: all
  roles:
    - angstwad.docker_ubuntu
```

Overriding the role's default variables is also pretty straightforward:
```
- name: Install Docker on Rax Server
  hosts: all
  roles:
    - role: angstwad.docker_ubuntu
      ssh_port: 2222
      kernel_pkg_state: present
```


Requirements
------------

Requires python-pycurl for apt modules.

Role Variables
--------------

These are the defaults, which can be set to present to prevent a reboot if the latest linux-image-extra, cgroup-lite packages are already installed.
The following role variables are defined:

```
---
# docker-engine is the default package name
docker_pkg_name: docker-engine
docker_apt_cache_valid_time: 600

# docker dns path for docker.io package ( changed at ubuntu 14.04 from docker to docker.io )
docker_defaults_file_path: /etc/default/docker

# Important if running Ubuntu 12.04-13.10 and ssh on a non-standard port
ssh_port: 22
# Place to get apt repository key
apt_key_url: https://apt.dockerproject.org/gpg
# apt repository key signature
apt_key_sig: 2C52609D
# Name of the apt repository for docker
apt_repository: deb https://apt.dockerproject.org/repo ubuntu-{{ ansible_distribution_release }} main
# The following help expose a docker port or to add additional options when
# running docker daemon.  The default is to not use any special options.
#docker_opts: >
#  -H unix://
#  -H tcp://0.0.0.0:2375
#  --log-level=debug
docker_opts: ""
# List of users to be added to 'docker' system group (disabled by default)
# SECURITY WARNING: 
# Be aware that granted users can easily get full root access on the docker host system!
docker_group_members: []
# Flags for whether to install pip packages
pip_install_pip: true
pip_install_setuptools: true
pip_install_docker_py: true
pip_install_docker_compose: true
# Versions for the python packages that are installed installed
pip_version_pip: latest
pip_version_setuptools: latest
pip_version_docker_py: latest
pip_version_docker_compose: latest

# If this variable is set to true kernel updates and host restarts are permitted.
# Warning: Use with caution in production environments.
kernel_update_and_reboot_permitted: no

# Set to 'yes' or 'true' to enable updates (sets 'latest' in apt module)
update_docker_package: no
# Change these to 'present' if you're running Ubuntu 12.04-13.10 and are fine with less-than-latest packages
kernel_pkg_state: latest
cgroup_lite_pkg_state: latest
# Force an install of the kernel extras, in case you're suffering from some issue related to the
# static binary provided by upstream Docker.  For example, see this GitHub Issue in Docker:
# https://github.com/docker/docker/issues/12750
# Warning: Installing kernel extras is potentially interruptive/destructive and will install backported
# kernel if running 12.04.
install_kernel_extras: false
# Install Xorg packages for backported kernels.  This is usually unnecessary except for environments
# where an X/Unit desktop is actively being used. If you're not using an X/Unity on 12.04, you
# won't need to enable this.
install_xorg_pkgs: false

```

Dependencies
------------

None.

Testing
-------

To test the role in a Vagrant environment just run `vagrant up`.  This will
create two VMs, one based on Ubuntu 12.04 and second based on Ubuntu 14.04,
and it will provision them by applying this role with Ansible.

Requires `ansible-playbook` to be in the path.

License
-------

Apache v2.0
