# Ansible role: `foundata.sshd.run`

The `foundata.sshd.run` Ansible role (part if the `foundata.sshd` Ansible collection). It provides automated configuration management of `sshd`, implementing security best practices by default across major platforms.



## Table of contents<a id="toc"></a>

- [Features](#features)<!-- BEGIN ANSIBLE DOCSMITH TOC --><!-- END ANSIBLE DOCSMITH TOC -->
- [Example playbooks, using this role](#examples)
- [Supported tags](#tags)
- [Dependencies](#dependencies)
- [Compatibility](#compatibility)
- [External requirements](#requirements)



## Features<a id="features"></a>

Main features:

* Sane defaults:
  * Modern cryptography.
  * Extended Logging.
  * Key-based authentication, no password based `root` login
  * Disabled Kerberos and GSSAPI (easy to re-enable but quite often not needed *by default*).
  * See `__run_sshd_sshd_settings_defaults` in  `./vars/main.yml` for a complete list.
* Default configuration result passes [`ssh-audit`](https://github.com/jtesta/ssh-audit) without errors or warnings.
* Simple to use: extend or adapt / overwrite the role's default configuration with a simple dictionary.


<!-- BEGIN ANSIBLE DOCSMITH MAIN -->
## Role variables<a id="variables"></a>

The following variables can be configured for this role:

| Variable | Type | Required | Default | Description |
|----------|------|----------|---------|-------------|
| `run_sshd_state` | `str` | Yes | `"present"` | Determines whether the managed resources should be `present` or `absent`.<br><br>`present` ensures that required components, such as software packages, are installed and configured.<br><br>`absent` reverts changes as much as possible, such as removing packages, deleting created users, stopping services, restoring modified settings, … |
| `run_sshd_autoupgrade` | `bool` | Yes | `false` | If set to true, all managed packages will be upgraded during each Ansible run (e.g., when the package provider detects a newer version than the currently installed one). |
| `run_sshd_service_state` | `str` | Yes | `"enabled"` | Defines the status of the service(s).<br><br>`enabled`: Service is running and will start automatically at boot.<br><br>`disabled`: Service is stopped and will not start automatically at boot.<br><br>`running` Service is running but will not start automatically at boot. This can be used to start a service on the first Ansible run without enabling it for boot.<br><br>`unmanaged`: Service will not start at boot, and Ansible will not manage its running state. This is primarily useful when services are monitored and managed by systems other than Ansible.<br><br>The singular form (`service`) is used for simplicity. However, the defined status applies to all services if multiple are being managed by this role. |
| `run_sshd_sshd_settings` | `dict` | Yes | N/A | sshd configuration values (additional ones or to overwrite defaults; see `__run_sshd_sshd_settings_defaults` in `vars/main.yml` for them).<br><br>Simply use standard SSH option names as keys with their corresponding values. Special cases:<br><br>- For boolean values, use true/false (these will be converted to yes/no by configuration tasks as needed).<br><br>- For options that can have multiple values, you can use a list: HostKey: - &quot;/etc/ssh/ssh_host_ed25519_key&quot; - &quot;/etc/ssh/ssh_host_ecdsa_key&quot; This will generate multiple entries in the config file, one per list item. |

### Variable details<a id="variable-details"></a>

#### `run_sshd_state`<a id="variable-run_sshd_state"></a>

Determines whether the managed resources should be `present` or `absent`.

`present` ensures that required components, such as software packages, are
installed and configured.

`absent` reverts changes as much as possible, such as removing packages,
deleting created users, stopping services, restoring modified settings, …

- **Type**: `str`
- **Required**: Yes
- **Default**: `"present"`
- **Choices**: `present`, `absent`


#### `run_sshd_autoupgrade`<a id="variable-run_sshd_autoupgrade"></a>

If set to true, all managed packages will be upgraded during each Ansible run (e.g., when the package provider detects a newer version than the currently installed one).

- **Type**: `bool`
- **Required**: Yes
- **Default**: `false`


#### `run_sshd_service_state`<a id="variable-run_sshd_service_state"></a>

Defines the status of the service(s).

`enabled`: Service is running and will start automatically at boot.

`disabled`: Service is stopped and will not start automatically at boot.

`running` Service is running but will not start automatically at boot.
This can be used to start a service on the first Ansible run without
enabling it for boot.

`unmanaged`: Service will not start at boot, and Ansible will not manage
its running state. This is primarily useful when services are monitored
and managed by systems other than Ansible.

The singular form (`service`) is used for simplicity. However, the defined
status applies to all services if multiple are being managed by this role.

- **Type**: `str`
- **Required**: Yes
- **Default**: `"enabled"`
- **Choices**: `enabled`, `disabled`, `running`, `unmanaged`


#### `run_sshd_sshd_settings`<a id="variable-run_sshd_sshd_settings"></a>

sshd configuration values (additional ones or to overwrite defaults; see
`__run_sshd_sshd_settings_defaults` in `vars/main.yml` for them).

Simply use standard SSH option names as keys with their corresponding values.
Special cases:

- For boolean values, use true/false (these will be converted to yes/no by
  configuration tasks as needed).

- For options that can have multiple values, you can use a list:
    HostKey:
      - "/etc/ssh/ssh_host_ed25519_key"
      - "/etc/ssh/ssh_host_ecdsa_key"
  This will generate multiple entries in the config file, one per list item.

- **Type**: `dict`
- **Required**: Yes



<!-- END ANSIBLE DOCSMITH MAIN -->


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

Installation with custom configuration options (e.g., `GatewayPorts: false`) and an override of the role's default setting for `GSSAPIAuthentication` and `Port`:

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
          Port: 2222
          GatewayPorts: false
          GSSAPIAuthentication: true
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

* **SELinux**: This role does not handle SELinux configurations. Please add additional tasks before or after this role to accommodate these changes (e.g. if you're changing SSH port on a system with SELinux enabled, allowing it via `ssh_port_t` is needed). The following Ansible modules may help with SELinux configuration:
  - [`ansible.posix.selinux`](https://docs.ansible.com/ansible/latest/collections/ansible/posix/selinux_module.html)
  - [`community.general.sefcontext_module`](https://docs.ansible.com/ansible/latest/collections/community/general/sefcontext_module.html)
  - [`community.general.seport_module`](https://docs.ansible.com/ansible/latest/collections/community/general/seport_module.html)
* **Firewall**: This role does not manage firewall configurations. Please add additional tasks before or after this role to configure firewall rules as needed to access your SSH service.

Beside that, there are no special requirements not covered by the role or Ansible itself.
