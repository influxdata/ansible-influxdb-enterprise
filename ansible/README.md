InfluxEnterprise Ansible Roles
==============================

This repository is for deploying InfluxEnterprise using Ansible.

Requirements
------------

The only prerequisites for these roles are:

* These must be deployed with >= 3 servers, and no less.

* They assume meta nodes and data nodes are under a `meta` and `data`
  node inventory group (the same servers can be listed under each, see
  below for example).

* A license key or license path variable must be set for each role
  prior to deployment.
  
* The hostnames or IP's used for each node must be reachable from
  every node in the inventory (ex: if you use the server name
  `influx1`, then all servers in the inventory must be able to reach
  that server as `influx1`).

Role Variables
--------------

The most important variables for these roles are:

* `influx_enterprise_license_key` - License key, retrievable from
  portal.influxdata.com. If using this variable, you only need to set
  it once (it is assumed to be shared by all nodes in the cluster) and
  you do not need to set the license path below.

* `influxdb_[meta|data]_license_path` - Path to a license file,
  retrievable from portal.influxdata.com and must be set in both meta
  and data configurations (if used). **This variable is required for
  offline installations.**

* `influxdb_[meta|data]_overwrite_hosts` - If set to `yes`, this will
  overwrite the local `/etc/hosts` file, mapping the inventory
  hostnames to the IP addresses of the nodes. This is useful for
  environments without DNS propagation or local testing (defaults to
  `yes` when using Vagrant).

Dependencies
------------

There are currently no external dependencies.

Example Playbook
----------------

Included in each repository is the `cluster-test.yml` playbook, which
will install and configure InfluxEnteprise for a provided inventory
(assuming the license keys are already set):

```
- hosts: all
  roles:
    - role: influxdb-meta
    - role: influxdb-data
```

**Note: meta nodes must be deployed prior to data nodes.**

Again, the inventory must have two specific groups: `data` and
`meta`. An example inventory may look like:

```
[meta]
influx1
influx2
influx3

[data]
influx1
influx2
influx3
```

The recommended method for testing is with vagrant, using the included
`Vagrantfile`'s.

License
-------

BSD
