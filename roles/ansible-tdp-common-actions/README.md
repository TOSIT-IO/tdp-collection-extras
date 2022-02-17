# ansible-tdp-common-actions

This role comproses a set of tasks commonly applied accross a TDP-getting-started cluster.

# Example playbook example

```yaml
- hosts: all
  tasks:
    - name: sync hosts file
      import_role:
        name: ansible_roles/roles/ansible-tdp-common-actions
        tasks_from: update_hosts_file
```

# Modifying xml config

The `modify-xml-config.yml` file can be used to add to or modify an element in an xml file on a remote server. The file is backed up with timestamp before making any changes.

The below example creates a new `/configuration/property` element with a name-subelement='abc.def.ghi' and a value-subelement='rst.uvw.xyz'.

```
- hosts: "{{ groups['hdfs_nn'] }}"
  tasks:
    - name: Update core-site.xml
      vars:
        - element_parent_tag: property
        - element_name: abc.def.ghi
        - element_value: rst.uvw.xyz
        - target_xml_filepath: /etc/hadoop/conf/core-site.xml
        - existence_check_xpath: //name[text()='{{ element_name }}']/parent::property
        - added_child_root_xpath: /configuration
      import_role:
        name: ansible_roles/collections/ansible_collections/tosit/tdp-extra/roles/ansible-tdp-common-actions
        tasks_from: modify-xml-config
```
