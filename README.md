# Ansible Role: Unifi Poller

[![ubuntu-18](https://img.shields.io/badge/ubuntu-18.x-orange?style=flat&logo=ubuntu)](https://ubuntu.com/)
[![ubuntu-20](https://img.shields.io/badge/ubuntu-20.x-orange?style=flat&logo=ubuntu)](https://ubuntu.com/)
[![debian-9](https://img.shields.io/badge/debian-9.x-orange?style=flat&logo=debian)](https://www.debian.org/)
[![debian-10](https://img.shields.io/badge/debian-10.x-orange?style=flat&logo=debian)](https://www.debian.org/)
[![centos-7](https://img.shields.io/badge/centos-7.x-orange?style=flat&logo=centos)](https://www.centos.org/)
[![centos-8](https://img.shields.io/badge/centos-8.x-orange?style=flat&logo=centos)](https://www.centos.org/)

[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg?style=flat)](https://opensource.org/licenses/MIT)
[![GitHub issues](https://img.shields.io/github/issues/OnkelDom/ansible-role-unifi-poller?style=flat)](https://github.com/OnkelDom/ansible-role-unifi-poller/issues)
[![GitHub tag](https://img.shields.io/github/tag/OnkelDom/ansible-role-unifi-poller.svg?style=flat)](https://github.com/OnkelDom/ansible-role-unifi-poller/tags)
[![GitHub action](https://github.com/OnkelDom/ansible-role-unifi-poller/workflows/ansible-lint/badge.svg)](https://github.com/OnkelDom/ansible-role-unifi-poller)

## Description

Deploy and manage prometheus [Unifi Poller](https://github.com/unpoller/unpoller/) using ansible. This in an Prometheus Exporter for Unifi Controller.

For detailed configuration see [Unifi Poller config example](https://github.com/unpoller/unpoller/blob/master/examples/up.json.example).

## Requirements

- Ansible >= 2.9 (It might work on previous versions, but we cannot guarantee it)

## Role Variables

All variables which can be overridden are stored in [defaults/main.yml](defaults/main.yml) file as well as in table below.

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `proxy_env` | {} | Proxy environment variables |
| `unifi_poller_version` | 2.1.3 | Version | 
| `unifi_poller_system_user` | unifi_poller | System user name |
| `unifi_poller_system_group` | unifi_poller | System user group |
| `unifi_poller_binary_install_dir` | /usr/local/bin | Binary install folder |
| `unifi_poller_config_path` | /etc/unifi_poller | Config folder |
| `unifi_poller_config_file` | config.yaml | Config file name |
| `unifi_poller_config` | {} | Configuration |

## Example
```yaml
# For detailed configuration see https://github.com/unpoller/unpoller/blob/master/examples/up.json.example
unifi_poller_config:
  poller:
    quiet: false
    debug: false
    plugins: []
  prometheus:
    disable: false
    http_listen: "0.0.0.0:9130"
    ssl_cert_path: ""
    ssl_key_path: ""
    report_errors: false
  loki:
    url: "http://localhost:3100/api/v1/push"
    # The rest of this is advanced & optional. See wiki.
    user: ""
    pass: ""
    verify_ssl: false
    tenant_id: ""
    interval: "2m"
    timeout: "10s"
  influxdb:
    disable: false
    interval: "30s"
    url: "http://127.0.0.1:8086"
    user: "unifipoller"
    pass: "unifipoller"
    db: "unifi"
    verify_ssl: false
    dead_ports: false
  webserver:
    enable: false
    port: 37288
    html_path: "{{ unifi_poller_config_path }}/web"
    ssl_cert_path: ""
    ssl_key_path: ""
    max_events: 200
    accounts:
      captain: "$2a$04$mxw6i0LKH6u46oaLK2cq5eCTAAFkfNiRpzNbz.EyvJZZWNa2FzIlS"
  unifi:
    dynamic: false
    defaults:
      url:  "https://127.0.0.1:8443"
      user: "unifipoller"
      pass: "unifipoller"
      sites:
        - all
      save_ids: false
      save_events: false
      save_alarms: false
      save_anomalies: false
      save_dpi: false
      save_sites: true
      hash_pii: false
      verify_ssl: false
    controllers:
    # Repeat the following stanza to poll multiple controllers.
      - url:  "https://127.0.0.1:8443"
        user: "unifipoller"
        pass: "unifipoller"
        sites:
          - all
        save_ids: false
        save_events: false
        save_alarms: false
        save_anomalies: false
        save_dpi: false
        save_sites: true
        hash_pii: false
        verify_ssl: false
```

### Playbook

```yaml
- hosts: all
  become: yes
  roles:
    - onkeldom.unifi_poller
```

## Contributing

See [contributor guideline](CONTRIBUTING.md).

## License

This project is licensed under MIT License. See [LICENSE](/LICENSE) for more details.

