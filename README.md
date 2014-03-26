docker_ubuntu
========

Installs Docker on a version higher than Ubuntu 12.04.
This role differs from other roles in that it specifically follows docker.io installation instructions for each Ubuntu version, 12.04 or 13.04+.

**Please see [this playbook](https://github.com/angstwad/ansible-docker-rackspace) for an example of how to utilize this role.**

This is an example playbook in which I call two playbooks to launch and display information about a Rackspace Cloud server, on top of which I use the *docker_ubuntu* role to install Docker.
```
# Install to an instance running Ubuntu 12.04
- include: rax-servers.yml
  vars:
    image: "ubuntu-1204-lts-precise-pangolin"

- name: Install pycurl
  hosts: docker
  gather_facts: no
  tasks:
    - name: Install pycurl
      apt: pkg=python-pycurl update_cache=yes cache_valid_time=600

- name: Install Docker on Rax Server
  hosts: docker
  roles:
    - angstwad.docker_ubuntu

- include: rax-servers-info.yml
```

Requirements
------------

Requires python-pycurl for apt modules.

Role Variables
--------------

These are the defaults, which can be set to present to prevent a reboot if the latest linux-image-extra, cgroup-lite packages are already installed

kernel_pkg_state: latest

cgroup_lite_pkg_state: latest


Dependencies
------------

None.

License
-------

Apache v2.0
