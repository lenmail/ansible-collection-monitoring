# consul_exporter

Install and configure the Prometheus consul_exporter.

## Supported platforms

- Ubuntu 22.04+
- Debian 12+
- RHEL 9+

## Role Variables

The role interface is validated through `meta/argument_specs.yml`. Defaults are defined in `defaults/main.yml`.

```yaml
---
proxy_env: {}
consul_exporter_version: 0.7.1
consul_exporter_web_listen_address: 0.0.0.0
consul_exporter_web_listen_port: 9107
consul_exporter_log_level: warn
consul_exporter_log_format: json
consul_exporter_binary_install_dir: /usr/local/bin
consul_exporter_system_user: "{{ prometheus_user | default('consul_exporter') }}"
consul_exporter_system_group: "{{ prometheus_group | default('consul_exporter') }}"
consul_exporter_consul_token: example
consul_exporter_params:
- consul.server-namelocalhost:8500
```

## Example Playbook

```yaml
- name: Apply consul_exporter
  hosts: all
  become: true
  roles:
    - role: lenmail.monitoring.consul_exporter
```

## Testing

The collection CI runs `ansible-lint`, `ansible-test sanity`, repository consistency tests, and per-role syntax checks using `roles/consul_exporter/tests/test.yml`.
