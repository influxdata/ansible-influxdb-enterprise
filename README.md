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

Include in your Playbooks (see [this example](https://github.com/influxdata/ansible-influxdb-enterprise/blob/master/tests/vagrant.yml) for a more detailed usage):

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
  influx_queries:
    - "CREATE DATABASE test"
    - "CREATE RETENTION POLICY testrp ON test DURATION 24h REPLICATION 2 default"
    - "ALTER RETENTION POLICY autogen ON test DURATION 666h REPLICATION 2 default"
```

_Note: When bringing up the cluster for the first time, it's a good idea to use `--skip-tags=influxdb-cluster`, this ensures all hosts are up before establishing a cluster. It's also wise to run against the first meta-node first using the `--limit` option. We'll improve this in the future._

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
