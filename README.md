pmm-client
==========

This role installs and configures the Percona PMM Client.

Requirements
------------

This role requires Ansible 2.2 or higher and platform requirements are listed
in the metadata file.

Install
-------

```sh
ansible-galaxy install timorunge.pmm-client
```

Role Variables
--------------

The variables that can be passed to this role and a brief description about
them are as follows. (For all variables, take a look at [defaults/main.yml](defaults/main.yml))

```yaml
# IP address and port of the pmm-server:
pmm_client_server_host: 172.20.0.10
pmm_client_server_port: 443
# Disable basic auth:
pmm_client_server_basic_auth: false
# Enable SSL:
pmm_client_server_use_ssl: true
# Define services to be added or removed:
pmm_client_add_services:
  - linux:metrics
  - mysql:metrics
  - mongodb:metrics
pmm_client_remove_services:
  - mysql:queries
# Define services to be started or stopped:
pmm_client_start_services:
  - linux:metrics
  - mysql:metrics
pmm_client_stop_services:
  - mongodb:metrics
# Define credentials for the MySQL DB connection:
pmm_client_db:
  mysql:
    host: localhost
    port: 3306
    username: root
    password: toor
```

Examples
--------

## 1) Install the PMM Client with no basic auth and disabled SSL

```yaml
- hosts: all
  become: yes
  vars:
    pmm_client_server_host: 172.20.0.10
    pmm_client_server_port: 443
    pmm_client_server_basic_auth: false
    pmm_client_server_use_ssl: false
    pmm_client_add_services:
      - linux:metrics
    pmm_client_start_services:
      - linux:metrics
  roles:
    - timorunge.pmm-client
```

## 2) Install the PMM Client with basic auth and enabled SSL

```yaml
- hosts: all
  become: yes
  vars:
    pmm_client_server_host: 172.20.0.10
    pmm_client_server_port: 443
    pmm_client_server_basic_auth: true
    pmm_client_server_basic_auth_username: admin
    pmm_client_server_basic_auth_password: mySecurePassword
    pmm_client_server_use_ssl: true
    pmm_client_add_services:
      - linux:metrics
    pmm_client_start_services:
      - linux:metrics
  roles:
    - timorunge.pmm-client
```

## 3) Install the PMM Client from a defined URL

```yaml
- hosts: all
  become: yes
  vars:
    pmm_client_use_official_repo: false
    pmm_client_debian_pkg: "https://www.percona.com/downloads/pmm/1.8.1/binary/debian/xenial/x86_64/pmm-client_1.8.1-1.{{ ansible_distribution_release }}_amd64.deb"
  roles:
    - timorunge.pmm-client
```

## 4) Uninstall the PMM Client

```yaml
- hosts: all
  become: yes
  vars:
    pmm_client_enabled: false
  roles:
    - timorunge.pmm-client
```

Dependencies
------------

None

License
-------
BSD

Author Information
------------------

- Based on the Ansible role from [Chris Sam](https://github.com/chrissam/ansible-role-pmm-client)
- Heavily modified by: Timo Runge
