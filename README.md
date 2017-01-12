Stouts.collectd
==========

[![Build Status](http://img.shields.io/travis/Stouts/Stouts.collectd.svg?style=flat-square)](https://travis-ci.org/Stouts/Stouts.collectd)
[![Galaxy](http://img.shields.io/badge/galaxy-Stouts.collectd-blue.svg?style=flat-square)](https://galaxy.ansible.com/list#/roles/1960)

Ansible role which help you with:

* Install and setup [Collectd](https://collectd.org/)

#### Variables

```yaml
collectd_enabled: yes               # Enable the role
collectd_version: 5.5.2             # Set version

# version options
collectd_use_latest:  no            # Don't fix package version to collectd_version

# PPA options
collectd_ppa_source: 'ppa:collectd/collectd-5.5'

# Source options
collectd_from_source: no            # Build collectd from sources, instead of existing packages
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
collectd_default_plugins: [cpu, df, interface, load, memory, swap]
collectd_default_plugins_options:
  swap:
  - ReportByDevice false
  interface:
  - Interface lo
  - IgnoreSelected true

# Additional types
# format: { name: ..., value: ... }
collectd_types: []

# Collectd graphite options
collectd_write_graphite: no
collectd_write_graphite_options:    # Setup write_graphite (https://collectd.org/wiki/index.php/Plugin:Write_Graphite)
  Host: "{{inventory_hostname}}"
  Port: 2003
  Prefix: stats.
  # Postfix: .collectd
  Protocol: tcp
  AlwaysAppendDS: 'false'
  EscapeCharacter: _
  LogSendErrors: 'true'
  StoreRates: 'true'

# Setup logs
collectd_logpath:                   # If it is not empty, will be used logfile
collectd_loglevel: info
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

If you wish to express your appreciation for the role, you are welcome to send
a postcard to:

    Kirill Klenov
    pos. Severny 8-3
    MO, Istra, 143500
    Russia
