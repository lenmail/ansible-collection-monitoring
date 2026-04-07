# promtail

Install and configure Grafana Promtail log shipping.

## Supported platforms

- Ubuntu 22.04+
- Debian 12+
- RHEL 9+

## Role Variables

The role interface is validated through `meta/argument_specs.yml`. Defaults are defined in `defaults/main.yml`.

```yaml
---
proxy_env: {}
promtail_version: 3.3.0
promtail_listen_port: 9080
promtail_listen_address: 0.0.0.0
promtail_config_dir: /etc/promtail
promtail_config_file: config.yml
promtail_config_file_sd_dir: '{{ promtail_config_dir }}/file_sd'
promtail_binary_install_dir: /usr/local/bin
promtail_data_dir: /var/lib/promtail
promtail_config_log_level: warn
promtail_system_user: "{{ prometheus_user | default('loki') }}"
promtail_system_group: "{{ prometheus_user | default('loki') }}"
promtail_user_additional_groups: adm
promtail_add_network_capabilities: true
promtail_config:
  positions:
    filename: '{{ promtail_data_dir }}/positions.yaml'
  server:
    http_listen_address: '{{ promtail_listen_address }}'
    http_listen_port: '{{ promtail_listen_port }}'
    grpc_listen_address: '{{ promtail_listen_address }}'
    grpc_listen_port: 0
    log_level: '{{ promtail_config_log_level }}'
  clients:
  - url: http://localhost:3100/loki/api/v1/push
    tenant_id: prime
    external_labels:
      node: '{{ inventory_hostname }}'
  scrape_configs:
  - job_name: journal
    journal:
      path: /var/log/journal
      labels:
        job: systemd-journal
    relabel_configs:
    - source_labels:
      - __journal__systemd_unit
      target_label: unit
    - source_labels:
      - __journal__hostname
      target_label: host
    - source_labels:
      - __journal__transport
      target_label: transport
```

## Example Playbook

```yaml
- name: Apply promtail
  hosts: all
  become: true
  roles:
    - role: lenmail.monitoring.promtail
```

## Testing

The collection CI runs `ansible-lint`, `ansible-test sanity`, repository consistency tests, and per-role syntax checks using `roles/promtail/tests/test.yml`.
