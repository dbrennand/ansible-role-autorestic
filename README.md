# Ansible Role: dbrennand.backup

![Ansible-Lint](https://github.com/dbrennand/ansible-role-backup/actions/workflows/ansible-lint.yml/badge.svg)

An Ansible role to configure backups using [autorestic](https://autorestic.vercel.app/).

## Requirements

None.

## Role Variables

```yaml
backup_restic_version: 0.14.0
backup_autorestic_version: 1.7.3
```

The version of [restic](https://restic.net/) and [autorestic](https://autorestic.vercel.app/) to install.

```yaml
backup_autorestic_config: |-
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
      path: 'myBucket:backup/home'
      env:
        B2_ACCOUNT_ID: ID
        B2_ACCOUNT_KEY: Key
```

The autorestic YAML configuration to be placed into the `~/.autorestic.yml` file. See the [autorestic documentation](https://autorestic.vercel.app/config) for details on the YAML configuration.

```yaml
backup_autorestic_check: true
```

Whether or not to run `autorestic check` to make sure backends are configured properly and initialises them if they are not already.

```yaml
backup_autorestic_cron: true
```

Whether or not to create an autorestic crontab entry to trigger automated backups. Autorestic locations need to be configured with `cron`.

## Dependencies

None.

## Example Playbook

```yaml
- hosts: all
  roles:
    - dbrennand.backup
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) for details.

## Authors & Contributors

[**dbrennand**](https://github.com/dbrennand) - *Author*
