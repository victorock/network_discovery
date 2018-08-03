Ansible Role to Discover Network Devices
=========

Ansible role to Discover Network Devices. This role use [snmp_facts](https://docs.ansible.com/ansible/latest/modules/snmp_facts_module.html) to gather sysDescr ([RFC3418](https://tools.ietf.org/html/rfc3418)) or probe of <platform>\_facts modules. After discovery of the device, devices are [grouped by](https://docs.ansible.com/ansible/latest/modules/group_by_module.html) Ansible Network OS.

Contribute
--------------

If the platform you were looking for didn't get any match:
1. Fork this repository.
2. Update the file `vars/main.yaml` or `tasks/probe.yaml`.
3. Commit your change.
4. Send your Pull Request.

Role Variables
--------------

Variables are defined in `defaults/main.yml` and structured/encapsulated in `vars/main.yaml`.

| Name              | Default Value       | Description          |
|-------------------|---------------------|----------------------|
| `autorun` | `False`  | Boolean to define if the role "autorun" (`tasks/main.yml`). Useful when you want to have dependencies solved by galaxy (`meta/main.yml`) but don't want it to run automatically.  |
| `network_discovery` | `snmp` | Define the discovery method: `snmp` (snmp_facts) or `probe` (platform_facts try/error). |
| `network_discovery_snmp_version` | `v2c`  | SNMP Version to use, v2/v2c or v3. |
| `network_discovery_snmp_community` | `None` | SNMP Community (v2) |
| `network_discovery_snmp_level` | `None` | Authentication level, required if version is v3. |
| `network_discovery_snmp_integrity` | `None` | Hashing algorithm, required if version is v3. |
| `network_discovery_snmp_privacy` | `None` | Encryption algorithm, required if level is authPriv. |
| `network_discovery_snmp_username` | `None` | Username for SNMPv3, required if version is v3. |
| `network_discovery_snmp_authkey` | `None` | Authentication key, required if version is v3. |
| `network_discovery_snmp_privkey` | `None` | Encryption key, required if version is authPriv. |
| `network_discovery_probe_groups` | `[ 'eos', 'ios', 'iosxr', 'junos', 'linux', 'nxos' ]` | Probe Groups, meaning type of devices to discover. Supported: [ 'eos', 'ios', 'iosxr', 'junos', 'linux', 'nxos' ] |


> NOTE: For additional  [DETAILS](https://docs.ansible.com/ansible/latest/modules/snmp_facts_module.html).

Examples
------------

Follow below different examples and ways to use this role.

> 1. Discover the Network Operating System.

```YAML
---
- name: "Network Discovery: Probe"
  hosts: all
  connection: local
  gather_facts: false
  roles:
    - role: victorock.network_discovery
      network_discovery: "probe"
      autorun: true
```

> 2. Use Platform specific role to gather facts from Network Operating System.

```YAML
---
- name: "Gather Facts: NXOS"
# This group will be created by previous play who will discover the platforms.
  hosts: nxos
  connection: network_cli
  gather_facts: false
  roles:
    - role: ansible-network.cisco_nxos
      function: get_facts
```

> 3. Use Network Inventory role to create inventory Structure.

```YAML
---
- name: "Create Inventory"
  hosts: all
  connection: local
  gather_facts: false
  roles:
    - role: victorock.network_inventory
      autorun: true
```

Requirements
--------------

You need to install:
  - pysnmp

License
------------

GPLv3

Author
------------

Victor da Costa (@victorock)
