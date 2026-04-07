# unbound_exporter

Install and configure the Prometheus unbound_exporter.

## Supported platforms

- Ubuntu 22.04+
- Debian 12+
- RHEL 9+

## Role Variables

The role interface is validated through `meta/argument_specs.yml`. Defaults are defined in `defaults/main.yml`.

```yaml
---
proxy_env: {}
unbound_exporter_log_level: warn
unbound_exporter_config_flags_extra:
  block-file: /etc/unbound/unbound.block.conf
  unbound.uri: tcp://localhost:953
```

## Example Playbook

```yaml
- name: Apply unbound_exporter
  hosts: all
  become: true
  roles:
    - role: lenmail.monitoring.unbound_exporter
```

## Testing

The collection CI runs `ansible-lint`, `ansible-test sanity`, repository consistency tests, and per-role syntax checks using `roles/unbound_exporter/tests/test.yml`.
