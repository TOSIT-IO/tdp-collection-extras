# Ansible Hue TDP Extra

Currently the roles only supports the deployment of HA, SSL-enabled, Kerberos authenticated Hadoop clusters.

## Prerequisites

- `java-1.8.0-openjdk` and `krb5-workstation` installed on all nodes
- Hue release (`hue_dist_file` role variable) file available in `files`
- Group `hue_server` defined in the Ansible inventory
- Certificate files `{{ fqdn }}.key` and `{{ fqdn }}.pem` for every node available in `files`
- Admin access to a KDC with the `realm`, `kadmin_principal` and `kadmin_password` role vars provided
- The hue tarball must be present in the files directory. The tested version is [hue-release-4.11.0-2.0-cp38-cp38-manylinux2014_x86_64.tar.gz] for python3.8.
- The hue_user role must already exist on the target database where the hue desktop database will be created
- The LDAP confifugration (`ldapauth` path of variable file) needs to be adapted to your environment.
- All hue dependencies must exist on the target hue_server [check here for Hue dependencies](https://docs.gethue.com/administrator/installation/dependencies/)

## Notes

- The first user logged to the hue web-ui will be the hue admin user.
- Upon a successful deployment of hue, the default web-ui url is `https://<_hue_server_host_>:<hue_port>`
- `yarn_rm` and `hdfs_nn` *must* be defined in the ansible hosts file
- Errors resembling `The error was: ansible.errors.AnsibleUndefinedVariable: 'dict object' has no attribute 'ats'` are often related to a missing entry in the ansible hosts file.

## Example

The following hosts file and playbook are given as examples.

### Host file

```ini
[hue_server]
edge
```
