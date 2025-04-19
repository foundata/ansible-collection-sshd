# Ansible role: `foundata.sshd.run`

The `foundata.sshd.run` Ansible role (part if the `foundata.sshd` Ansible collection). It provides automated configuration management of `sshd`, implementing security best practices by default across major platforms.



## Table of contents<a id="toc"></a>

- [Features](#features)
- [Role variables](#variables)
- [Example playbooks, using this role](#examples)
- [Supported tags](#tags)
- [Dependencies](#dependencies)
- [Compatibility](#compatibility)
- [External requirements](#requirements)



## Features<a id="features"></a>

Main features:

* Sane defaults:
  * Key-based authentication, no password based `root` login
  * Modern cryptography
  * Extended Logging
  * Disabled Kerberos and GSSAPI (easy to re-enable but quite often not needed *by default*).
  * See `__run_sshd_sshd_settings_defaults` in  `./vars/main.yml` for a complete list
* Default configuration result passes [`ssh-audit`](https://github.com/jtesta/ssh-audit) without errors or warnings.
* Simple to use: extend or adapt / overwrite the role's default configuration with a simple dictionary.



## Role variables<a id="variables"></a>

See [`defaults/main.yml`](./defaults/main.yml) for all available role parameters and their description. [`vars/main.yml`](./vars/main.yml) contains internal variables you should not override (but their description might be interesting).

Additionally, there are variables read from other roles and/or the global scope (for example, host or group vars) as follows:

- None right now.



## Example playbooks, using this role<a id="examples"></a>

Installation with automatic upgrade:

```yaml
---

- name: "Initialize the foundata.sshd.run role"
  hosts: localhost
  gather_facts: false
  tasks:

    - name: "Trigger invocation of the foundata.sshd.run role"
      ansible.builtin.include_role:
        name: "foundata.sshd.run"
      vars:
        run_sshd_autoupgrade: true
```

Installation with custom configuration options (e.g., `GatewayPorts: false`) and an override of the role's default setting for `GSSAPIAuthentication`:

```yaml
---

- name: "Initialize the foundata.sshd.run role"
  hosts: localhost
  gather_facts: false
  tasks:

    - name: "Trigger invocation of the foundata.sshd.run role"
      ansible.builtin.include_role:
        name: "foundata.sshd.run"
      vars:
        run_sshd_autoupgrade: true
        run_sshd_sshd_settings:
          GSSAPIAuthentication: true
          GatewayPorts: false

```

Uninstall (⚠️ Warning: This will remove SSH access from the target machines!)

```yaml
---

- name: "Initialize the foundata.sshd.run role"
  hosts: localhost
  gather_facts: false
  tasks:

    - name: "Trigger invocation of the foundata.sshd.run role"
      ansible.builtin.include_role:
        name: "foundata.sshd.run"
      vars:
        run_sshd_state: "absent"
```



## Supported tags<a id="tags"></a>

It might be useful and faster to only call parts of the role by using tags:

- `run_sshd_setup`: Manage basic resources, such as packages or service users.
- `run_sshd_config`: Manage settings, such as adapting or creating configuration files.
- `run_sshd_service`: Manage services and daemons, such as running states and service boot configurations.

There are also tags usually not meant to be called directly but listed for the sake of completeness** and edge cases:

- `run_sshd_always`, `always`: Tasks needed by the role itself for internal role setup and the Ansible environment.



## Dependencies<a id="dependencies"></a>

See `dependencies` in [`meta/main.yml`](./meta/main.yml).



## Compatibility<a id="compatibility"></a>

See `min_ansible_version` in [`meta/main.yml`](./meta/main.yml) and `__run_sshd_supported_platforms` in [`vars/main.yml`](./vars/main.yml).



## External requirements<a id="requirements"></a>

There are no special requirements not covered by Ansible itself.
