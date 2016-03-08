paxctld
=========

Installs and configures the [paxctld] service for managing PaX flags.
Intended for use with [grsecurity]-patched kernels.


Requirements
------------

A grsecurity-patched kernel. The `paxctld` service manages [PaX] flags
via extended file attributes, and so should not be used in conjunction with
the `paxctl` command.

Role Variables
--------------

```yaml
paxctld_download_directory: /usr/local/src
paxctld_version: "1.1-1"
paxctld_filename: "paxctld_{{ paxctld_version }}_amd64.deb"
paxctld_deb_url: "https://grsecurity.net/paxctld/{{ paxctld_filename }}"
paxctld_sig_url: "https://grsecurity.net/paxctld/{{ paxctld_filename }}.sig"
paxctld_gpg_keyserver: hkps.pool.sks-keyservers.net

# PaX flags. Override as necessary, but make sure to override the entire list,
# including items you want to preserve. In particular consider retaining the python
# config, as it's necessary for Ansible to run.
paxctld_configs:
  - binary: /usr/bin/python2.7
    flags: m
  - binary: /usr/lib/jvm/java-7-openjdk-amd64/jre/bin/java
    flags: m
  - binary: /usr/bin/nodejs
    flags: m
  - binary: /usr/bin/node
    flags: m
  - binary: /usr/sbin/asterisk
    flags: m
  - binary: /opt/kibana/node/bin/node
    flags: m
  - binary: /usr/bin/grub-script-check
    flags: pm
  - binary: /usr/bin/grub-bios-setup
    flags: pm
  - binary: /usr/sbin/grub-mkdevicemap
    flags: pm
  - binary: /usr/sbin/grub-probe
    flags: pm
```

Dependencies
------------

If you're not running a grsecurity-patched kernel yet, check out the [grsecurity Ansible role]
for building and installing.

Example Playbook
----------------

```yaml
- name: Configure grsecurity-patched server.
  hosts: server
  roles:
    - role: ansible-role-paxctld
      tags: paxctld
```

License
-------

MIT

Author Information
------------------

[Freedom of the Press Foundation]

[paxctld]: https://grsecurity.net/download.php
[grsecurity]: https://grsecurity.net/
[PaX]: https://en.wikipedia.org/wiki/Grsecurity#PaX
[grsecurity Ansible role]: https://github.com/freedomofpress/grsec
[Freedom of the Press Foundation]: https://freedom.press/
