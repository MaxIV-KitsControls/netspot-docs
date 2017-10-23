# NetSPOT CLI

## Assets

### Add new device

Login to the automation server:

    ssh automation@w-v-netconf-0

Then run:

    ./nsclient -a ASSET_NAME -l LOOPBACK_IP -r GROUP(S)

Where DEVICE_NAME is the hostname, LOOPBACK_IP is the IP to connect to and GROUP(S) is a list of groups that the device belongs to, eg. *white* for a single group or *white,lab* for multiple groups.

This will add the device to the inventory database. This can be verified by the web gui at netspot.maxiv.lu.se

Once the device is in the database we need to enable the automation SSH key to the device. Run this (please add a space before the command to have to command excluded from the CLI history):

    export HISTCONTROL=ignorespace
    ANSIBLE_NET_USERNAME=XXXX ANSIBLE_NET_PASSWORD=XXXXX FILTER=ASSET_NAME ansible-playbook -i ./spotmax/nsinv.py  network_playbooks/junos_add_automation_user.yml

That's it! If you want to test it you can do so by taking a config backup:

    FILTER=ASSET_NAME ansible-playbook -i ./spotmax/nsinv.py  network_playbooks/junos_config_backup.yml

### Discover device

This process is run when the device is added to the database but should be run on a regular basis to keep the database updated. This will update information (serial number, model, software version, interfaces etc).

    ./nsclient -y DEVICE_NAME

### Delete device

    ./nsclient -d DEVICE_NAME

This will delete the device from the database.

## Groups

### Add new group

    ./nsclient.py -g -a GROUP_NAME

### Add new group variable

    ./nsclient.py -g -z GROUP_NAME VARIABLE:VALUE

Where GROUP_NAME is the name of the group. VARIABLE:VALUE is the variable and it's value eg. *ntp_server:192.168.45.23*.

### Delete group variable

    ./nsclient.py -g -x GROUP VARIABLE:VALUE

Where GROUP_NAME is the name of the group. VARIABLE:VALUE is the variable and it's value eg. *ntp_server:192.168.45.23*.