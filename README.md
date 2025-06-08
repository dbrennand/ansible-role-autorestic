# Ansible Role: dbrennand.autorestic

![Ansible-Lint](https://github.com/dbrennand/ansible-role-autorestic/actions/workflows/ansible-lint.yml/badge.svg)
![Molecule](https://github.com/dbrennand/ansible-role-autorestic/actions/workflows/molecule.yml/badge.svg)
![Ansible-Release](https://github.com/dbrennand/ansible-role-autorestic/actions/workflows/ansible-release.yml/badge.svg)

Ansible role to configure backups using [autorestic](https://autorestic.vercel.app/).

## Requirements

None.

## Assumptions

This role places the autorestic and restic binaries into `/opt/autorestic/bin` and `/opt/restic/bin` respectively. Symbolic links are created to `/usr/local/bin`.

## Role Variables

```yaml
autorestic_architecture: mips
```

Overrides `ansible_architecture` in case you have some exotic combination. See [dependencies](#dependencies) for further details.

```yaml
autorestic_version: 1.8.3
autorestic_restic_version: 0.18.0
```

The version of [autorestic](https://autorestic.vercel.app/) and [restic](https://restic.net/) to install.

```yaml
autorestic_install_directory:
  path: /opt/autorestic/bin
  # Optional
  # owner: owner
  # group: group
  # mode: 0700
autorestic_restic_install_directory:
  path: /opt/restic/bin
  # ...
```

The directories to install the autorestic and restic binaries at.

```yaml
autorestic_config:
  version: 2
  locations:
    home:
      from: /home/me
      to: remote
      # Every Monday
      cron: "0 0 * * MON"
  backends:
    remote:
      type: b2
      path: "myBucket:backup/home"
      env:
        B2_ACCOUNT_ID: ID
        B2_ACCOUNT_KEY: Key
```

See the [autorestic documentation](https://autorestic.vercel.app/config) for details on the YAML configuration.

```yaml
autorestic_config_template: autorestic.yml.j2
```

The [`autorestic.yml.j2`](templates/autorestic.yml.j2) template to use for the autorestic YAML configuration. By default the content of `autorestic_config` is templated. This can be overridden to use a custom template. See [#14](https://github.com/dbrennand/ansible-role-autorestic/issues/14#issuecomment-2198394106) for an example.

```yaml
autorestic_config_file: ~/.autorestic.yml
```

The autorestic YAML configuration to be placed into the `~/.autorestic.yml` file.

```yaml
autorestic_info: false
```

Whether or not to run `autorestic info` to validate that the autorestic YAML configuration is valid.

```yaml
autorestic_check: false
```

Whether or not to run `autorestic check` to make sure backends are configured properly and initialises them if they are not already.

```yaml
autorestic_cron: false
autorestic_cron_verbose: false
```

Whether or not to create an autorestic crontab entry to trigger automated backups. Use `autorestic_cron_verbose` to enable verbose logging. Autorestic locations need to be configured with [`cron`](https://autorestic.vercel.app/location/cron).

```yaml
autorestic_state: present
```

Whether or not to remove autorestic, restic, configuration and crontab entry. Set to `absent` for removal.

> This will not affect any backends and their data.

## Dependencies

This role depends on precompiled binaries published on GitHub:

* [cupcakearmy/autorestic](https://github.com/cupcakearmy/autorestic/releases/)
* [restic/restic](https://github.com/restic/restic/releases/)

When using `autorestic_architecture`, refer to the release assets for supported binary architectures.

## Example Playbook

```yaml
- hosts: all
  roles:
    - dbrennand.autorestic
```

## Molecule Tests 🧪

To test the role, use Molecule: `molecule test`

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) for details.

## Authors & Contributors

[**dbrennand**](https://github.com/dbrennand) - *Author*

[**whysthatso**](https://github.com/whysthatso) - *Contributor*

[**PleaseStopAsking**](https://github.com/PleaseStopAsking) - *Contributor*

[**markstos**](https://github.com/markstos) - *Contributor*

[**micxer**](https://github.com/micxer) - *Contributor*
