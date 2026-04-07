# redis_exporter

Install and configure the Prometheus redis_exporter.

## Supported platforms

- Ubuntu 22.04+
- Debian 12+
- RHEL 9+

## Role Variables

The role interface is validated through `meta/argument_specs.yml`. Defaults are defined in `defaults/main.yml`.

```yaml
---
proxy_env: {}
redis_exporter_version: 1.46.0
redis_exporter_config_flags_extra:
  redis.addr: redis://localhost:6379
  redis.password: '{{ redis_password_sha256 }}'
```

## Example Playbook

```yaml
- name: Apply redis_exporter
  hosts: all
  become: true
  roles:
    - role: lenmail.monitoring.redis_exporter
```

## Testing

The collection CI runs `ansible-lint`, `ansible-test sanity`, repository consistency tests, and per-role syntax checks using `roles/redis_exporter/tests/test.yml`.
