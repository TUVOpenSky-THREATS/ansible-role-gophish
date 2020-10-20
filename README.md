# ansible-role-gophish

[![Ansible Role Name](https://img.shields.io/ansible/role/UPDATE_ME?label=Role%20Name&logo=ansible&style=flat-square)](https://galaxy.ansible.com/justin_p/gophish)
[![Ansible Quality Score](https://img.shields.io/ansible/quality/UPDATE_ME?label=Ansible%20Quality%20Score&logo=ansible&style=flat-square)](https://galaxy.ansible.com/justin_p/gophish)
[![Ansible Role Downloads](https://img.shields.io/ansible/role/d/UPDATE_ME?label=Ansible%20Role%20Downloads&logo=ansible&style=flat-square)](https://galaxy.ansible.com/justin_p/gophish)
[![Github Actions](https://img.shields.io/github/workflow/status/justin-p/ansible-role-chisel/CI?label=Github%20Actions&logo=github&style=flat-square)](https://github.com/justin-p/ansible-role-gophish/actions)

A Ansible role that deploys the [gophish](https://github.com/gophish/gophish) application. This role does not install any mail services for relaying or web services for proxing gophish. You are expected to handle this in your own playbooks.

## Requirements

None.

## Variables

`defaults/main.yml`

| Variable                             | Description                                                                                    | Default value                                                                     |
| ------------------------------------ | ---------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| gophish_version                      | The version of gophish that should be installed.                                               | 0.11.0                                                                            |
| gophish_platform                     | The platform type.                                                                             | linux                                                                             |
| gophish_arch                         | The architecture.                                                                              | 64bit                                                                             |
| gophish_sha256                       | The sha256 sum of the downloaded file that matches the version, platform and arch combination. | sha256:f33ac7695850132c04d190f83ef54732421a8d4578be1475d3a819fe6173c462           |
| gophish_user                         | The user that gophish will run as.                                                             | {{ ansible_user }}                                                                |
| gophish_download_destination         | The download destination of the gophish release zip file.                                      | /tmp/gophish-v{{ gophish_version }}-{{ gophish_platform }}-{{ gophish_arch }}.zip |
| gophish_install_destination          | The install destination of gophish.                                                            | /home/{{ gophish_user }}/gophish                                                  |
| gophish_config_template_source       | The gophish config jinja template to deploy. Will default to included template.                | config.json.j2                                                                    |
| gophish_config_template_destination  | The destination of the gophish config file.                                                    | {{ gophish_install_destination }}/config.json                                     |
| gophish_service                      | The name of the gophish service.                                                               | gophish.service                                                                   |
| gophish_service_template_source      | The gophish service template to deploy. Will default to included template.                     | gophish.service.j2                                                                |
| gophish_service_template_destination | The destination where the service should be placed.                                            | /etc/systemd/system/{{ gophish_service }}                                         |
| gophish_service_log_directory        | The location of the gophish log directory. Used by the service.                                | /var/log/gophish                                                                  |

`vars/main.yml`

| Variable                    | Value                                                                                                                                                    |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| gophish_url                 | https://github.com/gophish/gophish/releases/download/v{{ gophish_version }}/gophish-v{{ gophish_version }}-{{ gophish_platform }}-{{ gophish_arch }}.zip |
| gophish_binary              | {{ gophish_install_destination }}/gophish                                                                                                                |
| gophish_binary_capability   | cap_net_bind_service+eip                                                                                                                                 |
| gophish_dependency_packages | ['unzip', 'libcap2-bin']                                                                                                                                 |

## Dependencies

[geerlingguy.pip](https://github.com/geerlingguy/ansible-role-pip)

## Example Playbooks

### Default role installation

```yaml
---
- hosts: gophish_hosts
  become: yes
  roles:
    - role: justin_p.gophish
```

### Using custom templates

```yaml
---
- hosts: gophish_hosts
  become: yes
  roles:
    - role: justin_p.gophish
      vars:
        - gophish_config_template_source: /path/to/your/template
        - gophish_service_template_source: /path/to/your/template
```

## Local Development

This role includes molecule that will spin up a local docker environment to deploy, configure and test this role.

Development requirements:

- Docker
- Molecule
- Molecule-docker
- yamllint
- ansible-lint

or simply use a VM with [this](https://github.com/justin-p/ansible-terraform-workstation) configuration.

## License

MIT

## Authors

Justin Perdok ([@justin-p](https://github.com/justin-p/)), Orange Cyberdefense

## Contributing

Feel free to open issues, contribute and submit your Pull Requests. You can also ping me on Twitter ([@JustinPerdok](https://twitter.com/JustinPerdok))
