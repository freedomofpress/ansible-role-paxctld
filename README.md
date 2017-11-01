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
paxctld_version: "1.2.2-1"
paxctld_filename: "paxctld_{{ paxctld_version }}_amd64.deb"
paxctld_deb_url: "https://grsecurity.net/paxctld/{{ paxctld_filename }}"
paxctld_sig_url: "https://grsecurity.net/paxctld/{{ paxctld_filename }}.sig"

# Custom PaX flags to set. Format is dict with keys as binary path and value
# the flags to be set. Flags should be a single string. This var is intended
# to be overridden to provide site-specific PaX flags; the default flags are
# provided via the `paxctld_configs_dist` var below.
paxctld_configs: []

# PaX flags provided by the default paxctld config. These are shipped with
# the deb package, and will be preserved in the config file generated
# by this role. Set to an empty dict to override preserving these flags.
paxctld_configs_dist:
  /usr/bin/grub-script-check: E
  /usr/bin/grub-bios-setup: E
  /usr/sbin/grub-mkdevicemap: E
  /usr/sbin/grub-probe: E
  /usr/bin/qemu-alpha: m
  /usr/bin/qemu-arm: m
  /usr/bin/qemu-armeb: m
  /usr/bin/qemu-cris: m
  /usr/bin/qemu-i386: m
  /usr/bin/qemu-m68k: m
  /usr/bin/qemu-microblaze: m
  /usr/bin/qemu-microblazeel: m
  /usr/bin/qemu-mips: m
  /usr/bin/qemu-mips64: m
  /usr/bin/qemu-mips64el: m
  /usr/bin/qemu-mipsel: m
  /usr/bin/qemu-mipsn32: m
  /usr/bin/qemu-mipsn32el: m
  /usr/bin/qemu-or32: m
  /usr/bin/qemu-ppc: m
  /usr/bin/qemu-ppc64: m
  /usr/bin/qemu-ppc64abi32: m
  /usr/bin/qemu-s390x: m
  /usr/bin/qemu-sh4: m
  /usr/bin/qemu-sh4eb: m
  /usr/bin/qemu-sparc: m
  /usr/bin/qemu-sparc32plus: m
  /usr/bin/qemu-sparc64: m
  /usr/bin/qemu-unicore32: m
  /usr/bin/qemu-x86_64: m
  /usr/bin/qemu-system-aarch64: m
  /usr/bin/qemu-system-alpha: m
  /usr/bin/qemu-system-arm: m
  /usr/bin/qemu-system-cris: m
  /usr/bin/qemu-system-i386: m
  /usr/bin/qemu-system-lm32: m
  /usr/bin/qemu-system-m68k: m
  /usr/bin/qemu-system-microblaze: m
  /usr/bin/qemu-system-microblazeel: m
  /usr/bin/qemu-system-mips: m
  /usr/bin/qemu-system-mips64: m
  /usr/bin/qemu-system-mips64el: m
  /usr/bin/qemu-system-mipsel: m
  /usr/bin/qemu-system-moxie: m
  /usr/bin/qemu-system-or32: m
  /usr/bin/qemu-system-ppc: m
  /usr/bin/qemu-system-ppc64: m
  /usr/bin/qemu-system-ppcemb: m
  /usr/bin/qemu-system-s390x: m
  /usr/bin/qemu-system-sh4: m
  /usr/bin/qemu-system-sh4eb: m
  /usr/bin/qemu-system-sparc: m
  /usr/bin/qemu-system-sparc64: m
  /usr/bin/qemu-system-unicore32: m
  /usr/bin/qemu-system-x86_64: m
  /usr/bin/qemu-system-xtensa: m
  /usr/bin/qemu-system-xtensaeb: m
  /usr/lib/skype/skype: m
  /usr/lib32/skype/skype: m
  /usr/lib32/ld-linux.so.2: m
  /usr/bin/node: m
  /opt/google/chrome/chrome-sandbox: m
  /opt/google/chrome/nacl_helper: m
  /opt/google/chrome/chrome: m
  /usr/lib/chromium-browser/chromium-browser: m
  /usr/lib/firefox/firefox: m
  /usr/lib/firefox/plugin-container: m
  /usr/bin/webapp-container: m
  /usr/lib/x86_64-linux-gnu/oxide-qt/oxide-renderer: m
  /usr/bin/valgrind: m
  /usr/bin/python2.7: E
  /usr/bin/python3.5: E
  /usr/lib/jvm/java-6-sun-1.6.0.10/jre/bin/java: m
  /usr/lib/jvm/java-6-sun-1.6.0.10/jre/bin/javaws: m
  /usr/lib/jvm/java-6-openjdk/jre/bin/java: m
  /usr/lib/jvm/java-8-openjdk/jre/bin/java: m
  /lib/rc/bin/lsb2rcconf: E

# ASCII-armored GPG public key for Bradley Spengler (spender), grsecurity maintainer.
# Used for verifying package integrity on downloaded deb package.
paxctld_gpg_pubkey_content: "{{ lookup('file', 'spender-gpg-key.asc') }}"
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
