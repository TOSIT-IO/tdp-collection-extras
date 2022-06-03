# Ansible Kafka Broker

Deploys Apache Kafka brokers to the `kafka_broker` Ansible group.

## Prerequisites

- `java-1.8.0-openjdk` and `krb5-workstation` installed on all nodes
- `kafka_2.13-3.2.0.tgz` available in `files` directory
- Group `kafka_broker` defined in the Ansible inventory
- Certificate of the CA available as `root.pem` in `files`
- Certificate files `{{ fqdn }}.key` and `{{ fqdn }}.pem` available in `files`
- Admin access to a KDC with the `realm`, `kadmin_principal` and `kadmin_password` role vars provided

## Example

The following hosts file and playbook are given as examples.

### Host file

```
[kafka_broker]
worker-01
worker-02
worker-03

[kafka_client]
edge-01
```

### Available Playbooks

[kafka](../../playbooks/kafka.yml)
[kafka_ssl-tls_install](../../playbooks/kafka_ssl-tls_install.yml)
[kafka_kerberos_install](../../playbooks/kafka_kerberos_install.yml)
[kafka_client](../../playbooks/kafka_client.yml)
[kafka_broker](../../playbooks/kafka_broker.yml)
[kafka_broker_stop](../../playbooks/kafka_broker_stop.yml)
[kafka_broker_start](../../playbooks/kafka_broker_start.yml)
[kafka_broker_install](../../playbooks/kafka_broker_install.yml)
[kafka_broker_config](../../playbooks/kafka_broker_config.yml)


