# Ansible Nessie TDP Extra

Deploys [Nessie](https://projectnessie.org/) server to the `nessie_server` Ansible group.

## Prerequisites

- `nessie-quarkus-0.29.0-runner` available in `files`
- Group `nessie_server` defined in the Ansible inventory
- Certificate of the CA available as `root.pem` in `files`
- Certificate files `{{ fqdn }}.key` and `{{ fqdn }}.pem` available in `files`

## Example

The following hosts file and playbook are given as examples.

### Host file

```
[nessie_server]
edge-01
```

### Available Playbooks

- [nessie.yml](../../playbooks/nessie.yml):

  - A-Z deployment of Nessie server

## TODO

- Integrate [Nessie CLI](https://projectnessie.org/tools/cli/)

## References

- [Releases](https://github.com/projectnessie/nessie/releases)
- [Configuration](https://projectnessie.org/try/configuration/)
