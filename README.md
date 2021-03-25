# Ansible Role: ansible-apps_horus

## Description

[![Build Status](https://travis-ci.com/lotusnoir/ansible-apps_horus.svg?branch=master?style=flat)](https://travis-ci.com/lotusnoir/ansible-apps_horus)
[![License](https://img.shields.io/badge/license-Apache--2.0-brightgreen?style=flat)](https://opensource.org/licenses/Apache-2.0)
[![Ansible Role](https://img.shields.io/badge/galaxy-apps_horus-purple?style=flat)](https://galaxy.ansible.com/lotusnoir/apps_horus)
![GitHub repo size](https://img.shields.io/github/repo-size/lotusnoir/ansible-apps_horus?color=orange&style=flat)
![Ansible Quality Score](https://img.shields.io/ansible/quality/52300)
[![downloads](https://img.shields.io/ansible/role/d/52300)](https://galaxy.ansible.com/lotusnoir/apps_horus)
[![Version](https://img.shields.io/github/release/lotusnoir/ansible-apps_horus.svg)](https://github.com/lotusnoir/ansible-apps_horus/releases/)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=lotusnoir_ansible-apps_horus&metric=alert_status)](https://sonarcloud.io/dashboard?id=lotusnoir_ansible-apps_horus)
[![Maintainability Rating](https://sonarcloud.io/api/project_badges/measure?project=lotusnoir_ansible-apps_horus&metric=sqale_rating)](https://sonarcloud.io/dashboard?id=lotusnoir_ansible-apps_horus)
[![Reliability Rating](https://sonarcloud.io/api/project_badges/measure?project=lotusnoir_ansible-apps_horus&metric=reliability_rating)](https://sonarcloud.io/dashboard?id=lotusnoir_ansible-apps_horus)
[![Security Rating](https://sonarcloud.io/api/project_badges/measure?project=lotusnoir_ansible-apps_horus&metric=security_rating)](https://sonarcloud.io/dashboard?id=lotusnoir_ansible-apps_horus)

Deploy [horus](https://github.com/kosctelecom/horus) monitoring system using ansible.

## Requirements

You need to have golang installed locally in order to build the sources.

## Role variables

| Name                               | Default Value  | Description                        |
| ---------------------------------- | -------------- | -----------------------------------|
| `horus_src_dir`                    | /tmp/horus     | temp folder to build sources |
| `horus_log_dir`                    | /var/log/horus | log file location|
| `horus_agent_debug`                | 0              | 1 is debug mode |
| `horus_agent_port`                 | 80             | listen port |
| `horus_agent_snmp_jobs`            | 100            | concurent snmp jobs  |
| `horus_agent_stat_freq`            | 30             | stats refresh seconds |
| `horus_agent_prom_max_age`         | 600            | max age for metrics to keep |
| `horus_agent_kafka_host`           | ""             | kafka host |
| `horus_agent_kafka_topic`          | horus          | kafka topic |
| `horus_agent_kafka_partition`      | 1              | kafka partition number |
| `horus_agent_poll_delay`           | 250            | delay between each metrics poll |
| `horus_agent_fping_max_procs`      | 40             | max process for pings requests |
| `horus_agent_fping_pks_count`      | 15             | paquets count for pings requests |
| `horus_dispatcher_port`            | 81             | listen port |
| `horus_dispatcher_loglevel`        | 0              | 1 for debug mode |
| `horus_dispatcher_snmp_freq`       | 20             |  |
| `horus_dispatcher_ping_freq`       | 15             |  |
| `horus_dispatcher_keepalive_freq`  | 15             |  |
| `horus_dispatcher_max_delta`       | 0.25           |  |
| `horus_dispatcher_error_retention` | 2              | numbers of retention days for polling errors |
| `horus_dispatcher_ping_batch`      | 100            | numbers of concurent ping requests |
| `horus_psql_user`                  | horus          | bdd user |
| `horus_psql_pass`                  | strongpass     | bdd password |
| `horus_psql_host`                  | localhost      | bdd hostname |
| `horus_psql_database`              | horus_prod     | bdd database |
| `horus_type`                       | []             | install agent / query / dispatcher |
| `horus_version`                    | ""             | install a specific version, latest by default |

## Examples 

### install an agent:

	---
	- hosts: apps_horus
	  become: yes
	  become_method: sudo
	  gather_facts: yes
	  roles:
	    - role: ansible-apps_horus
	  vars:
        horus_agent_kafka_host:             "{{ groups['kafka_sys'] | join(',') }}"
        horus_agent_kafka_topic:            horus
        horus_agent_kafka_partition:        "{{ ansible_fqdn.split('0')[1][:1] }}"
        horus_type:
          - agent
          - query
	  environment: 
	    http_proxy: "{{ http_proxy }}"
	    https_proxy: "{{ https_proxy }}"
	    no_proxy: "{{ no_proxy }}"

### install a dispatcher:

	---
	- hosts: apps_horus
	  become: yes
	  become_method: sudo
	  gather_facts: yes
	  roles:
	    - role: ansible-apps_horus
	  vars:
        horus_psql_user: horus
        horus_psql_pass: strongpassword
        horus_psql_host: 10.64.38.201
        horus_psql_database: horus_prod
        horus_type:
          - dispatcher
        horus_version: 
          dispatcher: "v1.4.0"
	  environment: 
	    http_proxy: "{{ http_proxy }}"
	    https_proxy: "{{ https_proxy }}"
	    no_proxy: "{{ no_proxy }}"


## License

This project is licensed under Apache License. See [LICENSE](/LICENSE) for more details.
