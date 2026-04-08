# snmp_exporter

Install and configure the Prometheus snmp_exporter.

## Supported platforms

- Ubuntu 22.04+
- Debian 12+
- RHEL 9+

## Role Variables

The role interface is validated through `meta/argument_specs.yml`. Defaults are defined in `defaults/main.yml`.

```yaml
---
proxy_env: {}
snmp_exporter_version: 0.18.0
snmp_exporter_web_listen_address: 0.0.0.0
snmp_exporter_web_listen_port: '9116'
snmp_exporter_log_level: warn
snmp_exporter_log_format: json
snmp_exporter_binary_local_dir: /usr/local/bin
snmp_exporter_config_dir: /etc/snmp_exporter
snmp_exporter_create_consul_agent_service: true
snmp_exporter_config_file: ''
snmp_exporter_system_user: "{{ prometheus_user | default('snmp_exporter') }}"
snmp_exporter_system_group: "{{ prometheus_group | default('snmp_exporter') }}"
snmp_exporter_limit_nofile: 8192
```

## Example Playbook

```yaml
- name: Apply snmp_exporter
  hosts: all
  become: true
  roles:
    - role: lenmail.monitoring.snmp_exporter
```

## Testing

The collection CI runs `ansible-lint`, `ansible-test sanity`, repository consistency tests, and per-role syntax checks using `roles/snmp_exporter/tests/test.yml`.
