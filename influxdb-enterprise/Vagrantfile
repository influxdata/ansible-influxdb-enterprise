# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = ENV["VAGRANT_VM_BOX"] ||  "ubuntu/trusty64"

  BOX_COUNT      = ENV["VAGRANT_BOX_COUNT"] ? ENV["VAGRANT_BOX_COUNT"].to_i : 1
  MEMORY         = ENV["VAGRANT_MEMORY"] || 256
  CPUS           = ENV["VAGRANT_CPUS"] || 1
  META_BIND_PORT = 8091
  DATA_BIND_PORT = 8086

  leader = ""
  members = Array.new

  (1..BOX_COUNT).each do |machine_id|
    primary      = machine_id == 1 ? true : false
    host_postfix = "%03d" % machine_id
    hostname     = "testnode-#{host_postfix}"
    leader       = hostname if machine_id == 1

    members << hostname

    config.vm.define hostname, primary: primary do |machine|
      machine.vm.hostname = hostname
      machine.vm.network "private_network", type: "dhcp"
      machine.vm.network "forwarded_port", guest: DATA_BIND_PORT, host: DATA_BIND_PORT if primary
      machine.vm.network "forwarded_port", guest: META_BIND_PORT, host: META_BIND_PORT if primary
      machine.vm.provider "virtualbox" do |v|
        v.memory = MEMORY
        v.cpus   = CPUS
      end

      machine.vm.provision "ansible" do |ansible|
        ansible.verbose           = 'vv'
        ansible.limit             = 'all'
        ansible.playbook          = "tests/vagrant.yml"
        ansible.sudo              = true
        ansible.raw_arguments     = ENV["ANSIBLE_RAW_ARGS"] || nil
        ansible.host_key_checking = false
        ansible.groups            = {"influx-nodes" => members}

        ansible.extra_vars = {
          influx_enterprise_license_key: ENV['INFLUX_ENTERPRISE_LICENSE_KEY'],
          influx_meta_cluster_leader: leader,
        }
      end if machine_id == BOX_COUNT # Run Ansible, only once the final machine is configured
    end
  end
end
