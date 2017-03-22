# InfluxDB Enterprise

This role will install an InfluxDB Enterprise cluster. Both meta and data nodes.

## Usage

Add the following to your `requirements.yml`:

```
- src: influxdata.influxdb-enterprise
  name: influxdb-enterprise
```

Install to your roles directory:

` $ ansible-galaxy install -r requirements.yaml`

Include in your Playbooks:

```
---
# site.yml

- hosts: influxdb
  become: yes

  roles:
  - { role: 'influxdb-enterprise', influx_node_type: meta }
  - { role: 'influxdb-enterprise', influx_node_type: data }

  vars:
  influx_cluster_auto_join: true
  influx_meta_cluster_leader: influxdb_001
  influx_enterprise_license_key: XXX-XXX-XXX
```

# Prerequisites

* InfluxDB Enterprise License Key, a free trial can be obtained [here](https://www.influxdata.com/products/)
* Ansible, see [getting started](https://www.ansible.com/get-started) for more information.
* Vagrant (for testing and evaluation only)

# Contributing

Pull requests welcome!

A full test suite can be executed using via the following commands.

```
 $ export INFLUX_ENTERPRISE_LICENSE_KEY=XXX-XXX-XXX
 $ vagrant up
 $ ansible-playbook tests/cluster.yml -vvvv
```
