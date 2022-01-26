Custom APT repository
=========

With this role, it's possible to set up a repo proxy for Ubuntu and Debian (and every distribution which uses apt as package manager).

The use is intended for areas with weak Internet connections in order to save bandwidth.

Requirements
------------

You need to have a repository server already running. (SonaType Nexus or JFrog Artifactory or ...)

By default, the role expects the path for the proxied repository to have this format:
    deb *http://localhost/repository/ubuntu_focal_updates/* focal-updates main restricted universe multiverse

In this case "_repository/ubuntu_focal_updates/_" is the configured proxy path of the official Ubuntu focal updates repository.

If you have a different structure, then you need to define it with the variable "repository_paths". (see below)

Role Variables
--------------

| Parameter        | Default          | Description                         |
|------------------|------------------|-------------------------------------|
| repository_host  | http://localhost | DNS or IP of repository server      |
| upgrade_system   | no               | Do an upgrade after proxy is set up |
| update_cache     | no               | Update cache after proxy is set up  |
| repository_paths | []               | Definition of own repository config |

Config for _repository_paths_:

```yaml
repository_paths:
  - { dist: "focal", path: "repository/ubuntu_focal/", additional: "restricted universe multiverse" }
  - { dist: "focal-security", path: "repository/ubuntu_focal_security/", additional: "restricted universe multiverse" }
  - { dist: "focal-updates", path: "repository/ubuntu_focal_updates/", additional: "restricted universe multiverse" }
```

The information of the variable is used to configure the _deb_ line as follows:
    
    deb {{ repository_host }}/{{ path }} {{ dist }} main {{ additional }}

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
    - roles:
        - role: pmoscode.custom_apt_repository
          vars:
            repository_host: http://internal-proxy:8081
            upgrade_system: yes

License
-------

MIT
