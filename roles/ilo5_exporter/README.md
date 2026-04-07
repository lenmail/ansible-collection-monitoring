# ilo5_exporter

Install and configure the HPE iLO5 Prometheus exporter.

## Supported platforms

- Ubuntu 22.04+
- Debian 12+
- RHEL 9+

## Role Variables

The role interface is validated through `meta/argument_specs.yml`. Defaults are defined in `defaults/main.yml`.

```yaml
---
proxy_env: {}
ilo5_exporter_config_flags_extra:
  api.username: '{{ ilo5_user }}'
  api.password: '{{ ilo5_pass }}'
```

## Example Playbook

```yaml
- name: Apply ilo5_exporter
  hosts: all
  become: true
  roles:
    - role: lenmail.monitoring.ilo5_exporter
```

## Testing

The collection CI runs `ansible-lint`, `ansible-test sanity`, repository consistency tests, and per-role syntax checks using `roles/ilo5_exporter/tests/test.yml`.
