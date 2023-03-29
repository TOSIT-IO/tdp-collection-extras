# Ansible Airflow TDP Extra

This role deploys the Apache Airflow release. It installs:

- Airflow Webserver
- Airflow Scheduler
- Airflow Worker (Celery + Flower)
- Airflow broker :  Redis

Currently the role only supports the deployment of SSL-enabled Airflow.

## Prerequisites

- `python3` and `python3-pip` installed on all nodes. Airflow version > 2.2.5 requires python >= 3.7

- Hadoop TDP release .tar.gz (`hadoop_dist_file` role variable) file available in `files`

- Groups `airflow_webserver`, `airflow_scheduler` and `airflow_worker` defined in the Ansible hosts file

- Postegres user and databse to store metadata.

- Certificate files `{{ fqdn }}.key` and `{{ fqdn }}.pem` for every node available in `files`

- Admin access to a KDC with the `realm`, `kadmin_principal` and `kadmin_password` role vars provided   

  ```yaml
  # This section can be added to tdp_vars/hadoop/hadoop.yml
  # This should be done automatically at tdp init step
  auth_to_local:
    airflow:
    - RULE:[2:$1/$2@$0](airflow/.*@{{ realm }})s/.*/airflow/
  
  core_site:
    hadoop.proxyuser.airflow.groups: "*"
    hadoop.proxyuser.airflow.users: "*"
    hadoop.proxyuser.airflow.hosts: "*"
  ```

  ```yaml
  # This section can be added to tdp_vars/hive/hive.yml
  # This should be done automatically at tdp init step
  hive_site:
    hive.security.authorization.sqlstd.confwhitelist.append: "{{ 'mapred.job.name' }}|{{ 'mapred.queue.name' }}|{{ 'airflow.ctx.*' }}"
    hive.stats.autogather: false
    hive.txn.stats.enabled: false
  ```

## Example

The following hosts file and playbook are given as examples.

### Host file

```
[airflow_webserver:children]
master1

[airflow_scheduler:children]
master1

[airflow_worker:children]
worker
```

### Required variables

- `realm`: Kerberos realm of the cluster
- `kadmin_principal`: admin principal used to connect kadmin service
- `kadmin_password`: passowrd of the admin principal