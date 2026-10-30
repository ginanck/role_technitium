<!-- DOCSIBLE START -->
# Ansible Role: role_technitium


Ansible role for installing and configuring Technitium DNS Server


## Table of Contents

- [Requirements](#requirements)
- [Dependencies](#dependencies)
- [Role Variables](#role-variables)
- [Task Overview](#task-overview)
- [Example Playbook](#example-playbook)
- [Documentation Maintenance](#documentation-maintenance)
- [License](#license)
- [Author Information](#author-information)

## Requirements



- Ansible >= 2.9


- Supported platforms:
  - Ubuntu (noble)
  - AlmaLinux (8.0)



## Dependencies


This role requires the following roles and collections:




  
    
  

  
    
  

  
    
  

  
    
  



**Roles:**

- [role_base](https://github.com/ginanck/role_base.git) (version: master)




**Collections:**

- `community.docker` (>= 4.8.1)

- `community.general` (>= 6.6.1)

- `ansible.posix` (>= 1.5.4)



To install all dependencies:
```bash
ansible-galaxy install -r meta/install_requirements.yml
```


## Role Variables

**These are static variables with lower priority**



#### File: defaults/main.yml

| Var | Type | Value |
|-----|------|-------|
| [technitium_admin_password](defaults/main.yml#L73) | str |  |
| [technitium_admin_user](defaults/main.yml#L71) | str | `admin` |
| [technitium_api_address](defaults/main.yml#L51) | str | `127.0.0.1` |
| [technitium_api_base_url](defaults/main.yml#L52) | str | `http://{{ technitium_api_address }}:{{ technitium_console_port }}` |
| [technitium_console_port](defaults/main.yml#L48) | int | `5380` |
| [technitium_default_admin_password](defaults/main.yml#L76) | str | `admin` |
| [technitium_disable_systemd_resolved_stub](defaults/main.yml#L56) | bool | `True` |
| [technitium_dns_checksum](defaults/main.yml#L15) | str |  |
| [technitium_dns_config](defaults/main.yml#L32) | str | `/etc/dns` |
| [technitium_dns_dir](defaults/main.yml#L33) | str | `/opt/technitium/dns` |
| [technitium_dns_port](defaults/main.yml#L47) | int | `53` |
| [technitium_dns_url](defaults/main.yml#L13) | str | `https://download.technitium.com/dns/DnsServerPortable.tar.gz` |
| [technitium_dotnet_dir](defaults/main.yml#L24) | str | `/opt/dotnet` |
| [technitium_dotnet_install_script](defaults/main.yml#L26) | str | `{{ technitium_dns_dir }}/dotnet-install.sh` |
| [technitium_dotnet_install_script_checksum](defaults/main.yml#L27) | str |  |
| [technitium_dotnet_runtime](defaults/main.yml#L23) | str | `Microsoft.AspNetCore.App 10.0.` |
| [technitium_dotnet_url](defaults/main.yml#L25) | str | `https://dot.net/v1/dotnet-install.sh` |
| [technitium_dotnet_version](defaults/main.yml#L22) | str | `10.0` |
| [technitium_firewall_manage](defaults/main.yml#L81) | bool | `True` |
| [technitium_firewall_ports](defaults/main.yml#L82) | list |  |
| [technitium_firewall_ports.0](defaults/main.yml#L83) | str | `53/tcp` |
| [technitium_firewall_ports.1](defaults/main.yml#L84) | str | `53/udp` |
| [technitium_nameservers](defaults/main.yml#L60) | list |  |
| [technitium_nameservers.0](defaults/main.yml#L61) | str | `127.0.0.1` |
| [technitium_resolv_conf_managed](defaults/main.yml#L59) | bool | `True` |
| [technitium_resolv_options](defaults/main.yml#L64) | list |  |
| [technitium_resolv_options.0](defaults/main.yml#L65) | str | `edns0` |
| [technitium_resolv_options.1](defaults/main.yml#L66) | str | `trust-ad` |
| [technitium_search_domains](defaults/main.yml#L62) | list |  |
| [technitium_search_domains.0](defaults/main.yml#L63) | str | `.` |
| [technitium_service_group](defaults/main.yml#L41) | str | `technitium` |
| [technitium_service_home](defaults/main.yml#L42) | str | `{{ technitium_dns_dir }}` |
| [technitium_service_name](defaults/main.yml#L39) | str | `technitium` |
| [technitium_service_user](defaults/main.yml#L40) | str | `technitium` |
| [technitium_version](defaults/main.yml#L12) | str | `15.6.2` |
| [technitium_version_marker](defaults/main.yml#L34) | str | `{{ technitium_dns_config }}/technitium.version` |
| [technitium_web_port](defaults/main.yml#L50) | str | `{{ technitium_console_port }}` |




## Task Overview


This role performs the following tasks:


### File: `tasks/admin.yml`

| Task Name | Module | Has Conditions | Line |
|-----------|--------|----------------|------|
| [Wait for the Technitium web console](tasks/admin.yml#L) | ansible.builtin.uri | No | N/A |
| [Check whether the vault admin password already authenticates](tasks/admin.yml#L) | ansible.builtin.uri | No | N/A |
| [Authenticate with the factory admin password](tasks/admin.yml#L) | ansible.builtin.uri | Yes | N/A |
| [Set the admin password from the vault](tasks/admin.yml#L) | ansible.builtin.uri | Yes | N/A |
| [Assert the admin password is the vault password](tasks/admin.yml#L) | ansible.builtin.assert | No | N/A |




### File: `tasks/preflight.yml`

| Task Name | Module | Has Conditions | Line |
|-----------|--------|----------------|------|
| [Assert the admin password is supplied by the vault](tasks/preflight.yml#L) | ansible.builtin.assert | No | N/A |
| [Assert the vault password is not the factory password](tasks/preflight.yml#L) | ansible.builtin.assert | No | N/A |
| [Assert the pinned runtime version matches the required runtime](tasks/preflight.yml#L) | ansible.builtin.assert | No | N/A |
| [Assert the DNS and console ports are valid and distinct](tasks/preflight.yml#L) | ansible.builtin.assert | No | N/A |




### File: `tasks/firewall.yml`

| Task Name | Module | Has Conditions | Line |
|-----------|--------|----------------|------|
| [Open the resolver ports in firewalld](tasks/firewall.yml#L) | ansible.posix.firewalld | Yes | N/A |
| [Allow the resolver ports through ufw](tasks/firewall.yml#L) | community.general.ufw | Yes | N/A |




### File: `tasks/runtime.yml`

| Task Name | Module | Has Conditions | Line |
|-----------|--------|----------------|------|
| [Check the installed .NET runtimes](tasks/runtime.yml#L) | ansible.builtin.command | No | N/A |
| [Download the .NET install script](tasks/runtime.yml#L) | ansible.builtin.get_url | Yes | N/A |
| [Install the ASP.NET Core runtime](tasks/runtime.yml#L) | ansible.builtin.command | Yes | N/A |
| [Link dotnet into the system path](tasks/runtime.yml#L) | ansible.builtin.file | Yes | N/A |
| [Read the installed .NET runtimes after installation](tasks/runtime.yml#L) | ansible.builtin.command | No | N/A |
| [Assert the required ASP.NET Core runtime is installed](tasks/runtime.yml#L) | ansible.builtin.assert | No | N/A |




### File: `tasks/port.yml`

| Task Name | Module | Has Conditions | Line |
|-----------|--------|----------------|------|
| [Record whether the Technitium service is running](tasks/port.yml#L) | ansible.builtin.set_fact | No | N/A |
| [Collect listening sockets](tasks/port.yml#L) | ansible.builtin.command | No | N/A |
| [Assert the DNS port is free for Technitium](tasks/port.yml#L) | ansible.builtin.assert | Yes | N/A |




### File: `tasks/systemd_resolved.yml`

| Task Name | Module | Has Conditions | Line |
|-----------|--------|----------------|------|
| [Check for the systemd-resolved unit](tasks/systemd_resolved.yml#L) | ansible.builtin.stat | No | N/A |
| [Record whether systemd-resolved is present](tasks/systemd_resolved.yml#L) | ansible.builtin.set_fact | No | N/A |
| [Create the systemd-resolved drop-in directory](tasks/systemd_resolved.yml#L) | ansible.builtin.file | Yes | N/A |
| [Disable the systemd-resolved stub listener on port 53](tasks/systemd_resolved.yml#L) | ansible.builtin.copy | Yes | N/A |




### File: `tasks/install.yml`

| Task Name | Module | Has Conditions | Line |
|-----------|--------|----------------|------|
| [Check the installed release version marker](tasks/install.yml#L) | ansible.builtin.stat | No | N/A |
| [Read the installed release version marker](tasks/install.yml#L) | ansible.builtin.slurp | Yes | N/A |
| [Record the installed release version](tasks/install.yml#L) | ansible.builtin.set_fact | No | N/A |
| [Stop the resolver before replacing its release](tasks/install.yml#L) | ansible.builtin.systemd | Yes | N/A |
| [Download the Technitium DNS Server release](tasks/install.yml#L) | ansible.builtin.get_url | Yes | N/A |
| [Extract the Technitium DNS Server release](tasks/install.yml#L) | ansible.builtin.unarchive | Yes | N/A |
| [Record the installed release version](tasks/install.yml#L) | ansible.builtin.copy | Yes | N/A |




### File: `tasks/resolv.yml`

| Task Name | Module | Has Conditions | Line |
|-----------|--------|----------------|------|
| [Check the resolver configuration path](tasks/resolv.yml#L) | ansible.builtin.stat | No | N/A |
| [Remove a symlinked /etc/resolv.conf](tasks/resolv.yml#L) | ansible.builtin.file | Yes | N/A |
| [Point the host resolver at the local Technitium instance](tasks/resolv.yml#L) | ansible.builtin.template | No | N/A |




### File: `tasks/main.yml`

| Task Name | Module | Has Conditions | Line |
|-----------|--------|----------------|------|
| [Check installed packages](tasks/main.yml#L) | ansible.builtin.package_facts | Yes | N/A |
| [Gather service facts](tasks/main.yml#L) | ansible.builtin.service_facts | No | N/A |
| [Run pre-flight checks](tasks/main.yml#L) | ansible.builtin.include_tasks | No | N/A |
| [Install OS family packages](tasks/main.yml#L) | ansible.builtin.include_tasks | No | N/A |
| [Create the Technitium service group](tasks/main.yml#L) | ansible.builtin.group | No | N/A |
| [Create the Technitium service account](tasks/main.yml#L) | ansible.builtin.user | No | N/A |
| [Create the Technitium configuration directory](tasks/main.yml#L) | ansible.builtin.file | No | N/A |
| [Create the Technitium installation directory](tasks/main.yml#L) | ansible.builtin.file | No | N/A |
| [Create the .NET installation directory](tasks/main.yml#L) | ansible.builtin.file | No | N/A |
| [Disable the systemd-resolved stub listener](tasks/main.yml#L) | ansible.builtin.include_tasks | Yes | N/A |
| [Guard the DNS port](tasks/main.yml#L) | ansible.builtin.include_tasks | No | N/A |
| [Install the required .NET runtime](tasks/main.yml#L) | ansible.builtin.include_tasks | No | N/A |
| [Install the pinned Technitium release](tasks/main.yml#L) | ansible.builtin.include_tasks | No | N/A |
| [Create the systemd unit for Technitium](tasks/main.yml#L) | ansible.builtin.template | No | N/A |
| [Enable and start the Technitium service](tasks/main.yml#L) | ansible.builtin.systemd | No | N/A |
| [Set the admin credentials from the vault](tasks/main.yml#L) | ansible.builtin.include_tasks | No | N/A |
| [Point the host resolver at Technitium](tasks/main.yml#L) | ansible.builtin.include_tasks | Yes | N/A |
| [Manage the firewall](tasks/main.yml#L) | ansible.builtin.include_tasks | Yes | N/A |




### File: `tasks/os_family/RedHat.yml`

| Task Name | Module | Has Conditions | Line |
|-----------|--------|----------------|------|
| [Install the .NET runtime dependencies](tasks/os_family/RedHat.yml#L) | ansible.builtin.dnf | No | N/A |




### File: `tasks/os_family/Debian.yml`

| Task Name | Module | Has Conditions | Line |
|-----------|--------|----------------|------|
| [Install the .NET runtime dependencies](tasks/os_family/Debian.yml#L) | ansible.builtin.apt | No | N/A |






## Example Playbook

```yaml
---
- hosts: all
  become: yes
  roles:
    - role: role_technitium

      vars:
        technitium_version: 15.6.2
        technitium_dns_url: https://download.technitium.com/dns/DnsServerPortable.tar.gz
        technitium_dns_checksum: 

```

## License


license (GPL-2.0-or-later, MIT, etc)


## Author Information


**Author:** Gorkem Korkmaz



**Company:** GK


**GitHub:** [gkorkmaz](https://github.com/gkorkmaz)

---
*This documentation was automatically generated using [docsible](https://github.com/zbohm/docsible).*
<!-- DOCSIBLE END -->
