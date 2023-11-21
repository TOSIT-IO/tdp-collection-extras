# Ansible TDP Collection Extras

Ansible collection to deploy additionnal components of TDP. The components here are useful in a TDP installation but are not core to the project (ie: not part of the Hadoop ecosystem).

Please check the core [Ansible TDP Collection](https://github.com/TOSIT-IO/tdp-collection) repository for the core components.

## Available Roles

- `airflow`: deploys the Apache Airflow release (Airflow Webserver + Scheduler + Worker)
- `livy.server`: deploys Apache Livy Server
- `kafka`: deploys Kafka brokers and Kafka client

## Other services

- `zookeeper-kafka`: deploys Apache ZooKeeper dedicated to Kafka (version 3.5.9 by default). Note that this service is deployed to hosts member of the `[zk]` group and uses `tdp-collection`'s [zookeeper role](https://github.com/TOSIT-IO/tdp-collection/tree/master/roles/zookeeper).

## Example

For examples on how to run these roles, please check out the [TDP Getting Started](https://github.com/TOSIT-IO/tdp-getting-started).

## Contribute

Please follow the guidelines at [contributing](./docs/contributing.md) and respect the [code of conduct](./CODE_OF_CONDUCT.md).

## Contributors

- [leopaul36](https://github.com/leopaul36)
- [DanielJohnHarty](https://github.com/DanielJohnHarty)
- [Nuttymoon](https://github.com/Nuttymoon)
