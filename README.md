# Ansible Role: ansible-apps_horus


## Description

[![Build Status](https://travis-ci.com/lotusnoir/ansible-apps_horus.svg?branch=master)](https://travis-ci.com/lotusnoir/ansible-apps_horus)[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)[![Ansible Role](https://img.shields.io/badge/ansible%20role-apps__horus-blue)](https://galaxy.ansible.com/lotusnoir/ansible-apps_horus/)[![GitHub tag](https://img.shields.io/badge/version-latest-blue)](https://github.com/lotusnoir/ansible-apps_horus/tags)

Deploy [horus](https://github.com/kosctelecom/horus) monitoring system using ansible.

## Requirements

You need to have golang installed locally in order to build the sources.

## Role variables

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `horus_src_dir` | /tmp/horus |  |
| `horus_log_dir` | /var/log/horus |  |
| `horus_agent_debug` | 1 |  |
| `horus_agent_port` | 80 |  |
| `horus_agent_snmp_jobs` | 100 |  |
| `horus_agent_stat_freq` |  | 30 |
| `horus_agent_prom_max_age` | 600 |  |
| `horus_agent_kafka_host` | "" |  |
| `horus_agent_kafka_topic` | horus |  |
| `horus_agent_poll_delay` | 250 |  |
| `horus_agent_fping_max_procs` | 40 |  |
| `horus_agent_fping_pks_count` | 15 |  |
| `horus_dispatcher_port` | 81 |  |
| `horus_dispatcher_snmp_freq` | 20 |  |
| `horus_dispatcher_ping_freq` | 15 |  |
| `horus_dispatcher_keepalive_freq` | 15 |  |
| `horus_dispatcher_max_delta` | 0.25 |  |
| `horus_dispatcher_error_retention` | 2 | numbers of retention days for polling errors |
| `horus_dispatcher_ping_batch` | 100 |  |
| `horus_psql_user` | horus |  |
| `horus_psql_pass` | postgresqlpassword |  |
| `horus_psql_host` | localhost |  |
| `horus_psql_database` | horus_prod |  |
| `horus_type` | [] | install agent / query / dispatcher |
| `horus_version` | "" | install a specific version, latest by default |

## Examples

	---
	- hosts: apps_horus
	  become: yes
	  become_method: sudo
	  gather_facts: yes
	  roles:
	    - role: ansible-apps_horus
	  vars:
        horus_type:
          - agent
          - query
	  environment: 
	    http_proxy: "{{ http_proxy }}"
	    https_proxy: "{{ https_proxy }}"
	    no_proxy: "{{ no_proxy }}


## License

This project is licensed under MIT License. See [LICENSE](/LICENSE) for more details.
