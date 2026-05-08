# alloy

Installiert und konfiguriert Grafana Alloy als Standard-Agent fuer Linux- und Windows-Systeme.

## Plattform-Support

- Ubuntu 22.04+
- Debian 12+
- RHEL 9+
- Windows Server mit WinRM und `ansible.windows`

## Zielzustand

Die Rolle betreibt Alloy als hostnahen Agenten fuer Logs und Metriken:

- Linux-Hostmetriken ueber `prometheus.exporter.unix`
- Windows-Hostmetriken ueber `prometheus.exporter.windows`
- Linux-Logs aus journald und definierten Logdateien
- Windows Event Logs aus Application, System und Security
- Versand von Logs an Loki
- Versand von Metriken per Prometheus Remote Write
- erweiterbare Alloy-Rohkonfiguration fuer Sonderquellen

Exporter fuer fachliche Plattformen wie UniFi, Proxmox, Ceph, SNMP, Blackbox oder iLO werden bewusst nicht in diese Rolle eingebaut. Sie bleiben eigene Integrationen und werden bei Bedarf per `alloy_extra_config` oder separater Rolle angebunden.

## Wichtige Variablen

```yaml
alloy_labels:
  tenant: kunde_a
  environment: prod
  site: rz1
  platform: proxmox
  system_role: hypervisor
  component_type: compute
  managed_by: ansible

alloy_loki_endpoint: http://observability.example.local:3100/loki/api/v1/push
alloy_loki_tenant_id: kunde_a

alloy_prometheus_remote_write_url: http://observability.example.local:9090/api/v1/write

alloy_linux_metrics_enabled: true
alloy_linux_journal_enabled: true
alloy_linux_file_logs_enabled: true

alloy_windows_metrics_enabled: true
alloy_windows_eventlog_enabled: true
```

Wenn `alloy_loki_endpoint` leer ist, wird kein Logversand konfiguriert. Wenn `alloy_prometheus_remote_write_url` leer ist, wird kein Metrikversand konfiguriert.

## Linux-Beispiel

```yaml
- name: Deploy Alloy on Linux hosts
  hosts: linux
  become: true
  roles:
    - role: lenmail.monitoring.alloy
      vars:
        alloy_loki_endpoint: http://monitoring.internal:3100/loki/api/v1/push
        alloy_prometheus_remote_write_url: http://monitoring.internal:9090/api/v1/write
        alloy_labels:
          tenant: internal
          environment: prod
          site: rz1
          platform: linux
          system_role: server
          component_type: compute
          managed_by: ansible
```

## Windows-Beispiel

```yaml
- name: Deploy Alloy on Windows hosts
  hosts: windows
  gather_facts: true
  roles:
    - role: lenmail.monitoring.alloy
      vars:
        alloy_windows_install_method: winget
        alloy_loki_endpoint: http://monitoring.internal:3100/loki/api/v1/push
        alloy_prometheus_remote_write_url: http://monitoring.internal:9090/api/v1/write
        alloy_labels:
          tenant: internal
          environment: prod
          site: rz1
          platform: windows
          system_role: application
          component_type: compute
          managed_by: ansible
```

## Proxmox/Ceph-Erweiterung

Hostnahe Metriken und Logs kommen aus Alloy. Plattformmetriken sollten ueber geeignete Exporter oder vorhandene Prometheus-Endpunkte angebunden werden.

```yaml
alloy_extra_config: |
  prometheus.scrape "ceph_mgr" {
    targets = [{
      __address__ = "127.0.0.1:9283",
      job         = "ceph",
    }]
    forward_to = [prometheus.remote_write.default.receiver]
  }
```

## Betrieb

Die Rolle validiert die gerenderte Alloy-Konfiguration mit `alloy fmt --test`, sofern `alloy_validate_config` aktiv ist. Bei Plattformen ohne lokal verfuegbares Alloy-Binary kann die Validierung temporaer deaktiviert werden.

Interaktive Betriebsanleitungen verwenden `vim`, nicht `nano`.
