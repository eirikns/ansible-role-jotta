# Ansible role for Jottacloud

[![CI](https://github.com/eirikns/ansible-role-jotta/actions/workflows/ci.yml/badge.svg)](https://github.com/eirikns/ansible-role-jotta/actions/workflows/ci.yml)

Ansible role to install and configure command line client for [Jottacloud](https://jottacloud.com/en/) cloud backup on
Debian and Ubuntu systems.

It is tested against latest Ubuntu LTS and Debian releases, but may work with other Debian-derived distributions.

The role:

- Adds the official Jotta APT repository and installs the `jotta-cli` package
- Manages the `jottad` user service as well as (optionally) managing lingering so `jottad` will run with an active user session.
- Supports managing this for a list of users.

It does not configure the client or support authenticating against Jottacloud. See https://docs.jottacloud.com/en/ for
information on how to use the `jotta-cli` command to do this.

## A note on lingering

[Lingering](https://www.freedesktop.org/software/systemd/man/latest/loginctl.html) is a systemd feature that allows user services to run without an active login session. It is not specific to Jottacloud — other user services on the same host may depend on it too.

If you already manage lingering as part of your user provisioning (e.g. via a dedicated user-management role), it is recommended to set `jottad_linger: false` so this role does not interfere. Letting a single role own lingering for a user avoids conflicts when the user is later removed or reconfigured.

## Role Variables

See `defaults/main.yaml` for a full list of values.

## Example Playbook

```yaml
- name: Install Jottacloud backup
  hosts: all
  become: true
  vars:
    jottad_users:
      - name: alice
        state: present
      - name: bob
        state: present
        linger: false   # disable lingering for bob only
      - name: john
        state: absent
  roles:
    - role: eirikns.jotta
```

# License

[MIT No Attribution](https://opensource.org/license/mit-0)

# Author

This Ansible role was created in 2025 by Eirik Nicolai Synnes and published as an Open Source project in 2026.

# Acknowledgements

This role uses Docker images created by Jeff Geerling for testing. The approach for developing this role, as well how to use GitHub Actions to manage it, is inspired by his work. You can find him on GitHub at https://github.com/geerlingguy.
