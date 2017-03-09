# InfluxDB Enterprise

This role will install an InfluxDB Enterprise cluster. Both meta and data nodes.

# Usage

```
---
# influxdb-enterprise.yaml
hosts: influx-servers
roles:
    - common
    - { role: influxdb-enterprise, influx_enterprise_license_key: XXX-XXX-XXX }
```

# Prerequisites

* InfluxDB Enterprise License Key, a free trial can be obtained [here](https://www.influxdata.com/products/)
* Ansible, see [getting started](https://www.ansible.com/get-started) for more information.
* Vagrant (for testing and evaluation only)

# Contributing

Just send us a pull request!

A full test suite can be executed using via the following commands.

```
 $ vagrant up INFLUX_ENTERPRISE_LICENSE_KEY=XXX-XXX-XXX
 $ ansible-playbook tests/cluster.yml -vvvv
```
