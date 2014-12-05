## docker

[![Build Status](https://travis-ci.org/Oefenweb/ansible-docker.svg?branch=master)](https://travis-ci.org/Oefenweb/ansible-docker) [![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-docker-blue.svg)](https://galaxy.ansible.com/list#/roles/2309)

Set up the latest version of docker in Ubuntu systems.

#### Requirements

* `python-apt`

#### Variables

None

## Dependencies

None

#### Example

```yaml
---
- hosts: all
  roles:
  - docker
```

#### License

Apache v2.0

#### Author Information

Mischa ter Smitten (based on work of [angstwad](https://github.com/angstwad))

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Oefenweb/ansible-docker/issues)!
