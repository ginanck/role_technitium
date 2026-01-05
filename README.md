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



### File: `defaults/main.yml`

| Variable | Default Value | Description |
|----------|---------------|-------------|
| `technitium_dotnet_dir` | `/opt/dotnet` | None |
| `technitium_dotnet_version` | `9.0` | None |
| `technitium_dotnet_runtime` | `Microsoft.AspNetCore.App 9.0.` | None |
| `technitium_dotnet_url` | `https://dot.net/v1/dotnet-install.sh` | None |
| `technitium_dns_config` | `/etc/dns` | None |
| `technitium_dns_dir` | `/opt/technitium/dns` | None |
| `technitium_dns_url` | `https://download.technitium.com/dns/DnsServerPortable.tar.gz` | None |
| `technitium_install_log` | `{{ technitium_dns_dir }}/install.log` | None |
| `technitium_service_name` | `technitium` | None |
| `technitium_web_port` | `5380` | None |
| `technitium_nameservers` | `[]` | None |
| `technitium_nameservers.0` | `127.0.0.53` | None |
| `technitium_search_domains` | `[]` | None |
| `technitium_search_domains.0` | `.` | None |
| `technitium_resolv_options` | `[]` | None |
| `technitium_resolv_options.0` | `edns0` | None |
| `technitium_resolv_options.1` | `trust-ad` | None |




## Task Overview


This role performs the following tasks:


### `main.yml`


- **Create technitium directories**
- **Check if ASP.NET Core Runtime is installed**
- **Download .NET install script**
- **Install ASP.NET Core Runtime**
- **Create dotnet symlink**
- **Add dotnet to PATH**
- **Download Technitium DNS Server**
- **Extract Technitium DNS Server**
- **Update resolv.conf**
- **Create systemd file for technitium**




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

## Documentation Maintenance

### Updating Dependencies

1. **Update** `meta/main.yml`:
   ```yaml
   documented_requirements:
     - src: https://github.com/user/role.git
       version: master
     - name: collection.name
       version: 1.0.0
   ```

2. **Sync** `meta/install_requirements.yml` with the same requirements

3. **Regenerate** documentation:
   ```bash
   pre-commit run --all-files
   ```

### Template Updates

- Edit `.docsible_template.md` for structure changes
- Test with: `docsible --role . --md-template .docsible_template.md -nob -com -tl`
- Commit both template and generated README.md

### Quick Checklist

When updating dependencies:
- [ ] Add to `meta/main.yml` → `documented_requirements`
- [ ] Add to `meta/install_requirements.yml`
- [ ] Run `pre-commit run --all-files`
- [ ] Verify generated README.md
- [ ] Commit all changes

## License


license (GPL-2.0-or-later, MIT, etc)


## Author Information


**Author:** Gorkem Korkmaz



**Company:** GK


**GitHub:** [gkorkmaz](https://github.com/gkorkmaz)

---
*This documentation was automatically generated using [docsible](https://github.com/zbohm/docsible).*
<!-- DOCSIBLE END -->
