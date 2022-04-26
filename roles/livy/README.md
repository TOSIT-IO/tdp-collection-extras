# Ansible Livy TDP Extra

Deploys Apache Livy servers to the `livy_server` Ansible group.

## Prerequisites

- `java-1.8.0-openjdk` and `krb5-workstation` installed on all nodes
- `apache-livy-0.7.1-incubating-bin.zip` available in `files`
- Group `livy_server` defined in the Ansible inventory
- Certificate of the CA available as `root.pem` in `files`
- Certificate files `{{ fqdn }}.key` and `{{ fqdn }}.pem` available in `files`
- Admin access to a KDC with the `realm`, `kadmin_principal` and `kadmin_password` role vars provided
- A functioning spark deployment and spark configuration files at `$SPARK_HOME` of the `livy_server` hosts
- The `livy_user` (default `livy`) has to be added to the list of proxy users in Hadoop's `core-site.xml` trough the `core_site` variable:

  ```yaml
  # This section can be added to group_vars/all
  core_site:
    hadoop.security.auth_to_local: |
      RULE:[2:$1/$2@$0]([ndj]n/.*@{{ realm }})s/.*/hdfs/
      RULE:[2:$1/$2@$0]([rn]m/.*@{{ realm }})s/.*/yarn/
      RULE:[2:$1/$2@$0](jhs/.*@{{ realm }})s/.*/mapred/
      RULE:[2:$1/$2@$0](hive/.*@{{ realm }})s/.*/hive/
      RULE:[2:$1/$2@$0](livy/.*@{{ realm }})s/.*/livy/
      DEFAULT
    hadoop.proxyuser.livy.hosts: "*"
    hadoop.proxyuser.livy.groups: "*"
  ```

## Example

The following hosts file and playbook are given as examples.

### Host file

```
[spark_client:children]
livy_server

[livy_server]
edge-01
```

### Available Playbooks

- [livy.yml](../../playbooks/livy.yml):

  - A-Z deployment of Livy server

## TODO

- Please check out the [Livy related issues](https://issues.apache.org/jira/projects/LIVY/issues/LIVY-795?filter=allopenissues).
