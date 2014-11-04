docker_ubuntu
========

Installs Docker on a version higher than Ubuntu 12.04.
This role differs from other roles in that it specifically follows docker.io installation instructions for each Ubuntu version, 12.04 or 13.04+.

**Please see [this playbook](https://github.com/angstwad/ansible-docker-rackspace) as an example of how to utilize this role.**

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

These are the defaults, which can be set to present to prevent a reboot if the latest linux-image-extra, cgroup-lite packages are already installed
The following role variables are defined:

```
# Set to present to prevent a reboot if the latest linux-image-extra is already installed
kernel_pkg_state: latest
# Set to present to prevent a reboot if the latest cgroup-lite is already installed
cgroup_lite_pkg_state: latest
# Set to the default port that ssh is running on.  Only used if ansible_ssh_port is not defined.
ssh_port: 22
# URL from which to get apt key:
apt_key_url: http://get.docker.io/gpg
# apt key signatures
apt_key_sig: A88D21E9
# apt repository from which to get package
apt_repository: deb http://get.docker.io/ubuntu docker main
# Name of lxc-docker package to install
docker_lxc_pkg: lxc-docker
# Name of docker package to install
docker_io_pkg: docker.io

```

Dependencies
------------

None.

License
-------

Apache v2.0
