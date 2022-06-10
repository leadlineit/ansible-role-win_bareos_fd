# Ansible Galaxy role for install and configure Bareos File Daemon on Windows OS.

[![Galaxy Role](https://img.shields.io/badge/Ansible--Galaxy-leadlineit.win_bareos_fd-blue.svg?logo=ansible&logoColor=white)](https://galaxy.ansible.com/leadlineit/win_bareos_fd/)

This role helps to install and configure Bareos File Daemon on Windows OS(Server2016/2019/2022 or 8/10).

Requirements
------------

This role requires Ansible 2.9 or higher.

Role Variables
--------------

The variables that can be passed to this role and a brief description about them are as follows:

```yaml
---
bareos_release: 21
bareos_tls_path_win: C:\ProgramData\Bareos\tls
bareos_tls_certs: your.bareos.dir.com
bareos_tls_path: /etc/bareos/tls
bareos_server: you.bareos.dir.server

bareos_fd:
  director:
    - name: your-dir
      description: Director, who is permitted to contact this file daemon.
      password: DIRAver@gEStr0ngPaSSw0rd
      tls_enable: "yes"
    - name: your-mon
      description: Restricted Director monitor description
      password: MONAver@gEStr0ngPaSSw0rd
      monitor: "yes"
      tls_enable: "yes"
  client:
    - name: your-client
      description: Your Bareos client
      fdport: 9102
      tls_enable: "yes"
  messages:
    - name: your-messages
      description: Messages description
      server: your-dir
```

The variables above are optional. They don't have a default value, so if you don't define them - tasks using them will be skipped. 
You can set only some of them, or not set at all (in this case, you will simply install Bareos Storage with default configuration).
Also, you can use HashiCorp Vault for store client certificates (when you use Bareos with TLS)
Variable for this (optional too):

```yaml
---
  hashicorp_vault:
    address: your.vault.com
    token: your_token
    path: your-path-to-certs
    clients:
      - name: host1.client1
        client: client1
        ttl: 24h
      - name: host2.client1
        client: client1
        ttl: 18h
      - name: host01.client2
        client: client2
        ttl: 12h
      - name: host02.client2
        client: client2
        ttl: 96h
```

Another thing you can do is remotely add client configuration to your main Bareos Director server.
Variable for this (optional too):

```yaml
---
bareos_server: you.bareos.dir.server

bareos_dir:
  client:
    - name: your-client
      description: Your client configuration
      address: 10.0.0.1
      fdport: 9102
      passive: "yes"
      tls_enable: "yes"
      jobs:
        - name: client-job1
          description: Job1 for client
          client: client.name.com
          jobdef: your-jobdefs1
        - name: client-job2
          description: Job2 for client
          client: client.name.com
          jobdef: your-jobdefs2
```

Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
- hosts: servers
  roles:
    - { role: leadlineit.win_bareos_fd, tags: win_bareos_fd }
```

License
-------

MIT

Author Information
------------------

This role was created by Artem Kasianchuk.
