# process_exporter

Install and configure the Prometheus process_exporter.

## Supported platforms

- Ubuntu 22.04+
- Debian 12+
- RHEL 9+

## Role Variables

The role interface is validated through `meta/argument_specs.yml`. Defaults are defined in `defaults/main.yml`.

```yaml
---
proxy_env: {}
process_exporter_binary_install_dir: /usr/local/bin
process_exporter_version: 0.7.5
process_exporter_web_listen_address: 0.0.0.0
process_exporter_web_listen_port: 9256
process_exporter_config_dir: /etc/process_exporter
process_exporter_system_user: "{{ prometheus_user | default('process_exporter') }}"
process_exporter_system_group: "{{ prometheus_group | default('process_exporter') }}"
process_exporter_create_consul_agent_service: true
process_exporter_names: "{% raw %}\n  - name: \"{{.Comm}}\"\n    cmdline:\n      - '.+'\n{% endraw %}\n"
```

## Example Playbook

```yaml
- name: Apply process_exporter
  hosts: all
  become: true
  roles:
    - role: lenmail.monitoring.process_exporter
```

## Testing

The collection CI runs `ansible-lint`, `ansible-test sanity`, repository consistency tests, and per-role syntax checks using `roles/process_exporter/tests/test.yml`.
