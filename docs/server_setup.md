# Server setup

This document describes how you setup NetSPOT.

## Setup NetSPOT

* Do a git clone in your web server directory.
* Copy *netspot/netspot_settings_example.py* to *netspot/netspot_settings.py*
* Edit *netspot/netspot_settings.py* and fill in the blanks.
* Configure your webserver to start serve NetSPOT.
  * Hint: gunicorn is a good choice to serve a Django project like NetSPOT

## Disable / enable LDAP integration

By default NetSPOT uses LDAP for authentication. Settings for LDAP can be found in *netspot_settings.py*.

If you want to disable LDAP please remove LDAP from AUTHENTICATION_BACKENDS:

        AUTHENTICATION_BACKENDS = ['django_auth_ldap.backend.LDAPBackend', 'django.contrib.auth.backends.ModelBackend']

## Setup NetSPOT internal datbase

NetSPOT uses an internal database to keep some data. This needs to be setuped.

Enter the netspot directory (the one with manage.py in it) and run the following commands:

    $ python manage.py makemigrations netspot
    $ python manage.py sqlmigrate netspot 0001
    $ python manage.py migrate

Now create a super user:

    $ python manage.py createsuperuser

If your webserver is setup you can now login in to netspot and login using the super user you just setup.

## Setup cronjobs 

NetSPOT relies on a few cron jobs to collect data from the network devices. You need at least two jobs:

Below is an example of how collect MAC/ARP entries every 3 hour. This will collect from all devices in the "white" group.

    0 */3 * * * FILTER=groups:white /usr/bin/ansible-playbook -i /usr/share/nginx/netspot/netspot/lib/spotmax/nsinv.py /usr/share/nginx/netspot/netspot/lib/network_playbooks/junos_macarp.yml

The other job you need is a discovery job. The example below dicsover nodes in the group "white".

    0 */6 * * * FILTER=groups:white /usr/bin/ansible-playbook -i /usr/share/nginx/netspot/netspot/lib/spotmax/nsinv.py /usr/share/nginx/netspot/netspot/lib/network_playbooks/ns_discover.yml

## Setup ansible_runner

The *ansible_runner* is repsonsible for managing the Ansible job queue. This should be run as a service on the server. Here's an example of a service file:

File location: */etc/systemd/system/ansible_runner.service*

    [Unit]
    Description=Ansible Runner
    After=network.target

    [Service]
    User=automation
    Group=nginx
    Restart=on-failure
    Environment="PYTHONPATH=/usr/share/nginx/netspot/netspot/lib/:/usr/share/nginx/netspot/"
    WorkingDirectory=/usr/share/nginx/netspot/netspot/lib/ansible_runner
    ExecStart=/usr/share/nginx/netspot/netspot/lib/ansible_runner/ansible_runner.py

    [Install]
    WantedBy=multi-user.target

Make sure all paths are OK. Then run:

    sudo systemctl start ansible_runner
    sudo systemctl enable ansible_runner
    sudo systemctl status ansible_runner

Troubleshoot by looking at the logs:

    sudo journalctl -f -u ansible_runner

### Ansible callback (important)

In order to log all Ansible runs through the *ansible_runner* NetSPOT uses a callback funtions. This needs to be configured in *ansible.cfg*. This is an example of how to do globally on the server:

E.g. */etc/ansible/ansible.cfg*

Change this line:

    callback_plugins   = /usr/share/ansible/plugins/callback

Add the callback in the NetSPOT path eg:

    callback_plugins   = /usr/share/nginx/netspot/netspot/lib/ansible_runner/callbacks:/usr/share/ansible/plugins/callback

The Ansible log in NetSPOT will be empty without this callback. You might also see "Ansible Error" when you schedule a job.

## Troubleshooting

To see what the *ansible_runner* is up to you can issue the following command:

    sudo journalctl -u ansible_runner

If you have a job that's stuck in the job queue you can delete it by going to this URL:

    https://netspot.maxiv.lu.se/deletetask/109. (replace 109 with the failed job ID)

# Update

Run the update Ansible playbook:

    ansible-playbook netspot/lib/network_playbooks/update_netspot.yml --ask-sudo-pass

This will stop Nginx, gunicorn, git pull NetSPOT, git pull docs and start Nginx and gunicorn.

