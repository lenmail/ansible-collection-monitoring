# grafana_agent

Install and configure Grafana Agent.

## Supported platforms

- Ubuntu 22.04+
- Debian 12+
- RHEL 9+

## Role Variables

The role interface is validated through `meta/argument_specs.yml`. Defaults are defined in `defaults/main.yml`.

```yaml
---
proxy_env: {}
grafana_agent_version: 0.34.3
grafana_agent_config_flags_extra: {}
grafana_agent_config: "{{ all_grafana_agent_config|default({}) | combine(group_vars_grafana_agent_config|default({}),recursive=True,list_merge='append') | combine(host_vars_grafana_agent_config|default({}),recursive=True,list_merge='append') | combine(defaults_grafana_agent_config|default({}),recursive=True,list_merge='append') }}"
defaults_grafana_agent_config:
  server:
    log_level: warn
  metrics:
    wal_directory: /var/lib/grafana-agent
    wal_cleanup_age: 4h
    global:
      scrape_interval: 60s
      external_labels:
        product: '{{ ansible_product }}'
        environment: '{{ ansible_environment }}'
        ip: '{{ ansible_default_ipv4.address }}'
      remote_write:
      - url: https://prometheus.{{ ansible_external_domain }}/api/v1/write
  logs:
    positions_directory: /var/lib/grafana-agent
    configs:
    - name: default
      clients:
      - url: https://loki.{{ ansible_external_domain }}/api/v1/push
      scrape_configs:
      - job_name: dmesg
        static_configs:
        - targets:
          - localhost
          labels:
            job: dmesg
            hostname: '{{ ansible_hostname }}.{{ ansible_domain }}'
            environment: '{{ ansible_environment }}'
            ip: '{{ ansible_default_ipv4.address }}'
            product: '{{ ansible_product }}'
            __path__: /var/log/dmesg
      - job_name: auth
        static_configs:
        - targets:
          - localhost
          labels:
            job: auth
            hostname: '{{ ansible_hostname }}.{{ ansible_domain }}'
            environment: '{{ ansible_environment }}'
            ip: '{{ ansible_default_ipv4.address }}'
            product: '{{ ansible_product }}'
            __path__: /var/log/auth.log
      - job_name: journal
        journal:
          json: false
          max_age: 24h
          path: /var/log/journal
          labels:
            job: systemd-journal
            environment: '{{ ansible_environment }}'
            product: '{{ ansible_product }}'
            ip: '{{ ansible_default_ipv4.address }}'
        relabel_configs:
        - action: replace
          source_labels:
          - __journal__systemd_unit
          target_label: unit
        - source_labels:
          - __journal__hostname
          target_label: hostname
  integrations:
    agent:
      enabled: true
    node_exporter:
      enabled: true
      textfile_directory: /var/lib/grafana-agent-textfile
      systemd_unit_exclude: (.+\.(automount|device|mount|scope|slice)|(user.+|user-runtime-dir.+|systemd-(modules-load|networkd-wait-online|random-seed|remount-fs|sysctl|sysusers|tmpfiles.+|udev.+|update-utmp|user-sessions)|setvtrgb|polkit|plymouth.+|packagekit|motd-news|kmod-static-nodes|keyboard-setup|irqbalance|getty.+|console-setup|accounts-daemon).service)
      filesystem_mount_points_exclude: ^/(boot|boot/efi|dev.+|proc|run|run.+|sys.+|var/lib/docker.+|var/lib/containers/storage.+)($|/)$
      disable_collectors:
      - bonding
      - zfs
      - btrfs
      - infiniband
      - edac
      - nfs
      - nfsd
      - vmstat
      - entropy
      - rapl
      relabel_configs:
      - action: replace
        source_labels:
        - agent_hostname
        target_label: hostname
```

## Example Playbook

```yaml
- name: Apply grafana_agent
  hosts: all
  become: true
  roles:
    - role: lenmail.monitoring.grafana_agent
```

## Testing

The collection CI runs `ansible-lint`, `ansible-test sanity`, repository consistency tests, and per-role syntax checks using `roles/grafana_agent/tests/test.yml`.
