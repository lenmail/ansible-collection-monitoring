# systemd_exporter

Install and configure the Prometheus systemd_exporter.

## Supported platforms

- Ubuntu 22.04+
- Debian 12+
- RHEL 9+

## Role Variables

The role interface is validated through `meta/argument_specs.yml`. Defaults are defined in `defaults/main.yml`.

```yaml
---
proxy_env: {}
systemd_exporter_version: 0.4.0
systemd_exporter_web_listen_address: 0.0.0.0
systemd_exporter_web_listen_port: 9558
systemd_exporter_log_level: warn
systemd_exporter_log_format: logger:stdout?json=true
systemd_exporter_binary_install_dir: /usr/local/bin
systemd_exporter_create_consul_agent_service: false
systemd_exporter_system_user: "{{ prometheus_user | default('prometheus') }}"
systemd_exporter_system_group: "{{ prometheus_group | default('prometheus') }}"
systemd_exporter_user_additional_groups: adm
systemd_exporter_collector_private: false
systemd_exporter_ccollector_user: false
systemd_exporter_ccollector_enable_restart_count: false
systemd_exporter_ccollector_enable_file_descriptor_size: false
systemd_exporter_ccollector_enable_ip_accounting: false
systemd_exporter_collector_unit_whitelist: .+
systemd_exporter_collector_unit_blacklist: .+\\.(device)
```

## Example Playbook

```yaml
- name: Apply systemd_exporter
  hosts: all
  become: true
  roles:
    - role: lenmail.monitoring.systemd_exporter
```

## Testing

The collection CI runs `ansible-lint`, `ansible-test sanity`, repository consistency tests, and per-role syntax checks using `roles/systemd_exporter/tests/test.yml`.
