# postgresql_exporter

Install and configure the postgres_exporter for PostgreSQL metrics.

## Supported platforms

- Ubuntu 22.04+
- Debian 12+
- RHEL 9+

## Role Variables

The role interface is validated through `meta/argument_specs.yml`. Defaults are defined in `defaults/main.yml`.

```yaml
---
proxy_env: {}
postgres_exporter_version: 0.11.1
postgres_exporter_config_flags_extra: {}
postgres_exporter_dbname: postgres
postgres_exporter_username: postgres
postgres_exporter_query_filename: queries-default.yml
postgres_exporter_data_source_name: user={{ postgres_exporter_username }} dbname={{ postgres_exporter_dbname }} host=/var/run/postgresql/ sslmode=disable
```

## Example Playbook

```yaml
- name: Apply postgresql_exporter
  hosts: all
  become: true
  roles:
    - role: lenmail.monitoring.postgresql_exporter
```

## Testing

The collection CI runs `ansible-lint`, `ansible-test sanity`, repository consistency tests, and per-role syntax checks using `roles/postgresql_exporter/tests/test.yml`.
