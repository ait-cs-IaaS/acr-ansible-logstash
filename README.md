# Ansible-Role: acr-ansible-logstash

AIT-CyberRange: Installs and configures logstash. 


## Requirements

- Debian or Ubuntu
- opensearch

## Role Variables

```yaml
xms_value: 2
xmx_value: 2

logstash_version: 8.15.1
logstash_deb_url: https://artifacts.elastic.co/downloads/logstash/logstash-{{ logstash_version }}-amd64.deb

logstash_beats_port: 5044
logstash_plugins:
  - logstash-output-opensearch
  - logstash-input-beats

logstash_config_tmpl: templates/logstash.conf.j2
logstash_config_files:
  - "{{ logstash_config_tmpl }}"

logstash_inputs_config: |
  beats {
      port => {{ logstash_beats_port }}
  }
logstash_filters_config: ""
logstash_outputs_config: ""
```

## Example Playbook

```yaml
- hosts: all
  roles:
    - logstash
      vars:
          logstash_outputs_config: |
            elasticsearch {
                hosts => "{{ logstash_elasticsearch_host }}"
                index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
            }

```

## License

GPL-3.0

## Author

- Lenhard Reuter