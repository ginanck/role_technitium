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
| [technitium_dns_config](defaults/main.yml#L11) | str | `/etc/dns` |
| [technitium_dns_dir](defaults/main.yml#L12) | str | `/opt/technitium/dns` |
| [technitium_dns_url](defaults/main.yml#L13) | str | `https://download.technitium.com/dns/DnsServerPortable.tar.gz` |
| [technitium_dotnet_dir](defaults/main.yml#L6) | str | `/opt/dotnet` |
| [technitium_dotnet_runtime](defaults/main.yml#L8) | str | `Microsoft.AspNetCore.App 9.0.` |
| [technitium_dotnet_url](defaults/main.yml#L9) | str | `https://dot.net/v1/dotnet-install.sh` |
| [technitium_dotnet_version](defaults/main.yml#L7) | str | `9.0` |
| [technitium_install_log](defaults/main.yml#L15) | str | `{{ technitium_dns_dir }}/install.log` |
| [technitium_nameservers](defaults/main.yml#L19) | list |  |
| [technitium_nameservers.0](defaults/main.yml#L20) | str | `127.0.0.53` |
| [technitium_resolv_options](defaults/main.yml#L25) | list |  |
| [technitium_resolv_options.0](defaults/main.yml#L26) | str | `edns0` |
| [technitium_resolv_options.1](defaults/main.yml#L27) | str | `trust-ad` |
| [technitium_search_domains](defaults/main.yml#L22) | list |  |
| [technitium_search_domains.0](defaults/main.yml#L23) | str | `.` |
| [technitium_service_name](defaults/main.yml#L16) | str | `technitium` |
| [technitium_web_port](defaults/main.yml#L17) | int | `5380` |




## Task Overview


This role performs the following tasks:


### File: `tasks/main.yml`

| Task Name | Module | Has Conditions | Line |
|-----------|--------|----------------|------|
| [Create technitium directories](tasks/main.yml#L) | ansible.builtin.file | No | N/A |
| [Check if ASP.NET Core Runtime is installed](tasks/main.yml#L) | ansible.builtin.command | No | N/A |
| [Download .NET install script](tasks/main.yml#L) | ansible.builtin.get_url | Yes | N/A |
| [Install ASP.NET Core Runtime](tasks/main.yml#L) | ansible.builtin.command | Yes | N/A |
| [Create dotnet symlink](tasks/main.yml#L) | ansible.builtin.file | Yes | N/A |
| [Add dotnet to PATH](tasks/main.yml#L) | ansible.builtin.lineinfile | Yes | N/A |
| [Download Technitium DNS Server](tasks/main.yml#L) | ansible.builtin.get_url | No | N/A |
| [Extract Technitium DNS Server](tasks/main.yml#L) | ansible.builtin.unarchive | No | N/A |
| [Update resolv.conf](tasks/main.yml#L) | ansible.builtin.template | No | N/A |
| [Create systemd file for technitium](tasks/main.yml#L) | ansible.builtin.template | No | N/A |






## Example Playbook

```yaml
---
- hosts: all
  become: yes
  roles:
    - role: role_technitium

      vars:
        technitium_dotnet_dir: /opt/dotnet
        technitium_dotnet_version: 9.0
        technitium_dotnet_runtime: Microsoft.AspNetCore.App 9.0.

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
