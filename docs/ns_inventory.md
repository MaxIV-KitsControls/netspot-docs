# NetSPOT Inventory

## General

The NetSPOT NS inventory script; nsinv.py is used to grab data from the NetSPOT database and return a JSON output that can be read by Ansible Playbooks. Due to Ansible we have to pass arguments as shell enviorment variables. SSH to the automation server and run the commands below.

    ssh automation@w-v-netconf-0

### Filters

Filters enables you to run a playbook on a given set of devices. The filters can specify almost any field in the NetSPOT database. Below are some examples:

Get all devices in group "white":

    FILTER=groups:white ./nsinv.py --list

Get all devices that are EX4200:

    FILTER=model:EX4200 ./nsinv.py --list

This will return all EX4200 devices including EX4200-XX.

Get all devices running JUNOS 14.1:

    FILTER=version:14.1 ./nsinv.py --list

### Pass nsinv.py to Ansible playbooks

To get an Ansible playbook to use the data from nsinv.py:

    $ FILTER=MY_FILTER ansible-playbook -i nsinv.py MY_PLAYBOOK

Where MY_FILTER is you filter (eg. groups:white) and MY_PLAYBOOK is the name of your playbook eg. update_ntp_servers.yml.