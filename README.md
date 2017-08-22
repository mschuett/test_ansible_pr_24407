# Vagrant test scenario for Ansible module zabbix_host

This is a test case for [ansible/ansible PR #24410](https://github.com/ansible/ansible/pull/24410) and [issue #24407](https://github.com/ansible/ansible/issues/24407).

The Vagrant configuration starts two VMs with CentOS and uses pip to install a given Ansible version.
Then the Ansible playbook is run to install PostgreSQL and Zabbix in VM `zabbix` (using roles from Ansible Galaxy).
Finally two `zabbix_host` modules (from `./library`) are used to add both VMs as Zabbix hosts.

The current `zabbix_host_ansible_devel` is a copy of https://github.com/ansible/ansible/blob/devel/lib/ansible/modules/monitoring/zabbix_host.py

The current `zabbix_host_pr_24407` is a copy of the PR file: https://github.com/mschuett/ansible/blob/4de03ddcdb4f1896f5d650ee4abdc1beee064534/lib/ansible/modules/monitoring/zabbix_host.py
