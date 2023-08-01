# Ansible Airflow TDP Extra

This role deploys the Apache Airflow release. In general it installs and/or configures:

- **Airflow:** Installs packaged airflow 2.2.5 or 2.6.2, and all its dependencies, through a virtual-environment and pip-install
- **Scheduler:** Responsible for monitoring and ensuring scheduled execution of all tasks. Decides where and when tasks are run.
- **Webserver:** A Flask server that serves the Airflow UI. Helps monitor, trigger and debug DAGs.
- **Broker:** Uses Redis. This Facilitates communication between the Airflow Scheduler and the Workers, handling task messages and their status updates.
- **Executor:** Uses Celery. Responsible for determining how tasks are run, in parallel or sequentially, locally or distributed.
- **Flower:** Used to monitor task progress and history in celery executors.
- **Workers:** Execute the tasks. They pick up and run tasks sent to the queue by the executor.
- **Database (Metadata DB):** Uses postgresql. Stores metadata about the state of tasks and workflows, and assists in recovery in case of failures. 
- **Dag Directory:** Folder of DAG files. It is read by the scheduler and executor, and has to exist in every worker as well.
- **Services:** Creates services for web-server, scheduler, workers, redis, flower.

    ![image](https://github.com/TOSIT-IO/TDP/assets/12486708/9a3e09df-d4bb-4d27-ab09-b0194bcc568c)

## Prerequisites

- `python3` and `python3-pip` installed on all nodes.

- Airflow version = 2.2.5 requires python >= 3.6 & Airflow version > 2.6.3 requires python >= 3.8

- Hadoop TDP release .tar.gz (`hadoop_dist_file` role variable) file available in `files`

- Groups `airflow_webserver`, `airflow_scheduler` and `airflow_worker` defined in the Ansible hosts file

- Postegres user and databse to store metadata. This is done in prerequisites.

- Certificate files `{{ fqdn }}.key` and `{{ fqdn }}.pem` for every node available in `files`

- Admin access to a KDC with the `realm`, `kadmin_principal` and `kadmin_password` role vars provided

- Extra steps done for hadoop and hive configuration. This is currently added in this collection through `tdp-cluster.yml`.

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

### Utils Tasks:

These tasks are intended for some extra tasks the user might or might not want to include in the installation. They include:

- [Policies:](https://airflow.apache.org/docs/apache-airflow/stable/administration-and-deployment/cluster-policies.html) Currently used to limit what a user (ie. tdp_user) can do when editing a dag.
- [Roles](https://airflow.apache.org/docs/apache-airflow/stable/administration-and-deployment/security/access-control.html): Used to limit what a user/group can view or do within a dag.
-  Dag Directory structure: _Working on dag-folder structure to handle groups/users._
- Creates connectors with impersonation capabilities for Hive, Spark and HDFS
- Dags examples that can be directly used on top of TDP (given the previous steps have been included)

  
### Security implementations:
  - [Policies:](https://airflow.apache.org/docs/apache-airflow/stable/administration-and-deployment/cluster-policies.html) Currently used to limit what a user (ie. tdp_user) can do when editing a dag.
  - [Roles](https://airflow.apache.org/docs/apache-airflow/stable/administration-and-deployment/security/access-control.html): Used to limit what a user/group can view or do within a dag.
  -  Dag Directory structure: _Working on dag-folder structure to handle groups/users._
