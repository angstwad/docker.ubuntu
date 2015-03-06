docker_ubuntu
========

Installs Docker on a version higher than Ubuntu 12.04.
This role differs from other roles in that it specifically follows docker.io installation instructions for each Ubuntu version, 12.04 or 13.04+.

**Note**: This role now defaults to installing the lxc-docker package, the latest package from the docker.io repository.  There have been recent changes to the "interface" of this role, so to speak, and the changes are breaking for those using in a parameterize

**Example Play**:
```
---
- name: Run docker.ubuntu
  hosts: docker
  roles:
    - docker.ubuntu
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
# lxc-docker is the default
docker_pkg_name: lxc-docker
# docker_pgk_name: docker.io
# Change these to 'present' if you're running Ubuntu 12.04-13.10 and are fine with less-than-latest packages
kernel_pkg_state: latest
cgroup_lite_pkg_state: latest
# Important if running Ubuntu 12.04-13.10 and ssh on a non-standard port
ssh_port: 22
# Place to get apt repository key
apt_key_url: http://get.docker.io/gpg
# apt repository key signature
apt_key_sig: A88D21E9
# Name of the apt repository for docker
apt_repository: deb http://get.docker.io/ubuntu docker main
# The following help expose a docker port or to add additional options when running docker
# The default is to not use any special options; see docker_opts
#docker_host_ip: 0.0.0.0
#docker_host_port: 2375
docker_opts: ""
#docker_opts: "-H unix:// -H tcp://{{ docker_host_ip }}:{{ docker_host_port }}"
#docker_version: 1.2.0
# Want to export the docker host?  You'll need to uncomment the docker_host_ip, port, opts
export_docker_host: false

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
