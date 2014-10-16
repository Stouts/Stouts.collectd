Stouts.collectd
==========

[![Build Status](https://travis-ci.org/Stouts/Stouts.collectd.png)](https://travis-ci.org/Stouts/Stouts.collectd)
[![Stories in Ready](https://badge.waffle.io/Stouts/Stouts.collectd.svg?label=ready&title=Ready)](http://waffle.io/Stouts/Stouts.collectd)

Ansible role which help you with:

* Install and setup [Collectd](https://collectd.org/)

#### Variables

```yaml
collectd_enabled: yes               # Enable the role
collectd_version: 5.4.1             # Set version
collectd_prefix: /opt/collectd      # The place where Collectd will be installed

# General options
collectd_interval: 10
collectd_readthreads: 7
collectd_hostname: "{{ inventory_hostname }}"
collectd_fdqnlookup: false

# Collectd plugins
collectd_plugins: []                # Ex. [nginx, memcached]
collectd_plugins_options: {}        # See below for examples.

# Collectd default plugins
collectd_default_plugins: [logfile, cpu, df, disk, interface, load, memory, swap, vmem, protocols]
collectd_default_plugins_options:
  logfile:
  - LogLevel "info"
  - File "{{collectd_logpath}}"
  - Timestamp true
  vmem:
  - Verbose false
  swap:
  - ReportByDevice false
  protocols:
  - Value "/^Tcp:/"

# Collectd graphite options
collectd_write_graphite: no
collectd_write_graphite_options:    # Setup write_graphite (https://collectd.org/wiki/index.php/Plugin:Write_Graphite)
  Host: "{{inventory_hostname}}"
  Port: 2003
  # Prefix: "{{inventory_hostname}}"
  # Postfix: .collectd
  # Protocol: tcp
  AlwaysAppendDS: 'false'
  EscapeCharacter: _
  LogSendErrors: 'true'
  StoreRates: 'true'

# Setup logs
collectd_logpath: /var/log/collectd.log
collectd_logrotate: yes
collectd_logrotate_options:
  - compress
  - copytruncate
  - daily
  - dateext
  - rotate 7
  - size 10M
```


#### Usage

Add `Stouts.collectd` to your roles and set vars in your playbook file.

Example:

```yaml

- hosts: all

  roles:
    - Stouts.collectd

  vars:
    collectd_write_graphite: yes
    collectd_plugins: [nginx]
    collectd_write_graphite_options:
      Host: localhost
      LogSendErrors: 'true'
```

#### License

Licensed under the MIT License. See the LICENSE file for details.

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Stouts/Stouts.collectd/issues)!
