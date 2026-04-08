# victoriametrics

Install and configure VictoriaMetrics.

## Supported platforms

- Ubuntu 22.04+
- Debian 12+
- RHEL 9+

## Role Variables

The role interface is validated through `meta/argument_specs.yml`. Defaults are defined in `defaults/main.yml`.

```yaml
---
proxy_env: {}
victoriametrics_version: 1.57.1
victoriametrics_web_listen_address: 0.0.0.0
victoriametrics_web_listen_port: 8428
victoriametrics_binary_install_dir: /usr/local/bin
victoriametrics_system_user: "{{ victoriametrics_user | default('prometheus') }}"
victoriametrics_system_group: "{{ victoriametrics_group | default('prometheus') }}"
victoriametrics_data_dir: /var/lib/victoriametrics
victoriametrics_config_dir: /etc/victoriametrics
victoriametrics_log_level: warn
victoriametrics_log_format: json
victoriametrics_prometheus_config: {}
victoriametrics_limit_nofile: 16384
victoriametrics_config:
  storageDataPath: '{{ victoriametrics_data_dir }}'
```

## Example Playbook

```yaml
- name: Apply victoriametrics
  hosts: all
  become: true
  roles:
    - role: lenmail.monitoring.victoriametrics
```

## Testing

The collection CI runs `ansible-lint`, `ansible-test sanity`, repository consistency tests, and per-role syntax checks using `roles/victoriametrics/tests/test.yml`.
