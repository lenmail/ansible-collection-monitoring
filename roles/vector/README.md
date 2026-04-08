# vector

Install and configure Vector for log and metric pipelines.

## Supported platforms

- Ubuntu 22.04+
- Debian 12+
- RHEL 9+

## Role Variables

The role interface is validated through `meta/argument_specs.yml`. Defaults are defined in `defaults/main.yml`.

```yaml
---
vector_prepare: true
vector_install: true
vector_configure: true
vector_service: true
vector_configure_rm_default: true
vector_configure_config_path: /etc/vector/vector.yaml
vector_configure_data_dir: /var/lib/vector
vector_configure_api:
  enabled: true
vector_configure_sources:
  syslog:
    type: syslog
    address: 0.0.0.0:514
    max_length: '102_400'
    mode: udp
    path: /tmp/syslog_udp
vector_configure_sinks:
  loki:
    type: loki
    inputs:
    - source_syslog
    endpoint: http://localhost:3100
    labels:
      datasource: source_syslog
    out_of_order_action: rewrite_timestamp
    encoding:
      codec: json
vector_service_enabled: true
vector_service_status: started
```

## Example Playbook

```yaml
- name: Apply vector
  hosts: all
  become: true
  roles:
    - role: lenmail.monitoring.vector
```

## Testing

The collection CI runs `ansible-lint`, `ansible-test sanity`, repository consistency tests, and per-role syntax checks using `roles/vector/tests/test.yml`.
