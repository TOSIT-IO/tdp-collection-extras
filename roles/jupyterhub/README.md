# Jupyterhub TDP Extras

Deploys Jupyterhub server to the `jupyterhub_server` Ansible group.

## Prerequisites

- `python>=3.6` installed on all nodes
- `jupyterhub-2.3.1.tar.gz` available in `files`
- Group `jupyterhub_server` defined in the Ansible inventory
- Certificate files `{{ certificates_private_key }}` and `{{ certificates_public_key }}` present on host
- Admin access to a KDC with the `realm`, `kadmin_principal` and `kadmin_password` role vars provided
- If using yarnspawner (default), jupyterhub_server needs to be an `hadoop_client`
- A functioning spark 3 deployment and spark configuration files at `$SPARK_HOME` of the `yarn_nm` hosts
- The `{{ jupyterhub_user }}` (default `jupyterhub`) has to be added to the list of proxy users in Hadoop's `core-site.xml` trough the `core_site` variable:

```yaml
# This section can be added to tdp_vars/hadoop/hadoop.yml
# This should be done automatically at tdp init step
---
auth_to_local:
  jupyterhub:
    - RULE:[2:$1/$2@$0](jupyterhub/.*@{{ realm }})s/.*/jupyterhub/
core_site:
  hadoop.proxyuser.jupyterhub.groups: '*'
  hadoop.proxyuser.jupyterhub.hosts: '*'
```

## Example

The following hosts file and playbook are given as examples.

### Host file

```
[spark3_client:children]
yarn_nm

[hadoop_client:children]
jupyterhub_server

[jupyterhub_server:children]
master-03
```

# Current ISSUES

Currently, there is no delegation token fetch by the yarnspawner for the HIVE or HBASE service.
If you need access to the service, the easiest way is to use `kinit` from inside the yarn container trough
the jupyterlab cells or terminal.
