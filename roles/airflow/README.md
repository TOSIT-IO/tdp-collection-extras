# Ansible Airflow TDP Extra

This role deploys the Apache Airflow release. It installs:

- Airflow Webserver
- Airflow Scheduler
- Airflow Worker (Celery + Flower)

Currently the role only supports the deployment of SSL-enabled Airflow.

## Prerequisites

- `python3` and `python3-pip` installed on all nodes
- Hadoop TDP release .tar.gz (`hadoop_dist_file` role variable) file available in `files`
- Groups `airflow_webserver`, `airflow_executor` defined in the Ansible hosts file
- Certificate files `{{ fqdn }}.key` and `{{ fqdn }}.pem` for every node available in `files`
- Admin access to a KDC with the `realm`, `kadmin_principal` and `kadmin_password` role vars provided

## Example

The following hosts file and playbook are given as examples.

### Host file

```
[airflow_webserver]
tdp-master-1

[airflow_executor]
tdp-worker-1
```

### Required variables

- `realm`: Kerberos realm of the cluster
- `kadmin_principal`: admin principal used to connect kadmin service
- `kadmin_password`: passowrd of the admin principal

### Example playbooks

- [airflow.yml](../../playbooks/airflow.yaml)