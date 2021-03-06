[![CI](https://github.com/DanielWeeber/ansible-role-mysqld_exporter/actions/workflows/release.yml/badge.svg?branch=master)](https://github.com/DanielWeeber/ansible-role-mysqld_exporter/actions/workflows/release.yml)

# Ansible Role: mysqld exporter

This role installs Prometheus' [mysqld exporter](https://github.com/prometheus/mysqld_exporter) on mysqld hosts.

## Requirements

Create MySQL user first!
Be sure to keep this a localhost user for safety reasons.

    CREATE USER 'exporter'@'localhost' IDENTIFIED BY 'XXXXXXXX' WITH MAX_USER_CONNECTIONS 3;
    GRANT PROCESS, REPLICATION CLIENT, SELECT ON *.* TO 'exporter'@'localhost';

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    mysqld_exporter_version: '0.13.0'

The version of mysqld exporter to install. Available releases can be found on the [tags](https://github.com/prometheus-community/mysqld_exporter/tags) listing in the Node exporter repository. Drop the `v` off the tag.

If you change the version, the `mysqld_exporter` binary will be replaced with the updated version, and the service will be restarted.

    mysqld_exporter_arch: 'amd64'
    mysqld_exporter_download_url: https://github.com/prometheus/mysqld_exporter/releases/download/v{{ mysqld_exporter_version }}/mysqld_exporter-{{ mysqld_exporter_version }}-{{ mysqld_exporter_arch }}.tar.gz

The path where the `mysqld_exporter` binary will be downloaded and installed from.

    mysqld_exporter_config_path: /etc/prometheus_mysqld.cnf
    mysqld_exporter_options: '--config.my-cnf={{ mysqld_exporter_config_path }}'

Any additional options to pass to `mysqld_exporter` when it starts, e.g. `--no-collector.wifi` if you want to ignore any WiFi data.

    mysqld_exporter_state: started
    mysqld_exporter_enabled: true

Controls for the `mysqld_exporter` service.

## Dependencies

None.

## Example Playbook

    - hosts: all
      roles:
        - role: ansible-role-mysqld_exporter

## License

MIT / BSD

## Author Information

This role was created in 2021 by [Daniel Weeber](https://github.com/DanielWeeber). Heavily inspired and forked from [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
