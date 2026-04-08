# squid_exporter

Install and configure the Prometheus squid_exporter.

## Supported platforms

- Ubuntu 22.04+
- Debian 12+
- RHEL 9+

## Role Variables

The role interface is validated through `meta/argument_specs.yml`. Defaults are defined in `defaults/main.yml`.

```yaml
---
squid_exporter_version: 1.8.2
squid_exporter_web_listen_address: 0.0.0.0
squid_exporter_web_listen_port: 9301
squid_exporter_binary_local_dir: /usr/local/bin
squid_exporter_system_user: "{{ prometheus_user | default('prometheus') }}"
squid_exporter_system_group: "{{ prometheus_group | default('prometheus') }}"
squid_exporter_log_level: info
```

## Example Playbook

```yaml
- name: Apply squid_exporter
  hosts: all
  become: true
  roles:
    - role: lenmail.monitoring.squid_exporter
```

## Testing

The collection CI runs `ansible-lint`, `ansible-test sanity`, repository consistency tests, and per-role syntax checks using `roles/squid_exporter/tests/test.yml`.
